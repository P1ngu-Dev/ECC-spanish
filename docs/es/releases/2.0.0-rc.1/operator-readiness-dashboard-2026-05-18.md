# Panel de preparación del operador de ECC

Este panel se genera con `npm run operator:dashboard`. Es una instantánea para operadores, no una aprobación de la versión.

Generado: 2026-05-18T20:25:22.649Z
Commit: 4470e2e6702f17099d6feb137ba03ff00582c202
Estado: trabajo pendiente

## Estado actual

| Área | Estado | Evidencia |
| --- | --- | --- |
| Cola de PR | Actual | 0 PR abiertos en los repositorios rastreados |
| Cola de issues | Actual | 0 issues abiertos en los repositorios rastreados |
| Discusiones | Actual | 0 requieren atención de mantenimiento; 0 sin respuesta aceptada |
| Worktree local | Actual | 0 archivos sucios bloqueantes; 0 entradas sucias ignoradas |
| Generación del panel | Actual | auditoría de plataforma lista: true; GitHub omitido: false |
| Publicación | No completa | las puertas de release, npm, plugin, facturación y anuncio se rastrean abajo |

## Lista de verificación de prompt a artefacto

| Requisito objetivo | Artefacto o puerta | Estado | Evidencia | Brecha |
| --- | --- | --- | --- | --- |
| Mantener los PR públicos por debajo de 20 | scripts/platform-audit.js, barrido activo de GitHub, más registro de limpieza de cola a nivel de propietario | current | 0 PR abiertos en 5 repositorios rastreados; 0 PR abiertos a nivel de propietario después de la limpieza | repetir platform:audit y la búsqueda gh a nivel de propietario antes del release |
| Mantener los issues públicos por debajo de 20 | scripts/platform-audit.js, barrido activo de GitHub, más registro de limpieza de cola a nivel de propietario | current | 0 issues abiertos en 5 repositorios rastreados; 0 issues abiertos a nivel de propietario después de la limpieza | repetir platform:audit y la búsqueda gh a nivel de propietario antes del release |
| Responder y gestionar discusiones del repositorio | resumen de discusiones de scripts/platform-audit.js | current | 0 requieren atención de mantenimiento; 0 discusiones accionables sin respuesta aceptada | repetir antes del release |
| Convertir el cierre de ITO-44 en un panel repetible | npm run operator:dashboard | complete | existe el script de paquete operator:dashboard | mantener el panel generado adjunto como evidencia de publicación |
| Paquete de vista previa de ECC 2.0 listo | docs/releases/2.0.0-rc.1/preview-pack-manifest.md | current | el manifiesto del paquete de vista previa y la puerta determinista de smoke están en el árbol | repetir el smoke de clean-checkout del preview pack antes de la publicación |
| Incluir con seguridad las habilidades especializadas de Hermes | docs/HERMES-SETUP.md y skills/hermes-imports/SKILL.md | current | los artefactos de configuración/importación de Hermes están cubiertos por el smoke del preview pack | repetir el smoke del preview pack antes de la revisión del release |
| Preparar las rutas de cambio de nombre, plugin de Claude y plugin de Codex | matriz de naming-and-publication, lista de verificación release-name-plugin-publication y publication-readiness | in_progress | existen la matriz de nomenclatura, la lista de verificación de publicación del release y las puertas de preparación del plugin | el tag/push real, el envío al marketplace y la elección final del canal siguen sujetos a aprobación |
| Preparar notas de release, artículos, tuits y notificaciones push | archivos sociales y de copy del release en docs/releases/2.0.0-rc.1 | in_progress | las notas de release, el hilo de X, el borrador de LinkedIn y el registro de URL están presentes | las URL finales en vivo de release/npm/plugin/facturación y la aprobación de publicación siguen pendientes |
| Avanzar la iteración empresarial de AgentShield | evidencia de PR de AgentShield más hoja de ruta empresarial | in_progress | la promoción de políticas de AgentShield `reviewItems` se integró en `87aec47`; la detección de deriva de endurecimiento del gestor de paquetes se integró en `28d08c7`; los pins de tiempo de ejecución de la acción del flujo de trabajo se actualizaron en `659f569`; la guía npm age-gate se corrigió en `ee585cd`; las salidas de la Action de endurecimiento del gestor de paquetes se integraron en `1124535`; las salidas de la Action de promoción de políticas y la evidencia de resumen del trabajo en tiempo de ejecución se integraron en `1593925`; los payloads de los tickets de revisión de flota y las trazas IOC actuales de Mini Shai-Hulud se integraron en `840952a`; ECC-Tools consume esas salidas en `8658951`, expone telemetría legible por operadores sobre estado/conteo/hash en `16c537f` y renderiza trazas de auditoría del juez de promoción hospedado en `05d4e82`; todo está reflejado en la hoja de ruta GA | profundizar la aprobación/lectura en vivo del operador después de las puertas de Marketplace/pagos |
| Avanzar los pagos nativos de ECC Tools y la app agnóstica de harness con IA nativa | evidencia de PR de ECC Tools, puerta de facturación, carriles de análisis hospedados | in_progress | la puerta de anuncio de facturación, los carriles de análisis hospedados, el consumo del resumen de flota de AgentShield, las rutas de evidencia de hallazgos hospedados, el enlace de políticas de ruta del harness, la telemetría de salidas de la Action de promoción, los detalles visibles para operadores de la salida de promoción, las trazas de auditoría del juez de promoción hospedado, la prevalidación del anuncio de facturación, la lectura agregada de KV de facturación de producción, la lectura de OAuth de Wrangler, la lectura de facturación de la cuenta objetivo, las puertas de estado de facturación de Marketplace con conocimiento de procedencia, los conteos saneados de procedencia de plan/acción de Marketplace, los controles de feedback de aprendizaje de equipo hospedados y la remediación de alertas de Dependabot de ECC-Tools están reflejados en la hoja de ruta GA | crear o verificar el estado de facturación del objetivo Pro administrado por Marketplace con procedencia del webhook, configurar la cuenta objetivo y INTERNAL_API_SECRET, luego volver a ejecutar la lectura del objetivo y la puerta de anuncio en vivo |
| Auditar, depurar o adjuntar trabajo heredado | docs/stale-pr-salvage-ledger.md e inventario heredado | current | el registro de rescate heredado y el inventario están actualizados; todos los extremos de localización están adjuntos a Linear ITO-55 para revisión manual del propietario del idioma | repetir el escaneo heredado antes del release |
| Mantener detallada la hoja de ruta de Linear y sincronizado el seguimiento del progreso | espejo del proyecto en Linear más contrato progress-sync | current | la sincronización en vivo de Linear y la superficie de progreso del proyecto están actuales; el contrato progress-sync define la ruta respaldada por archivos de elementos de trabajo/estado | repetir la actualización del estado de Linear/proyecto y la sincronización local de elementos de trabajo después de cada lote de merges significativo |
| Proporcionar observabilidad de ECC 2.0 para uso propio | puerta de preparación de observabilidad | complete | existen el comando observability:ready y el documento de preparación | la implementación en tiempo de ejecución/del panel puede continuar después de las puertas del release |
| Mantener actualizado el ciclo de protección contra Mini Shai-Hulud/TanStack | vigilancia de supply chain más runbook más endurecimiento del gestor de paquetes de AgentShield | current | la vigilancia programada de supply chain emite artefactos de actualización de IOC/fuente de advisories; el escáner de ECC cubre la persistencia del almacén de tokens gh-token-monitor; AgentShield ahora detecta IOCs conocidos de persistencia de herramientas de IA, deriva npm lifecycle/token, deriva no compatible de clave de edad npm y deriva de cooldown de pnpm/Yarn; la evidencia de vigilancia del head actual y las actualizaciones de evidencia de Linear ITO-57 del 18 de mayo están vigentes | repetir la actualización de advisories/fuentes y la sincronización de Linear después de cada lote importante de supply chain |

