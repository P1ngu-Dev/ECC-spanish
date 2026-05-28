# Respuesta a incidentes de supply chain

Este playbook es el runbook de operador de ECC para incidentes de npm, GitHub
Actions y registries de paquetes cross-ecosystem. Es intencionalmente
conservador: las firmas del registry, la procedencia y trusted publishing son
señales útiles, pero no prueban que el workflow ejecutó la ruta de código
pretendida.

## Disparador externo actual

A fecha de 2026-05-15, la clase activa de incidente es el compromiso de supply
chain npm de TanStack de mayo de 2026 y la campaña Mini Shai-Hulud más amplia.
ECC mantiene el mismo barrido IOC para las olas relacionadas de npm/PyPI porque
estos incidentes atacan rutas de instalación/publicación de paquetes,
configuraciones de herramientas de desarrollo AI y credenciales de desarrollo:

- TanStack informó 84 versiones maliciosas en 42 paquetes `@tanstack/*`,
  publicadas el 2026-05-11 entre las 19:20 y las 19:26 UTC.
- El advisory de GitHub `GHSA-g7cv-rxg3-hmpx` / `CVE-2026-45321` describe un
malware en tiempo de instalación que recolecta credenciales cloud, tokens de
GitHub, credenciales npm, tokens de Vault, tokens de Kubernetes y claves
privadas SSH.
- El reporting posterior de StepSecurity, Socket, Aikido y Wiz describe la
misma campaña expandiéndose a paquetes asociados con Mistral AI, UiPath,
OpenSearch, Guardrails AI, Squawk y otros paquetes npm/PyPI.
- El reporte de `node-ipc` de Socket del 2026-05-14 describe un compromiso npm
activo separado que afecta a las versiones `9.1.6`, `9.2.3` y `12.0.1` de
`node-ipc`, con versiones maliciosas históricas de `node-ipc` también bloqueadas
por ECC porque incluían comportamiento destructivo o no autorizado de escritura
de archivos.
- El set de IOC en vivo incluye persistencia mediante `.claude/settings.json` de
Claude Code, `.vscode/tasks.json` de VS Code, `.zed/tasks.json` de Zed y
servicios `gh-token-monitor` a nivel de OS en LaunchAgent/systemd. Algunas
variantes añaden `~/.config/gh-token-monitor/token` más una descripción de token
de dead-man-switch `IfYouRevokeThisTokenItWillWipeTheComputerOfTheOwner`,
archivos de workflow maliciosos como `.github/workflows/codeql_analysis.yml` y
payloads de runtime de Python como `transformers.pyz` / `pgmonitor.py`. Retira
esos hooks de persistencia antes de rotar un token de GitHub robado.
- El scanner también vigila marcadores de reporte tardío: prefijo/sufijo SHA-256
`ab4fcada...8601266c` de `router_init.js`, prefijo/sufijo SHA-256
`2ec78d55...6be27fc96` de `tanstack_runner.js`, `opensearch_init.js`,
`vite_setup.mjs`, la sal `svksjrhjkcejg` de la campaña, strings de protocolo de
Session, commits dead-drop `claude@users.noreply.github.com`, nombres de rama
`dependabout/` y `OhNoWhatsGoingOnWithGitHub`.
- El sweep de `node-ipc` vigila el hash del payload `node-ipc.cjs`
  `96097e06...d9034144`, hashes de tarball de los artefactos maliciosos `9.1.6`,
  `9.2.3` y `12.0.1`, `sh.azurestaticprovider.net`, `bt.node.js`, `37.16.75.69`,
  labels DNS de exfiltración `xh` / `xd` / `xf` cuando están presentes en los
  artefactos, `__ntw`, `__ntRun`, archivos temporales `/nt-` y entradas de
  archivo como `uname.txt`, `envs.txt` y `fixtures/_paths.txt`.
