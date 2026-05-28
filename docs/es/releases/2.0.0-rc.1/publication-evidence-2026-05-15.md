# Evidencia de publicación de ECC v2.0.0-rc.1 - 2026-05-15

Esto es solo evidencia de preparación para el lanzamiento. No crea un release de GitHub,
una publicación en npm, una etiqueta de plugin, una publicación en marketplace ni una publicación de anuncio.

## Commit de origen

| Campo | Evidencia |
| --- | --- |
| Base upstream de main | `1949d75e18e59a37de269d88b188fc701f5cf122` |
| Rama de evidencia | `codex/rc1-agentshield-86-evidence` |
| Alcance de la evidencia | `main` actual después de PR #1932, #1933, #1934, #1935 y #1936; AgentShield #86; y ECC-Tools #75 |
| Git remote | `https://github.com/affaan-m/everything-claude-code.git` |
| Advertencia sobre el estado local | El árbol de trabajo tenía el directorio no rastreado no relacionado `docs/drafts/` antes de esta actualización de docs |

El operador real del release debe repetir todas las verificaciones orientadas a publicación desde el
commit final de release con un checkout limpio antes de publicar.

## Estado de la cola y discusiones

| Superficie | Comando | Resultado |
| --- | --- | --- |
| PRs/issues del trunk | `gh pr list` y `gh issue list` para `affaan-m/everything-claude-code` | 0 PRs abiertos, 0 issues abiertas |
| PRs/issues de AgentShield | `gh pr list` y `gh issue list` para `affaan-m/agentshield` | 0 PRs abiertos, 0 issues abiertas |
| PRs/issues de JARVIS | `gh pr list` y `gh issue list` para `affaan-m/JARVIS` | 0 PRs abiertos, 0 issues abiertas |
| PRs/issues de ECC Tools | `env -u GITHUB_TOKEN gh pr list` y `env -u GITHUB_TOKEN gh issue list` para `ECC-Tools/ECC-Tools` | 0 PRs abiertos, 0 issues abiertas |
| PRs/issues del sitio de ECC | `env -u GITHUB_TOKEN gh pr list` y `env -u GITHUB_TOKEN gh issue list` para `ECC-Tools/ECC-website` | 0 PRs abiertos, 0 issues abiertas |
| Discusiones del trunk | Conteo de discusiones GraphQL y barrido de contacto de mantenedores | 58 discusiones en total; 0 sin contacto de mantenedores después de los comentarios de mantenedores del 15 de mayo |
| Discusiones en otros repos | Conteo de discusiones GraphQL para AgentShield, JARVIS, ECC Tools y ECC website | Discusiones deshabilitadas o 0 en total |
| Auditoría de plataforma | `node scripts/platform-audit.js --json --allow-untracked docs/drafts/` | Listo; PRs abiertos 0/20, issues abiertas 0/20, discusiones que requieren contacto de mantenedores 0, PRs abiertos en conflicto 0, archivos sucios bloqueantes 0 |

La organización ECC Tools es accesible con la credencial de GitHub host configurada.
En esta shell, el `GITHUB_TOKEN` exportado reemplaza esa credencial y provoca falsos errores 404/403 para `ECC-Tools/*`. Usa `env -u GITHUB_TOKEN`
para los comandos de verificación de ECC Tools hasta que se limpie esa anulación de entorno.

## Estado de la hoja de ruta en Linear

La hoja de ruta de ejecución detallada ahora vive en el proyecto de Linear:

<https://linear.app/itomarkets/project/ecc-platform-roadmap-52b328ee03e1>

El proyecto contiene 16 carriles a nivel de issue y 5 hitos:

| Hito | Issues |
| --- | --- |
| Base de seguridad y acceso | `ITO-44`, `ITO-57`, `ITO-58` |
| Vista previa y publicación de ECC 2.0 | `ITO-45`, `ITO-46`, `ITO-47`, `ITO-56` |
| Iteración empresarial de AgentShield | `ITO-48`, `ITO-49` |
| Plataforma de siguiente nivel para ECC Tools | `ITO-50`, `ITO-51`, `ITO-52`, `ITO-53`, `ITO-54`, `ITO-59` |
| Auditoría y rescate heredado | `ITO-55` |

Documentos del proyecto agregados en Linear:

