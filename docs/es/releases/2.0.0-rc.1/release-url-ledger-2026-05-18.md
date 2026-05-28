# Registro de URLs de lanzamiento de ECC v2.0.0-rc.1

Este registro separa los enlaces que ya son públicos de los enlaces que solo se vuelven
válidos después de los pasos de lanzamiento, paquete, plugin y anuncio protegidos por
aprobación. Regénéralo desde el commit final de release antes de publicar cualquier
anuncio público.

Capturado desde el commit fuente
`81fca2cea6f1399c52c8faa70f9a17e42f0bd447` el 2026-05-18. El archivo del registro
puede comprometerse en una actualización posterior de solo docs después de la instantánea
de evidencia que describe.

## Disponible ahora

| Superficie | URL | Verificación |
| --- | --- | --- |
| Repositorio | <https://github.com/affaan-m/everything-claude-code> | `git remote get-url origin` |
| Commit fuente de evidencia | <https://github.com/affaan-m/everything-claude-code/commit/81fca2cea6f1399c52c8faa70f9a17e42f0bd447> | `git rev-parse HEAD` en la captura de evidencia |
| Carpeta del pack de release | <https://github.com/affaan-m/everything-claude-code/tree/main/docs/releases/2.0.0-rc.1> | evidencia del pack de release capturada desde `81fca2ce` |
| Borrador de notas de la versión | <https://github.com/affaan-m/everything-claude-code/blob/main/docs/releases/2.0.0-rc.1/release-notes.md> | copia de release en el árbol |
| Guía de configuración de Hermes | <https://github.com/affaan-m/everything-claude-code/blob/main/docs/HERMES-SETUP.md> | guía de Hermes saneada en el árbol |
| Instantánea de evidencia del 18 de mayo | <https://github.com/affaan-m/everything-claude-code/blob/main/docs/releases/2.0.0-rc.1/publication-evidence-2026-05-18.md> | evidencia de preparación más sólida actual |
| Dashboard de operador del 18 de mayo | <https://github.com/affaan-m/everything-claude-code/blob/main/docs/releases/2.0.0-rc.1/operator-readiness-dashboard-2026-05-18.md> | dashboard de prompt a artefacto |
| CI de head empujado | <https://github.com/affaan-m/everything-claude-code/actions/runs/26011460500> | CI pasó 37/37 jobs para `81fca2ce`, incluido el job de escaneo IOC de supply-chain |
| Último Supply-Chain Watch | <https://github.com/affaan-m/everything-claude-code/actions/runs/26010432490> | Supply-Chain Watch pasó para `25ac57ac`; relánzalo desde el commit final de release antes de publicar |
| Página del paquete npm | <https://www.npmjs.com/package/ecc-universal> | `npm view ecc-universal name version dist-tags --json` devolvió `latest: 1.10.0`; rc.1 todavía no está publicado |
| Docs CLI del marketplace de Codex | <https://developers.openai.com/codex/cli/reference#codex-plugin-marketplace> | la documentación oficial lista `codex plugin marketplace add` para shorthands de GitHub, URLs git, URLs SSH y raíces locales de marketplace |
| Estado del Directorio oficial de plugins de Codex | <https://developers.openai.com/codex/plugins/build#publish-official-public-plugins> | la documentación oficial dice que la publicación en el Directorio público de plugins y la gestión self-serve llegarán pronto |

## URLs protegidas por aprobación

| Superficie | URL o comando previsto | Gate antes de usar |
| --- | --- | --- |
| Pre-release de GitHub | <https://github.com/affaan-m/everything-claude-code/releases/tag/v2.0.0-rc.1> | `gh release view v2.0.0-rc.1 --repo affaan-m/everything-claude-code --json tagName,url,isPrerelease` debe devolver el prerelease |
| paquete npm rc | <https://www.npmjs.com/package/ecc-universal/v/2.0.0-rc.1> | aprobación de `npm publish --tag next` y, tras publicar, `npm view ecc-universal dist-tags --json` |
| tag del plugin de Claude | `claude plugin tag .claude-plugin --dry-run`, luego tag real solo después de la aprobación | commit de release limpio y aprobación de tag/push del plugin |
| instalación desde marketplace del repo de Codex | `codex plugin marketplace add affaan-m/everything-claude-code --ref v2.0.0-rc.1` | el tag de GitHub debe existir; la presentación al Directorio oficial de plugins es independiente |
| anuncio de pagos nativos de ECC Tools | URL de Marketplace/App de ECC Tools más readback de preparación de facturación | el target de prueba gestionado por Marketplace debe devolver `announcementGate.ready === true` |
| anuncios públicos | X, LinkedIn, GitHub release y URLs de contenido largo | primero deben resolver el release de GitHub, npm, plugin y facturación |

## Comprobación previa a la publicación

Ejecuta esto inmediatamente antes de publicar:

```bash
git status --short --branch
gh release view v2.0.0-rc.1 --repo affaan-m/everything-claude-code --json tagName,url,isPrerelease
npm view ecc-universal name version dist-tags --json
codex plugin marketplace add --help
rg -n "TODO|TBD|PLACEHOLDER" docs/releases/2.0.0-rc.1
npm run preview-pack:smoke
```

No publiques la copia social o de notificación hasta que las URLs protegidas por aprobación anteriores
resuelvan desde un commit limpio de release.
