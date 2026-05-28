# Registro de URLs de lanzamiento de ECC v2.0.0-rc.1

Este registro separa los enlaces que ya son públicos de los enlaces que solo se vuelven
válidos después de los pasos de lanzamiento, paquete, plugin y anuncio protegidos por
aprobación. Regénéralo desde el commit final de release antes de publicar cualquier
anuncio público.

Actualizado el 2026-05-19 después del cambio de nombre del repositorio público a
`affaan-m/ECC`. El pase final del lanzamiento debe reemplazar la evidencia específica
del commit con la salida del commit exacto de release.

## Disponible ahora

| Superficie | URL | Verificación |
| --- | --- | --- |
| Repositorio | <https://github.com/affaan-m/ECC> | `git remote get-url origin` devuelve `https://github.com/affaan-m/ECC.git` |
| Carpeta del pack de release | <https://github.com/affaan-m/ECC/tree/main/docs/releases/2.0.0-rc.1> | pack de release en el árbol |
| Borrador de notas de la versión | <https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/release-notes.md> | copia de release en el árbol |
| Guía de configuración de Hermes | <https://github.com/affaan-m/ECC/blob/main/docs/HERMES-SETUP.md> | guía de Hermes saneada en el árbol |
| Instantánea de evidencia del 19 de mayo | <https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/publication-evidence-2026-05-19.md> | evidencia actual más sólida de identidad, video, crecimiento y preparación de CI |
| Instantánea de evidencia del 18 de mayo | <https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/publication-evidence-2026-05-18.md> | evidencia previa de supply-chain y de preparación de la ruta de publicación |
| Dashboard de operador del 18 de mayo | <https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/operator-readiness-dashboard-2026-05-18.md> | dashboard previo de prompt a artefacto |
| Dashboard de operador del 19 de mayo | <https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/operator-readiness-dashboard-2026-05-19.md> | dashboard previo de prompt a artefacto con líneas de hipercrescimiento, video y outbound |
| Dashboard de operador del 20 de mayo | <https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/operator-readiness-dashboard-2026-05-20.md> | dashboard actual de prompt a artefacto con sincronización del gate de release de Marketplace Pro |
| Página del paquete npm | <https://www.npmjs.com/package/ecc-universal> | `npm view ecc-universal name version dist-tags --json` devolvió `latest: 1.10.0`; rc.1 todavía no está publicado |
| Docs CLI del marketplace de Codex | <https://developers.openai.com/codex/cli/reference#codex-plugin-marketplace> | la documentación oficial lista `codex plugin marketplace add` para shorthands de GitHub, URLs git, URLs SSH y raíces locales de marketplace |
| Estado del Directorio oficial de plugins de Codex | <https://developers.openai.com/codex/plugins/build#publish-official-public-plugins> | la documentación oficial dice que la publicación en el Directorio público de plugins y la gestión self-serve llegarán pronto |

## URLs protegidas por aprobación

| Superficie | URL o comando previsto | Gate antes de usar |
| --- | --- | --- |
| Pre-release de GitHub | <https://github.com/affaan-m/ECC/releases/tag/v2.0.0-rc.1> | `gh release view v2.0.0-rc.1 --repo affaan-m/ECC --json tagName,url,isPrerelease` debe devolver el prerelease |
| paquete npm rc | <https://www.npmjs.com/package/ecc-universal/v/2.0.0-rc.1> | aprobación de `npm publish --tag next` y, tras publicar, `npm view ecc-universal dist-tags --json` |
| tag del plugin de Claude | `claude plugin tag .claude-plugin --dry-run`, luego tag real solo después de la aprobación | commit de release limpio y aprobación de tag/push del plugin |
| instalación desde marketplace del repo de Codex | `codex plugin marketplace add affaan-m/ECC --ref v2.0.0-rc.1` | el tag de GitHub debe existir; la presentación al Directorio oficial de plugins es independiente |
| anuncio de pagos nativos de ECC Tools | URL de Marketplace/App de ECC Tools más readback de preparación de facturación del target seleccionado a través de la ruta bearer del operador | el target gestionado por Marketplace devolvió `announcementGate.ready === true` el 2026-05-20; repetir inmediatamente antes de publicar |
| anuncios públicos | X, LinkedIn, GitHub release y URLs de contenido largo | primero deben resolver el release de GitHub, npm, plugin y facturación |

## Comprobación previa a la publicación

Ejecuta esto inmediatamente antes de publicar:

```bash
git status --short --branch
gh release view v2.0.0-rc.1 --repo affaan-m/ECC --json tagName,url,isPrerelease
npm view ecc-universal name version dist-tags --json
codex plugin marketplace add --help
rg -n "TODO|TBD|PLACEHOLDER" docs/releases/2.0.0-rc.1
npm run preview-pack:smoke
npm run release:approval-gate -- --format json
```

No publiques la copia social o de notificación hasta que las URLs protegidas por aprobación anteriores
resuelvan desde un commit limpio de release.