- Índice de la hoja de ruta y base de ejecución actual
- Actualización de estado 2026-05-15
- Instantánea de la cola de GitHub 2026-05-15
- Instantánea de auditoría de finalización 2026-05-15
- Evidencia de la cola de discusiones 2026-05-15
- Evidencia de acceso a ECC-Tools 2026-05-15

## Evidencia de cadena de suministro

| Superficie | Evidencia |
| --- | --- |
| PR #1921 | Se fusionó la expansión IOC de cadena de suministro para el seguimiento de Mini Shai-Hulud/TanStack |
| Seguimiento de Node IPC / PR #1924 | Agregó cobertura IOC de versión maliciosa, hash, DNS y runtime de `node-ipc` del 14 de mayo |
| PR #1926 | Agregó superficies de comando `platform:audit` y `security-ioc-scan` junto con compuertas IOC del flujo de trabajo de release |
| PR #1932 | Agregó modos JSON/Markdown/salida-a-archivo para `scripts/platform-audit.js` para que la cola, las discusiones, la hoja de ruta y la evidencia de release puedan capturarse como un artefacto durable en lugar de solo salida de terminal |
| PR #1933 | Amplió la cobertura IOC de escaneo de home a `settings.local.json` de Claude, `.claude/hooks/hooks.json`, y `tasks.json` de VS Code / Code Insiders a nivel de usuario en macOS, Linux y Windows |
| PR #1934 | Cambió las cachés ordinarias de dependencias de CI a uso restore-only de `actions/cache/restore` para que los jobs de prueba no guarden estado mutable de dependencias en las cachés compartidas |
| PR #1935 | Estabilizó las pruebas de `ecc2` que mutan el directorio actual con una protección serializada del directorio actual solo para pruebas, preservando la compuerta de la superficie de release Rust bajo ejecución paralela de pruebas |
| PR #1940 | Agregó `.github/workflows/supply-chain-watch.yml`, programado cada 6 horas, para que el escaneo IOC de TanStack/Mini Shai-Hulud/node-ipc y las comprobaciones de firma/auditoría de npm produzcan un artefacto durable `supply-chain-ioc-report.json` |
| PR #1941 | Eliminó el uso de caché de dependencias de GitHub Actions de los flujos de trabajo de prueba de CI, deshabilitó los scripts de ciclo de vida del administrador de paquetes para instalaciones npm/pnpm/Yarn/Bun, purgó cachés existentes de Actions y agregó pruebas de validador que rechazan patrones inseguros de instalación/caché |
| PR #83 de AgentShield | Se fusionó la expansión IOC de Mini Shai-Hulud para TanStack, Mistral, OpenSearch, Guardrails, UiPath, Squawk, persistencia de Claude Code / VS Code y artefactos de dead-man switch |
| PR #84 de AgentShield | Se fusionó la tabla de paquetes afectados de la campaña completa de Mini Shai-Hulud, incluyendo scopes adicionales de paquetes `@cap-js`, `@draftlab`, `@tallyui`, `intercom-client`, `lightning` e IOCs relacionados |
| PR #85 de AgentShield | Agregó verificación de cadena de suministro en GitHub Action, compuerta y paquetes de evidencia para que la ruta de release del scanner empresarial de AgentShield tenga una superficie de firma de registro verificada |
| PR #86 de AgentShield | Agregó `ci-context.json` a los paquetes de evidencia de AgentShield con procedencia permitida de GitHub Actions workflow, commit, run y runtime, manteniendo variables de entorno y tokens arbitrarios fuera del bundle |
| PR #75 de ECC-Tools | Ajustó la compuerta nativa de anuncio de pagos de GitHub para que las afirmaciones públicas de facturación sigan bloqueadas hasta que el readback en vivo de la cuenta de prueba administrada por Marketplace esté listo |
| Commits de merge del trunk | `f04702bdac132662c8496e817bcd850c86e2b854`, `ee85e1482e3d6322ddb2706392ea0fc97469bd26`, `13585f1092c92fa3f20ffe0d756e40c5720b0de5`, `553d507ea63bc252e815a924c0d2baea961351a1`, `c0bac4d6ced7f78a5464c6e3fd8cfbb43515a9d5`, `c2c54e7c0b84a213848b9ab3dfeb3ae16fb9844d`, `6b8a49a6eed11cc7df19d8b1f2add085b37cf466`, `1949d75e18e59a37de269d88b188fc701f5cf122`, `6951b8d5d29d13cac6b89b461104ad03838553de`, `f7035b5644ffc857879b71c39353b2141f17c3f0` |
| Commits de merge de AgentShield | `f899b27ba3fa60ec7e0dca41cc2dadcb1a1fb75d`, `d1aa5313afd915d0b7296e57aabaeb979b1ea93b`, `908d8f3a52a6a65b21e737339b56906603eb1345`, `69a5e25b675b77666d0c96abc22639a5ba883403` |
| Commits de merge de ECC-Tools | `6d00d67043e92cadc80f160bfe947115bfef33b1` |
| Pruebas IOC locales | `node tests/ci/scan-supply-chain-iocs.test.js` pasó 15/15 |
| Seguridad Unicode | `node scripts/ci/check-unicode-safety.js` pasó |
| Escaneo IOC | `node scripts/ci/scan-supply-chain-iocs.js --root <ECC-workspace> --home` pasó con 229 archivos inspeccionados después de la actualización de instalación sin lifecycle |
| Verificación del registro npm | `npm audit signatures` verificó 241 firmas de registro y 30 attestations; `npm audit --audit-level=high` no encontró vulnerabilidades |
| Purga de caché de Actions | `gh cache delete --all --succeed-on-no-caches` se completó y `gh cache list --limit 20` no devolvió cachés |
| Compuerta de superficie de release Rust | `cd ecc2 && cargo test` pasó 462/462 con las 14 advertencias existentes de dead-code/unused |
| Suite raíz | `node tests/run-all.js` pasó 2442/2442, 0 fallos |
| Barridos del repositorio | Las comprobaciones dirigidas de rutas de persistencia no encontraron artefactos activos `gh-token-monitor`, `pgsql-monitor`, `transformers.pyz` ni `pgmonitor.py` |