- La cadena de ataque combinó `pull_request_target`, poisoning de GitHub Actions
  cache a través de la frontera de confianza fork/base y extracción de tokens
  OIDC desde un runner de GitHub Actions.
- npm trusted publishing/provenance puede confirmar que un paquete provino de
  una identidad de CI vinculada. Por sí solo no puede probar que el cache de CI,
  los lifecycle scripts o la ruta de publicación fueran seguros.

Referencias primarias:

- <https://tanstack.com/blog/npm-supply-chain-compromise-postmortem>
- <https://github.com/advisories/GHSA-g7cv-rxg3-hmpx>
- <https://tanstack.com/blog/incident-followup>
- <https://www.wiz.io/blog/mini-shai-hulud-strikes-again-tanstack-more-npm-packages-compromised>
- <https://socket.dev/blog/node-ipc-package-compromised>
- <https://docs.npmjs.com/trusted-publishers/>
- <https://www.cisa.gov/news-events/alerts/2025/09/23/widespread-supply-chain-compromise-impacting-npm-ecosystem>

## Comprobación de exposición en ECC

Ejecuta esto antes de un release candidate, después de un bump amplio de
dependencias y después de cualquier incidente de registry de paquetes.

```bash
npm run security:ioc-scan
node scripts/ci/scan-supply-chain-iocs.js --home
npm ci --ignore-scripts
npm audit signatures
npm audit --audit-level=high
node scripts/ci/supply-chain-advisory-sources.js --json
node scripts/ci/validate-workflow-security.js
node tests/scripts/npm-publish-surface.test.js
node tests/run-all.js
```

Si un resultado de búsqueda aparece solo en ejemplos de documentación, anótalo
en la evidencia de release pero no rotees credenciales por una referencia solo de
docs.

## Flujo de vigilancia duradero

ECC también ejecuta `.github/workflows/supply-chain-watch.yml` cada seis horas y
con dispatch manual. El workflow es solo lectura, desactiva la persistencia de
credenciales del checkout, instala con `npm ci --ignore-scripts`, verifica
firmas del registry npm, ejecuta los fixtures del scanner IOC, ejecuta
`scripts/ci/supply-chain-advisory-sources.js --refresh --json`, emite
`supply-chain-ioc-report.json` y `supply-chain-advisory-sources.json`, y vuelve a
validar las reglas de hardening de GitHub Actions.

Trata un fallo del watch programado como un bloqueo de release hasta que un
operador confirme si el fallo es un advisory recién reportado, un fixture viejo
del scanner, un problema de firma del registry o una regresión de hardening del
workflow. Si el scanner necesita nuevos indicadores, actualiza
`scripts/ci/scan-supply-chain-iocs.js`, añade cobertura de fixture en
`tests/ci/scan-supply-chain-iocs.test.js`, refresca este runbook y adjunta el
último artefacto JSON a la evidencia de release.

El artefacto de advisory-source es el payload de estado ITO-57. Registra el
registry de fuente confiable, advertencias de refresco de URL en vivo y un
resumen listo para Linear. Refresca la cobertura de fuentes mediante
`npm run security:advisory-sources -- --json` antes de cambiar la cobertura IOC,
y adjunta el artefacto a la próxima actualización de estado del proyecto Linear
después de cada lote de merge significativo.

## Respuesta inmediata

Si ECC o una máquina de maintainer instaló una versión conocida como mala de un
paquete:

1. Detén al host para que no publique ni despliegue.
2. Conserva la evidencia antes de limpiar:
   - historial de comandos del package manager;
   - `package-lock.json`, `pnpm-lock.yaml` o `yarn.lock`;
   - URLs de runs de CI y logs del runner;
   - versiones npm del paquete y hashes de integridad del tarball;
   - logs de red de salida cuando estén disponibles.
3. Trata el host de instalación como comprometido si pudieron ejecutarse
   lifecycle scripts.
