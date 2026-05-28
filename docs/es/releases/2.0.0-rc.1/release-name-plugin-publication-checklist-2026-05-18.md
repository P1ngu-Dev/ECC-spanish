# Lista de verificación de nombre de lanzamiento y publicación del plugin de ECC v2.0.0-rc.1

Fecha de captura: 2026-05-18. La decisión canónica del repo se actualizó el 2026-05-19
después del cambio de nombre del repositorio público a `affaan-m/ECC`.

Esta lista de verificación es el gate de operador para el nombre del lanzamiento, la publicación del paquete
y la distribución de plugins de Claude/Codex. No es una acción de publicación por sí sola.
Ejecuta esto desde el commit exacto de release antes de crear tags, publicar en npm,
enviar formularios de marketplace o publicar anuncios.

## Decisión fija de rc.1

Lanzar `v2.0.0-rc.1` como **ECC**.

- Mantener el repo de GitHub en `affaan-m/ECC`.
- Mantener el paquete npm como `ecc-universal`.
- Mantener los slugs de plugin de Claude y Codex como `ecc`.
- Publicar el prerelease de npm con el dist-tag `next`, no `latest`.
- No renombrar el paquete npm a `ecc` ni `@affaan-m/ecc` antes de rc.1.
- Tratar `affaan-m/ECC` como el repo público canónico para rc.1 y la copia del release GA.

Razones:

- `ecc-universal` es la superficie actual de instalación y paquete en funcionamiento.
- `ecc` en npm está ocupado por un paquete de curva elíptica no relacionado.
- `@affaan-m/ecc` no está reclamado en npm, pero requeriría un plan de migración.
- `affaan-m/ECC` es ahora el repo público vivo de GitHub.
- Claude y Codex ya exponen el espacio de nombres corto deseado como `ecc`.

## Evidencia de la superficie actual

| Superficie | Valor actual | Comando de evidencia | Resultado 2026-05-18 | Acción de lanzamiento |
| --- | --- | --- | --- | --- |
| Commit git | `67e63e63f9bfd074bd6a21bf6bac71f3dfefa58b` | `git rev-parse HEAD` | Registrado desde `main` limpio antes de este refresco de evidencia ITO-46 | Reejecutar desde el commit final de release |
| Repo de GitHub | `affaan-m/ECC` | `git remote get-url origin` | `https://github.com/affaan-m/ECC.git` | Mantener para rc.1 y GA |
| Paquete npm | `ecc-universal@2.0.0-rc.1` local, `1.10.0` latest del registry | `node -p "require('./package.json').name + '@' + require('./package.json').version"` y `npm view ecc-universal name version dist-tags --json` | rc.1 listo localmente; el registry sigue con `latest 1.10.0` | Publicar rc.1 con `--tag next` tras aprobación |
| Nombre corto exacto de npm | `ecc` | `npm view ecc name version description repository.url --json` | Ocupado por `ecc@0.0.2` no relacionado | No usar |
| Nombre corto con scope | `@affaan-m/ecc` | `npm view @affaan-m/ecc name version --json` | 404 | Candidato solo después del plan de migración |
| Plugin de Claude | `ecc@2.0.0-rc.1` | `claude plugin validate .claude-plugin/plugin.json`; `claude plugin validate .`; `claude plugin tag .claude-plugin --dry-run` | Validación aprobada en Claude Code `2.1.143`; la validación completa del plugin tiene una advertencia esperada de contexto del `CLAUDE.md` raíz; el dry run crearía `ecc--v2.0.0-rc.1` | Ejecutar de nuevo el dry-run tag desde el commit final, luego tag/push solo tras la aprobación |
| Marketplace de Claude | `.claude-plugin/marketplace.json` | `claude plugin marketplace add --help`; docs del marketplace de plugins de Anthropic | Se soportan fuentes de marketplace desde repo de GitHub, URL git, JSON remoto de marketplace y ruta local | Verificar la ruta de instalación/actualización de marketplace después de la evidencia final |
| Plugin de Codex | `ecc@2.0.0-rc.1` | `node tests/plugin-manifest.test.js`; `codex plugin marketplace add --help`; docs de plugin de OpenAI Codex | El manifiesto del plugin pasó 54/54; los smokes locales y desde repo GitHub pasaron en Codex CLI `0.131.0` | Usar el repo marketplace para rc.1; no afirmar listado en el directorio oficial hasta que la ruta de publicación de OpenAI esté disponible |
| Paquete OpenCode | `ecc-universal@2.0.0-rc.1` | `node -p "require('./.opencode/package.json').name + '@' + require('./.opencode/package.json').version"` | Coincide con la identidad del paquete rc.1 | Seguir la publicación del paquete npm |
| Afirmación de facturación | evidencia de facturación target seleccionado de ECC Tools lista | gate de facturación de ECC Tools y readback de la cuenta de Marketplace | el readback seleccionado del 20 de mayo y el gate de anuncio del target seleccionado en vivo pasaron con `announcementGateReady: true`; repetir inmediatamente antes del anuncio | No anunciar pagos nativos hasta que las aprobaciones finales de release/plugin/URL en vivo estén en verde |