La expansión IOC del 15 de mayo agregó cobertura para variantes de campaña estilo OpenSearch/Mistral/Guardrails/
UiPath/Squawk, `opensearch_init.js`, `vite_setup.mjs`, cadenas de protocolo de dead-drop/session y superficies de persistencia de herramientas de IA sin
cometer indicadores de alta entropía completos que activen los escáneres de secretos.
El seguimiento de node-ipc del 15 de mayo bloquea `node-ipc@9.1.6`, `9.2.3`, `10.1.1`,
`10.1.2`, `11.0.0`, `11.1.0` y `12.0.1`, además del hash de la carga útil `node-ipc.cjs`, los hashes maliciosos de tarball, los dominios de exfiltración DNS y los marcadores de runtime reportados
por Socket.
El PR #83 de AgentShield agrega la cobertura empresarial correspondiente del lado del scanner:
detecciones de paquetes fijadas por versión, descubrimiento de superficies de automatización `.claude` / `.vscode`,
detección de artefactos `gh-token-monitor` LaunchAgent/systemd/local-bin,
IOCs de red/carga útil, bundles de action/CLI construidos, 1758/1758 pruebas locales y
verificación verde de GitHub Actions antes del merge.
El PR #84 de AgentShield cierra la brecha posterior de la tabla de paquetes de la campaña completa agregando
los scopes extra de paquetes npm afectados y paquetes sin scope reportados en la tabla actual de Wiz, reconstruyendo
`dist/action.js` y `dist/index.js`, y pasando
1758/1758 pruebas locales más la matriz completa de GitHub Actions de AgentShield antes del
merge.
El PR #85 de AgentShield y los PRs de trunk #1934, #1940 y #1941 amplían la respuesta
desde la detección de IOC hacia el endurecimiento de la ruta de release: AgentShield ahora registra
evidencia de firma de registro para su superficie de action, el trunk tiene un flujo de trabajo programado de monitoreo IOC, y la CI del trunk ya no usa cachés de dependencias ni scripts de ciclo de vida del administrador de paquetes en la matriz de instalación de pruebas durante el endurecimiento activo de la cadena de suministro.
El PR #86 de AgentShield completa el siguiente fragmento de procedencia del paquete de evidencia:
`agentshield scan --evidence-pack <dir>` ahora escribe `ci-context.json`, incluye
ese artefacto en el digest del bundle firmado, lo documenta en el README del bundle, y verifica que variables de entorno con tokens, como `GITHUB_TOKEN`,
no se copien en artefactos de security-review de larga duración. El PR pasó build local, typecheck, lint, 1764/1764 pruebas y la matriz completa de GitHub Actions
en Node 18, 20 y 22 antes del merge.
El PR #1933 cierra la brecha práctica de persistencia del workstation para las rutas documentadas de automatización de Claude Code y VS Code, incluidos archivos de configuración a nivel de usuario que
permanecen después de desinstalar el paquete.

