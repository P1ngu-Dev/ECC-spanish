# Evidencia de publicación de ECC v2.0.0-rc.1 — 2026-05-12

Esto es solo evidencia de una ejecución en seco del lanzamiento. No crea un release de GitHub, publicación en npm, etiqueta del plugin, envío al marketplace ni publicación de anuncio.

## Commit de origen

| Campo | Evidencia |
| --- | --- |
| Base ascendente de main | `0598af70a51346bae34d987b9bed143386055967` |
| Rama de evidencia | `codex/release-publication-evidence` |
| Alcance de la evidencia | Árbol de trabajo con la higiene de paquetes y las actualizaciones de documentación de lanzamiento de esta rama |
| Remote de Git | `https://github.com/affaan-m/everything-claude-code.git` |
| Advertencia de estado local | El árbol de trabajo tenía el directorio no rastreado no relacionado `docs/drafts/` |

El operador real del release debe repetir estas comprobaciones desde el commit final del release con un checkout limpio antes de publicar.

## Estado del registro y del release

| Superficie | Comando | Resultado |
| --- | --- | --- |
| Pre-release de GitHub | `gh release view v2.0.0-rc.1 --repo affaan-m/everything-claude-code --json tagName,url,isPrerelease` | `release not found` |
| Dist-tags de npm | `npm view ecc-universal dist-tags --json` | `{ "latest": "1.10.0" }` |
| Metadatos del paquete npm | `node -p "require('./package.json').name + '@' + require('./package.json').version"` | `ecc-universal@2.0.0-rc.1` |
| Identidad del producto | `rg -n "Everything Claude Code" README.md CHANGELOG.md docs/releases/2.0.0-rc.1` | Presente en README y en los docs de release rc.1 |

## npm Dry Run

El primer pase de empaquetado expuso archivos locales de caché de bytecode de Python en el tarball porque las entradas amplias de `files` del paquete incluían rutas `__pycache__` locales no rastreadas. Esta rama añade exclusiones explícitas de archivos del paquete y una prueba de regresión para que `npm pack` falle si aparece bytecode de Python en la superficie del paquete.

| Comando | Resultado |
| --- | --- |
| `node tests/scripts/npm-publish-surface.test.js` | Pasó `2/2`; incluye aserción de exclusión de bytecode de Python |
| `npm pack --dry-run --json` | `ecc-universal-2.0.0-rc.1.tgz`; `entryCount: 965`; `size: 1565968`; `unpackedSize: 4934637`; `hasBytecode: false` |
| `npm publish --tag next --dry-run --json` | El destino del dry run es el registro de npm con la etiqueta `next`; `entryCount: 965`; `hasBytecode: false` |

Prueba temporal de instalación:

| Comando | Resultado |
| --- | --- |
| `npm pack --pack-destination /tmp/ecc-publication-smoke-dd9ud5 --json` | Creó `ecc-universal-2.0.0-rc.1.tgz` para la prueba local de instalación |
| `npm install --prefix /tmp/ecc-publication-smoke-dd9ud5 /tmp/ecc-publication-smoke-dd9ud5/ecc-universal-2.0.0-rc.1.tgz` | Añadió 8 paquetes |
| `node /tmp/ecc-publication-smoke-dd9ud5/node_modules/ecc-universal/scripts/ecc.js --help` | Mostró la ayuda de la CLI de instalación selectiva de ECC |
| `node /tmp/ecc-publication-smoke-dd9ud5/node_modules/ecc-universal/scripts/catalog.js profiles --json` | Devolvió los 6 perfiles de instalación: `minimal`, `core`, `developer`, `security`, `research`, `full` |
| `find /tmp/ecc-publication-smoke-dd9ud5/node_modules/ecc-universal -path '*__pycache__*' -o -name '*.pyc' -o -name '*.pyo' -o -name '*.pyd'` | Sin salida |

## Evidencia del plugin y del harness

| Superficie | Comando | Resultado |
| --- | --- | --- |
| Manifest del plugin de Claude | `claude plugin validate .claude-plugin/plugin.json` | Pasó |
| Preflight de la etiqueta del plugin de Claude | `claude plugin tag .claude-plugin --dry-run` | Bloqueado por `docs/drafts/` no rastreado ajeno |
| Dry run forzado de la etiqueta del plugin de Claude | `claude plugin tag .claude-plugin --dry-run --force` | Crearía `ecc--v2.0.0-rc.1` en HEAD; no usar `--force` para un release real salvo que el mantenedor lo decida |
| CLI del marketplace de Codex | `codex plugin marketplace --help` y ayuda del subcomando | Soporta `add`, `upgrade` y `remove`; `add` soporta repo y raíces locales de marketplace |
| Paquete de OpenCode | `npm run build:opencode` | Pasó |
| Ruta de hook/plugin de Claude | `node tests/hooks/hooks.test.js` | Pasó `236/236` |
| Superficie de release de Codex | `node tests/docs/ecc2-release-surface.test.js` | Pasó `18/18` |
| Metadatos de agente/catálogo | `node tests/scripts/catalog.test.js` | Pasó `7/7` |
| Gate de observabilidad | `npm run observability:ready` | Pasó `16/16` |

## Prueba del plugin de Claude en checkout limpio

Este pase de seguimiento usó un worktree limpio separado en
`/tmp/ecc-clean-plugin-evidence` desde el commit
`bfacf37715b39655cbc2c48f12f2a35c67cb0253`. Usó un home temporal aislado
(`HOME=/tmp/ecc-clean-plugin-home`) y un proyecto local temporal
(`/tmp/ecc-plugin-install-smoke`), por lo que no escribió en la configuración real
del plugin de Claude del usuario.

| Comando | Resultado |
| --- | --- |
| `git -C /tmp/ecc-clean-plugin-evidence status --short --branch` | `## HEAD (no branch)` sin archivos sucios ni no rastreados |
| `claude plugin validate .claude-plugin/plugin.json` | Pasó |
| `claude plugin validate .claude-plugin/marketplace.json` | Pasó |
| `claude plugin tag .claude-plugin --dry-run` | Pasó sin `--force`; crearía `ecc--v2.0.0-rc.1` en HEAD y empujaría `refs/tags/ecc--v2.0.0-rc.1` |
| `claude plugin marketplace add /tmp/ecc-clean-plugin-evidence --scope local` con `HOME` temporal | Añadió el marketplace `ecc` en la configuración local |
| `claude plugin list --available --json` con `HOME` temporal | सूची? |