## Gate requerido

Ejecuta estas comprobaciones desde el commit final de release y pega la salida exacta en
un archivo nuevo `publication-evidence-YYYY-MM-DD.md` antes de las acciones de release:

```bash
git status --short --branch
git rev-parse HEAD
git remote get-url origin
npm view ecc name version description repository.url --json
npm view @affaan-m/ecc name version --json
npm view ecc-universal name version dist-tags --json
node tests/plugin-manifest.test.js
node tests/docs/ecc2-release-surface.test.js
claude plugin validate .claude-plugin/plugin.json
claude plugin tag .claude-plugin --dry-run
codex plugin marketplace add --help
HOME="$(mktemp -d)" codex plugin marketplace add ./
HOME="$(mktemp -d)" codex plugin marketplace add affaan-m/ECC --ref "$(git rev-parse HEAD)"
npm pack --dry-run --json
npm publish --tag next --dry-run
npm run build:opencode
npm run preview-pack:smoke
npm run release:approval-gate -- --format json
```

Si un comando no está disponible en la máquina de release, registra el error exacto y
mantén bloqueada la acción de publicación relacionada.

## Orden de publicación

| Paso | Acción | Evidencia requerida | Condición de parada |
| --- | --- | --- | --- |
| 1 | Congelar nombre y versión | Paquete, plugin de Claude, plugin de Codex, paquete OpenCode, `VERSION` y docs de release dicen todos `2.0.0-rc.1` | Cualquier desajuste `preview`/`rc.1` |
| 2 | Verificar rama de release limpia | `git status --short --branch` muestra solo el commit de release previsto y ningún drift no relacionado | cualquier archivo sucio inexplicado |
| 3 | Verificar manifests de paquete y plugin | `node tests/plugin-manifest.test.js` y `node tests/docs/ecc2-release-surface.test.js` pasan | fallo de manifest o superficie de release |
| 4 | Dry-run de la superficie del paquete | `npm pack --dry-run --json`; `npm publish --tag next --dry-run` | archivos faltantes, dist-tag incorrecto o fallo del dry-run de publicación |
| 5 | Dry-run de distribución de Claude | `claude plugin validate`; `claude plugin tag .claude-plugin --dry-run`; evidencia de marketplace source/help | fallo de validación, tag o smoke de instalación |
| 6 | Verificar marketplace del repo de Codex | `codex plugin marketplace add --help`; smoke de marketplace local y GitHub-ref con home temporal; estado del directorio oficial de OpenAI registrado | falta el marketplace del repo o el estado del directorio oficial no está verificado |
| 7 | Verificar paquete de OpenCode | `npm run build:opencode` | fallo de build |
| 8 | Regenerar ledger de URLs de release | URLs en vivo y protegidas por aprobación separadas en `release-url-ledger-YYYY-MM-DD.md` | placeholder, URL privada o drift de URL de anuncio |
| 9 | Crear prerelease de GitHub | `gh release view v2.0.0-rc.1 --json tagName,url,isPrerelease` | falta URL o la marca prerelease es incorrecta |
| 10 | Publicar rc de npm | `npm view ecc-universal version dist-tags --json` muestra rc.1 en `next` | rc.1 cae en `latest` o la salida del registry no es clara |
| 11 | Publicar/enviar plugin | evidencia de la sumisión oficial de Claude y del marketplace del repo de Codex registrada | formulario no enviado, listado no visible o cambio en el estado de docs |
| 12 | Anunciar | la copia de X, LinkedIn, GitHub release y formato largo usa las URLs finales en vivo | cualquier URL final sigue pendiente |

## No proceder

- No publiques en npm antes de capturar `npm pack --dry-run --json` desde el commit final de release.
- No crees ni empujes tags del plugin de Claude antes de que `claude plugin tag .claude-plugin --dry-run` pase desde el commit final de release.
- No afirmes un listado oficial en el Codex Plugin Directory a menos que OpenAI documente una ruta pública de envío o confirme que el plugin ha sido listado.
- No anuncies facturación, Marketplace o pagos nativos hasta que el readback de la cuenta en vivo de ECC Tools devuelva listo.
- No renombres el paquete npm hasta que rc.1 esté publicado y una guía de migración mapee los nombres de instalación antiguos a los nuevos.
- No publiques copia social mientras cualquier URL de release, npm, plugin o facturación siga protegida por aprobación.

## Fuentes externas de distribución

- Docs del plugin de Anthropic Claude Code: `https://code.claude.com/docs/en/plugins`
- Docs del marketplace de Anthropic Claude Code:
  `https://code.claude.com/docs/en/plugin-marketplaces`
- Docs del plugin de OpenAI Codex:
  `https://developers.openai.com/codex/plugins/build#add-a-marketplace-from-the-cli`

A fecha de esta captura, Anthropic documenta la distribución de marketplace autoalojado
mediante GitHub, URL git, JSON remoto de marketplace y fuentes de ruta local.
OpenAI documenta la distribución desde repo/personal marketplace para Codex y describe
un Directorio oficial de plugins, pero ECC no ha presentado ni recibido un listado oficial
en este pase.
