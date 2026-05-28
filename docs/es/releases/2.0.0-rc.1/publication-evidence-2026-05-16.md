# Evidencia de publicación de ECC v2.0.0-rc.1 - 2026-05-16

Esto es solo evidencia de preparación de la versión. No crea una publicación de GitHub,
una publicación en npm, una etiqueta del plugin, una entrega al marketplace ni una publicación de anuncio.

## Commit de origen

| Campo | Evidencia |
| --- | --- |
| main upstream | `6bced468d76b269243a6f0bd28472853aa78e0e4` |
| Remote de Git | `https://github.com/affaan-m/everything-claude-code.git` |
| Alcance de la evidencia | `main` actual después de PR #1944, PR #1945, triage de issue #1946, PR #1947 protección de supply chain, PR #87 de AgentShield, PR #88 de AgentShield, PR #89 de AgentShield, PR #90 de AgentShield, PR #91 de AgentShield, PR #92 de AgentShield, PR #76 de ECC-Tools, PR #77 de ECC-Tools, PR #78 de ECC-Tools, triage de localización japonesa, sincronización ITO-57 y actualización del panel de operador |
| Advertencia de estado local | `git status --short --branch` mostró `## main...origin/main` además de `docs/drafts/` sin seguimiento no relacionado |

El operador de publicación real debe repetir todas las verificaciones orientadas a la publicación desde el
commit final de la versión con un checkout estrictamente limpio antes de publicar.

## Cola y estado de discusión

| Superficie | Comando | Resultado |
| --- | --- | --- |
| PR del trunk | `gh pr list --state open --json number,title,url --limit 20` | 6 PR abiertos: Dependabot #1959-#1963 más PR #1953, que sigue abierto con cambios solicitados por paridad de localización japonesa |
| Issues del trunk | `gh issue list --state open --json number,title,url --limit 20` | 3 issues abiertos: #1951 vinculado al PR de localización retenido, más #1957 y #1958 esperando el siguiente lote de cola |
| Auditoría de la plataforma | `node scripts/platform-audit.js --json --allow-untracked docs/drafts/` | Listo; 6 PR abiertos, 3 issues abiertos, 0 brechas de contacto de mantenedor en discusiones, 0 brechas de respuesta faltante en discusiones, 0 archivos sucios bloqueantes en un checkout limpio; la generación actual de la rama ve las ediciones espejo como trabajo local sucio |
| Panel de operador | `npm run operator:dashboard -- --json --allow-untracked docs/drafts/` | `dashboardReady: true`, `platformReady: true`, head `6bced468d76b269243a6f0bd28472853aa78e0e4` |

## Lote de merge y triage

| Elemento | Resultado |
| --- | --- |
| PR #1944 | Se fusionó la actualización de la paleta ANSI de statusline como `50ac061f9e72d7daa137f1bd08760cf74e9b577d`; `node tests/hooks/ecc-statusline.test.js` y `node scripts/ci/validate-hooks.js` específicos pasaron antes del merge |
| PR #1945 | Se fusionó la skill comunitaria `recsys-pipeline-architect` como `9e973b29fb1a2a0aeb9e6980017b67c3ddb05201`; los parches de mantenedor sincronizaron los conteos del catálogo y eliminaron el emoji bloqueado por seguridad Unicode |
| Issue #1946 | Se cerró como triage con un comentario de mantenedor corregido; Linear `ITO-60` ahora rastrea la UX de preflight proactiva de GateGuard para forzar hechos |
| PR #1947 | Se fusionó la evidencia programada de vigilancia de supply chain/fuente de advisories como `4093d1bb7a14db1b4d4ea5bd00f2073baf94bfb0`; el trunk ahora tiene el escaneo IOC de TanStack/Mini Shai-Hulud/node-ipc más superficies de informe de source advisory conectadas a la evidencia programada de vigilancia |
| PR #87 de AgentShield | Se fusionó la clasificación de confianza en runtime de plugin-cache como `26bb44650663816d07180e0d20c1895e431a326c`; los hallazgos de caché del plugin Claude instalados ahora emiten `runtimeConfidence: plugin-cache`, `plugins/cache` solo mapea la caché de Claude bajo `.claude`, y las implementaciones de hook en caché ya no se etiquetan erróneamente como `hook-code` activo |
| PR #88 de AgentShield | Se fusionó inspect/readback de evidence-pack como `65ed6e2a87545dc99d962b58413f49096a4d70ec`; `agentshield evidence-pack inspect` ahora emite resúmenes JSON/texto verificados para report, policy, baseline, supply-chain, contexto de CI, remediation y errores de artefactos malformados |
| PR #89 de AgentShield | Se fusionó el enrutamiento de flota de evidence-pack como `521ada9091bb6d818511ab8589ae675b920c106a`; `agentshield evidence-pack fleet <dirs...> [--json]` ahora agrega múltiples bundles verificados en rutas ready, security-blocker, policy-review, baseline-regression, supply-chain-review e invalid, con totales de finding, policy, baseline, supply-chain y remediation |
| PR #90 de AgentShield | Se fusionaron los elementos de revisión de la flota como `6d1c57c92000541d65a3b6bc366f0322d7d0dacc`; `agentshield evidence-pack fleet --json` ahora emite `reviewItems` con route, severity, contexto de repository/target, rutas de evidence de origen, reason y una recomendación lista para owner, y la CLI de texto imprime un bloque `Review items` |
| PR #91 de AgentShield | Se fusionó la exportación de policy respaldada por checksum como `73e1e3586dc4513a462e39c9799f75eea104e110`; `agentshield policy export` escribe un archivo JSON de policy por cada pack seleccionado más `manifest.json` con digests SHA-256, y admite selección de packs, owners repetidos, prefijos de nombre y salida JSON |
| PR #92 de AgentShield | Se fusionó la promoción de policy verificada por checksum como `e7e259dc6212b63a8e03a253ca6b8c1e3c2abff7`; `agentshield policy promote` verifica el manifest exportado y el digest de policy seleccionado, rechaza JSON manipulado, exige selección explícita de pack para manifests de múltiples packs, admite revisión JSON de dry-run y escribe la policy activa solo después de la verificación |
| ECC-Tools PR #76 | Se fusionó el consumo de `fleet-summary` de AgentShield como `5bde2328d15f584481fb6334e6960716dbf3e16f`; `security-evidence-review` alojado ahora reconoce `agentshield-evidence/fleet-summary.json`, lo clasifica como `evidence-pack-fleet`, enruta resultados de flota invalid/security-blocker/policy/baseline/supply-chain hacia hallazgos alojados y falla cerrado ante JSON de flota malformado |
| ECC-Tools PR #77 | Se fusionó la salida de source-evidence para hallazgos alojados como `31fd883b3f0cee135aee4839b01d34855b7867f6`; los comentarios de PR alojados y los detalles de check-run ahora incluyen una columna `Evidence` con hasta tres rutas de source evidence por hallazgo, incluidos los hallazgos derivados de la flota de AgentShield |
| ECC-Tools PR #78 | Se fusionó la revisión del harness de route de flota de AgentShield como `0d4eb949aa56f56da88e6654273a22ffb95983a1`; `harness-compatibility-audit` alojado ahora recopila resúmenes de flota, mapea rutas de target a propietarios de harness Claude/Codex/OpenCode/MCP/plugin y emite hallazgos de owner-review con rutas de evidence de origen |
| ITO-57 | Actualizado con evidencia de source advisory de PR #1947, refresh de fuente post-merge, escaneo IOC, comprobaciones de npm audit/firma y advertencia de actualización de la app de OpenAI |
| ITO-49 | Actualizado con evidencia de merge de PR #87, #88, #89, #90, #91 y #92 de AgentShield, evidencia local de pruebas, estado de CI, conteos de clasificación del escaneo en vivo de `~/.claude`, resultados del escaneo local de protección Mini Shai-Hulud y validación de promoción de policy |
| ITO-50 | Actualizado con evidencia de merge de PR #76, PR #77 y PR #78 de ECC-Tools, comportamiento de revisión de seguridad alojado, comportamiento de evidence-path de hallazgos alojados, comportamiento de owner-review de route de flota de harness, evidencia local de pruebas y comprobaciones remotas de Verify/Security Audit/Workers build |
| ITO-44 | Actualizado con limpieza de cola, refresh del panel y brechas macro restantes |