## Acciones principales

- `naming-and-plugin-publication`: el tag/push real, el envío al marketplace y la elección final del canal siguen sujetos a aprobación
- `release-notes-and-notifications`: las URL finales en vivo de release/npm/plugin/facturación y la aprobación de publicación siguen pendientes
- `agentshield-enterprise-iteration`: profundizar la aprobación/lectura en vivo del operador después de las puertas de Marketplace/pagos
- `ecc-tools-next-level`: crear o verificar el estado de facturación del objetivo Pro administrado por Marketplace con procedencia del webhook, configurar la cuenta objetivo y INTERNAL_API_SECRET, luego volver a ejecutar la lectura del objetivo y la puerta de anuncio en vivo

## Orden de trabajo siguiente

1. Regenerar este panel desde el commit final del release antes de registrar la evidencia de publicación.
2. Repetir la sincronización de estado de Linear/proyecto ITO-57 después del próximo lote significativo de merges o de la actualización de la fuente de advisories.
3. Crear o verificar el estado de facturación del objetivo Pro administrado por Marketplace con procedencia del webhook, configurar la cuenta objetivo y INTERNAL_API_SECRET, luego volver a ejecutar la lectura del objetivo y la puerta de anuncio en vivo antes de publicar el copy de pagos nativos.
4. Reanudar ITO-45, ITO-46 e ITO-56 solo después de que se actualicen el panel generado y las puertas finales del release.