4. Elimina los hooks de persistencia antes de revocar tokens:
   - hooks `SessionStart` de `~/.claude/settings.json` y archivos payload adyacentes
     `router_runtime.js` / `setup.mjs`;
   - tareas folder-open de `.vscode/tasks.json` y archivos payload adyacentes;
   - `~/Library/LaunchAgents/com.user.gh-token-monitor.plist`;
   - `~/.config/systemd/user/gh-token-monitor.service`;
   - `~/.config/systemd/user/pgsql-monitor.service`;
   - `~/.config/gh-token-monitor/token`;
   - `~/.local/bin/gh-token-monitor.sh`;
   - `~/.local/bin/pgmonitor.py`;
   - `/tmp/transformers.pyz`, `/tmp/pgmonitor.py` y sus equivalentes
     `/private/tmp/` en macOS.
5. Rota cada credencial accesible para el proceso:
   - tokens de automatización npm y tokens de maintainer;
   - PATs de GitHub, tokens de fine-grained, deploy keys y secretos de Actions;
   - credenciales cloud, tokens de Vault, tokens de service-account de Kubernetes,
     claves SSH y tokens locales de `.npmrc`;
   - cualquier credencial MCP, plugin o harness disponible en variables de entorno
     o config de ámbito usuario.
6. Purga los caches de dependencias de GitHub Actions para los repos afectados.
7. Reinstala desde un entorno limpio con lifecycle scripts deshabilitados primero:
   `npm ci --ignore-scripts`, `pnpm install --ignore-scripts`,
   `yarn install --mode=skip-build` o `bun install --ignore-scripts`.
8. Vuelve a habilitar lifecycle scripts solo después de que el árbol de
   dependencias y las versiones de paquetes estén fijadas a releases conocidas
   como limpias.

## Reglas de GitHub Actions

ECC hace cumplir estas reglas mediante `scripts/ci/validate-workflow-security.js`:

- los workflows privilegiados no deben hacer checkout de refs PR no confiables;
- todas las instalaciones de dependencias del workflow deben deshabilitar los
  lifecycle scripts;
- los workflows no deben restaurar ni guardar caches compartidos de dependencias
  de GitHub Actions durante el hardening activo de supply chain;
- los workflows con `id-token: write` no deben restaurar ni guardar caches
  compartidos de dependencias;
- los workflows que ejecutan `npm audit` también deben ejecutar
  `npm audit signatures`;
- los workflows `pull_request_target` no deben restaurar ni guardar caches
  compartidos de dependencias.

Trata cualquier violación como bloqueo de release.

## Reglas de publicación

Antes de etiquetar o publicar ECC:

1. Verifica que no haya dependencia inesperada de paquetes en el advisory activo.
2. Usa un checkout limpio o un worktree desechable para los comandos de release.
3. No mezcles caches de PR/test con jobs de publicación.
4. Mantén `id-token: write` limitado a workflows de release que no usen caches
   compartidos de dependencias.
5. Prefiere trusted publishing/provenance donde esté soportado, pero sigue
   exigiendo tests locales de la superficie de paquete y verificación de firma
   del registry.
6. Confirma el estado de npm dist-tag, release de GitHub, plugin de Claude,
   plugin de Codex y paquete de OpenCode en el documento de evidencia de
   publication-readiness.

## Cuándo escalar

Escala a una revisión de seguridad de maintainer antes de cualquier release o
merge si:

- un lockfile de dependencia referencia un paquete nombrado en un advisory activo;
- `node scripts/ci/scan-supply-chain-iocs.js --home` encuentra indicadores de
  persistencia de Claude Code, VS Code, Zed o del nivel OS;
- un workflow combina `pull_request_target` con instalación de dependencias,
  restore/save de cache, checkout de PR head o permisos de escritura;
- un workflow de release combina `id-token: write` con uso de cache compartido;
- un workflow de publicación usa un token npm de larga duración sin una razón
  documentada;
- los checks de AgentShield, GitGuardian, Dependabot, npm audit o registry-signature
  no coinciden.