## Estado del paquete de vista previa

`preview-pack-manifest.md` ahora ensambla el límite del preview-pack de rc.1:

- notas de release, quickstart, launch checklist, publication readiness, matriz de nombres y evidencia del 15 de mayo;
- `docs/HERMES-SETUP.md` y `skills/hermes-imports/SKILL.md` como la superficie pública especializada en Hermes;
- docs de cross-harness, harness-adapter, observability y progress-sync;
- material de X, LinkedIn, artículo, Telegram y demo que debe recibir URLs en vivo finales después de la publicación de release/package/plugin;
- bloqueos explícitos para release de GitHub, publish `next` de npm, plugin de Claude, plugin de Codex, readiness de facturación/producto de ECC Tools y anuncios.

El preview pack está ensamblado para la compuerta final de checkout limpio, pero todavía no es una acción de publicación.

## Evidencia de Codex Marketplace

La documentación actual del plugin Codex de OpenAI ahora distingue la distribución de marketplace de repo/personal del Directorio oficial de Plugins.
Los marketplaces de repo viven en `.agents/plugins/marketplace.json`; `codex plugin marketplace add <source>` puede agregar shorthand de GitHub, URLs Git, URLs SSH o roots locales de marketplace.
La publicación en el Directorio oficial de Plugins y la administración self-serve están documentadas
como próximas:

- <https://developers.openai.com/codex/plugins/build#add-a-marketplace-from-the-cli>
- <https://developers.openai.com/codex/plugins/build#how-codex-uses-marketplaces>
- <https://developers.openai.com/codex/plugins/build#publish-official-public-plugins>

| Superficie | Evidencia |
| --- | --- |
| Forma de CLI | `codex plugin marketplace add --help` admite shorthand de GitHub, URLs Git, URLs SSH, roots locales de marketplace, `--ref` y `--sparse` solo para Git |
| Marketplace de repo | `.agents/plugins/marketplace.json` expone `ecc@2.0.0-rc.1` con `source.path: "./"` desde el root del marketplace |
| Prueba local de add | `HOME="$(mktemp -d)" codex plugin marketplace add <local-checkout>` agregó el marketplace `ecc` y registró el root instalado del marketplace como `<local-checkout>` sin tocar la configuración real de Codex |
| Alineación del README | `.codex-plugin/README.md` ahora usa `codex plugin marketplace add`, no el comando obsoleto `codex plugin install` |
| Estado del directorio público | La ruta de distribución soportada de Codex para rc.1 es marketplace de repo/instalación manual; la publicación en el Directorio oficial de Plugins sigue bloqueada por la disponibilidad del flujo self-serve de OpenAI que viene "próximamente" |

## Bloqueadores actuales de publicación

- El prerelease de GitHub `v2.0.0-rc.1` todavía no se creó en esta pasada.
- `ecc-universal@2.0.0-rc.1` de npm todavía no se publicó en el dist-tag `next`.
- La etiqueta del plugin de Claude y la propagación en el marketplace siguen sujetas a aprobación.
- La distribución del plugin de Codex vía marketplace de repo está verificada para rc.1, pero la publicación en el Directorio oficial de Plugins sigue bloqueada por la superficie de publicación self-serve que OpenAI anuncia como próxima.
- La PR #73 de ECC Tools agregó una compuerta `announcementGate` fail-closed para `/api/billing/readiness` para las afirmaciones nativas de pagos de GitHub, y la PR #74 de ECC Tools agregó `npm run billing:announcement-gate` como verificador del operador, pero el readback en vivo de la cuenta de prueba administrada por Marketplace todavía debe devolver `announcementGate.ready === true` antes de cualquier anuncio público de pagos.
- Las notas de release, X, LinkedIn y la copia de formato largo todavía necesitan URLs en vivo finales después de que existan las URLs de release/package/plugin.

## Resultado

La cola, las discusiones, la hoja de ruta de Linear y la evidencia de cadena de suministro
son más recientes que la evidencia de publicación del 13 de mayo. Mejoran la preparación, pero no
reemplazan la pasada final de publicación con checkout limpio requerida por
`publication-readiness.md`.
