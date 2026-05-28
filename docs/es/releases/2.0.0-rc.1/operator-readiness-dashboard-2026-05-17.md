# Panel de Preparación del Operador de ECC

Este panel se genera con `npm run operator:dashboard`. Es una instantánea del operador, no una aprobación de lanzamiento.

Generado: 2026-05-18T01:50:19.099Z
Commit: 000df72d6b9c5b11feb11deef609911943b48424
Estado: trabajo pendiente

## Estado Actual

| Área | Estado | Evidencia |
| --- | --- | --- |
| Cola de PR | Actual | 0 PR abiertos en los repositorios rastreados |
| Cola de issues | Actual | 0 issues abiertos en los repositorios rastreados |
| Discussions | Actual | 0 requieren intervención de mantenedor; 0 sin respuesta aceptada |
| Worktree local | Actual | 0 archivos sucios que bloquean; 1 entrada sucia ignorada |
| Generación del panel | Actual | auditoría de la plataforma lista: true; GitHub omitido: false |
| Publicación | Incompleto | las puertas de release, npm, plugin, facturación y anuncio se rastrean abajo |

## Lista de verificación de Prompt a Artefacto

| Requisito objetivo | Artefacto o puerta | Estado | Evidencia | Brecha |
| --- | --- | --- | --- | --- |
| Mantener los PR públicos por debajo de 20 | barrido en vivo de GitHub de `scripts/platform-audit.js` | current | 0 PR abiertos en 5 repositorios rastreados | repetir antes del release |
| Mantener los issues públicos por debajo de 20 | barrido en vivo de GitHub de `scripts/platform-audit.js` | current | 0 issues abiertos en 5 repositorios rastreados | repetir antes del release |
| Responder y gestionar discusiones del repositorio | resumen de discusiones de `scripts/platform-audit.js` | current | 0 requieren intervención de mantenedor; 0 discusiones respondibles sin respuesta aceptada | repetir antes del release |
| Convertir el dashboard de finalización ITO-44 en un comando repetible | `npm run operator:dashboard` | complete | existe el script de paquete `operator:dashboard` | mantener el panel generado adjunto a la evidencia de publicación |
| Paquete de vista previa de ECC 2.0 listo | `docs/releases/2.0.0-rc.1/preview-pack-manifest.md` | current | el manifiesto del paquete de vista previa y la puerta determinista de smoke están en el árbol | repetir el smoke de vista previa en clean-checkout antes de la publicación |
| Incluir con seguridad las habilidades especializadas de Hermes | `docs/HERMES-SETUP.md` y `skills/hermes-imports/SKILL.md` | current | los artefactos de configuración/importación de Hermes están cubiertos por el smoke del paquete de vista previa | repetir el smoke del paquete de vista previa antes de la revisión de release |
| Preparar las rutas de cambio de nombre, plugin de Claude y plugin de Codex | matriz de nombrado y publicación más preparación de publicación | in_progress | existen la matriz de nombrado y las puertas de preparación del plugin | la etiqueta/push real, el envío al marketplace y la elección final del canal siguen sujetos a aprobación |
| Preparar notas de release, artículos, tweets y notificaciones push | archivos de contenido social y de release en `docs/releases/2.0.0-rc.1` | in_progress | están presentes las notas de release, el hilo de X y el borrador de LinkedIn | la actualización respaldada por URL y la aprobación de publicación siguen pendientes |
| Avanzar la iteración empresarial de AgentShield | evidencia de PR de AgentShield más la hoja de ruta empresarial | in_progress | la promoción de política de AgentShield `reviewItems` llegó en `87aec47`; la detección de deriva en la hardening del package manager llegó en `28d08c7`; los anclajes de runtime de la acción del flujo de trabajo se actualizaron en `659f569`; la guía del age-gate de npm se corrigió en `ee585cd`; los outputs de la Action de hardening del package manager llegaron en `1124535`; los outputs de la Action de promoción de política y la evidencia del resumen de trabajo en runtime-smoke llegaron en `1593925`; ECC-Tools consume esos outputs en `8658951`, expone telemetría legible para operador de estado/paquetes/contador/resumen en `16c537f`, y renderiza trazas de auditoría del juez de promoción alojado en `05d4e82`; todo está reflejado en la hoja de ruta GA | profundizar la aprobación/lectura del operador en vivo después de las puertas de Marketplace/pago |
| Avanzar los pagos nativos y la app agnóstica al harness nativa de IA de ECC Tools | evidencia de PR de ECC Tools, puerta de facturación, lanes de análisis alojadas | in_progress | la puerta de anuncio de facturación, los lanes de análisis alojados, el consumo del resumen de flota de AgentShield, las rutas de evidencia de findings alojadas, el enlace de política de rutas de harness, la telemetría de outputs de la Action de promoción de política, los detalles visibles para operador del output de promoción, las trazas de auditoría del juez de promoción alojado, el preflight del anuncio de facturación y el estado de lectura KV en producción están reflejados en la hoja de ruta GA | completar la compra en Marketplace y la lectura del webhook, luego ejecutar la puerta de anuncio en vivo |
| Auditar, podar o adjuntar trabajo heredado | `docs/stale-pr-salvage-ledger.md` e inventario heredado | current | el libro mayor de salvamento heredado y el inventario están actualizados; todas las colas de localización están adjuntas a Linear ITO-55 para revisión manual por el propietario del idioma | repetir el escaneo heredado antes del release |
| Mantener detallada la hoja de ruta de Linear y sincronizado el seguimiento del progreso | espejo del proyecto Linear más contrato de sincronización de progreso | current | la sincronización en vivo de Linear y la instantánea de progreso del proyecto están actualizadas; el contrato de sincronización de progreso define la ruta de archivos para work-items/estado | repetir la actualización de estado de Linear/proyecto y la sincronización local de work-items después de cada lote de merge significativo |
| Proporcionar observabilidad de ECC 2.0 para uso propio | puerta de preparación de observabilidad | complete | existen el comando `observability:ready` y el documento de preparación | la implementación del runtime/panel puede continuar después de las puertas de release |
| Mantener actual el bucle de protección Mini Shai-Hulud/TanStack | vigilancia de cadena de suministro más runbook más hardening del package manager de AgentShield | current | la vigilancia programada de la cadena de suministro emite artefactos de actualización de fuentes/advisories/IOC; el escáner de ECC cubre la persistencia del almacén de tokens de `gh-token-monitor`; AgentShield ahora detecta IOCs conocidos de persistencia de herramientas de IA, deriva de ciclo de vida/token de npm, deriva no compatible de age-key de npm, y deriva de cooldown de pnpm/Yarn; ITO-57 tiene actualizaciones de evidencia de Linear del 17 de mayo | repetir la actualización de advisories/fuentes y la sincronización de Linear después de cada lote significativo de cadena de suministro |

## Acciones principales

- `naming-and-plugin-publication`: la etiqueta/push real, el envío al marketplace y la elección final del canal siguen sujetos a aprobación
- `release-notes-and-notifications`: la actualización respaldada por URL y la aprobación de publicación siguen pendientes
- `agentshield-enterprise-iteration`: profundizar la aprobación/lectura del operador en vivo después de las puertas de Marketplace/pago
- `ecc-tools-next-level`: completar la compra en Marketplace y la lectura del webhook, luego ejecutar la puerta de anuncio en vivo

## Próximo orden de trabajo

1. Regenera este panel a partir del commit final de release antes de registrar la evidencia de publicación.
2. Repite la sincronización de estado de Linear/proyecto ITO-57 después del siguiente lote significativo de merge o actualización de la fuente de advisories.
3. Completa la lectura de compra/webhook de Marketplace de ECC Tools, luego ejecuta el preflight y la puerta de anuncio en vivo antes de publicar el contenido de pagos nativos.
4. Reanuda ITO-45, ITO-46 e ITO-56 solo después de que el panel generado y las puertas finales de release se hayan actualizado.