## Comandos de control de la versión

| Control | Comando | Resultado |
| --- | --- | --- |
| Suite raíz | `npm test` | 2469 aprobados, 0 fallidos |
| Suite Rust `ecc2` | `cd ecc2 && cargo test` | 462 aprobados, 0 fallidos; solo advertencias existentes de dead-code/unused |
| Superficie de la versión | `node tests/docs/ecc2-release-surface.test.js` | 20 aprobados |
| Adaptadores de harness | `npm run harness:adapters -- --check` | PASS; 11 adaptadores |
| Auditoría de harness | `npm run harness:audit -- --format json` | 70/70, sin acciones principales |
| Preparación de observabilidad | `npm run observability:ready` | 21/21, ready yes |
| Escaneo IOC de supply chain | `npm run security:ioc-scan` | Passed; 227 archivos inspeccionados |
| Refresh de source advisory | `npm run security:advisory-sources -- --refresh --json` | Ready; 9 fuentes activas; el payload de Linear todavía apunta a `ITO-57` para sincronización |
| npm audit | `npm audit --audit-level=moderate` | 0 vulnerabilidades |
| npm signatures | `npm audit signatures` | 241 firmas de registry verificadas; 30 attestations verificadas |
| Renderizador del panel | `node tests/scripts/operator-readiness-dashboard.test.js` | 7 aprobados, 0 fallidos |

## Bloqueadores actuales de publicación

- La prerelease de GitHub `v2.0.0-rc.1` todavía no se ha creado en esta pasada.
- `ecc-universal@2.0.0-rc.1` en npm todavía no se ha publicado al dist-tag `next`.
- La propagación de la etiqueta del plugin Claude y del marketplace sigue sujeta a aprobación.
- La distribución del repositorio marketplace de Codex está verificada para rc.1, pero la publicación oficial en Plugin Directory sigue bloqueada por la superficie de publicación self-serve de OpenAI, que está “coming soon”.
- La copia de billing/native-payments de ECC Tools sigue bloqueada hasta que la lectura de vuelta de la cuenta de prueba administrada por Marketplace devuelva un control listo para anuncio.
- Las release notes, X, LinkedIn, GitHub release y la copia de formato largo todavía necesitan URLs finales en vivo después de que existan las URLs de release/package/plugin.
- El checkout local aún tiene `docs/drafts/` sin seguimiento no relacionado, así que sigue siendo necesaria una pasada de publicación con checkout limpio estricto antes de la publicación real.

## Resultado

La cola pública de PR, la cola de issues y la cola de discusiones están limpias, y el paquete de vista previa rc.1
aprobó los controles principales de Node, Rust, release-surface, harness, observability y supply-chain el 16 de mayo de 2026. Esto mejora la preparación para la publicación, pero
no reemplaza los pasos de publicación, paquete, plugin y anuncio sujetos a aprobación definidos en `publication-readiness.md`.
