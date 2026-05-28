# Panel de preparación del operador de ECC

Este panel se genera con `npm run operator:dashboard`. Es una instantánea del operador, no una aprobación de lanzamiento.

Generado: 2026-05-20T03:14:39.338Z
Commit: 66733b511b70cf1cb501e8a3298b1cbd9968a9a0
Estado: trabajo pendiente

## Estado actual

| Área | Estado | Evidencia |
| --- | --- | --- |
| Cola de PR | Actual | 0 PR abiertos en los repositorios rastreados |
| Cola de issues | Actual | 0 issues abiertos en los repositorios rastreados |
| Discussions | Actual | 0 requieren intervención de mantenedor; 0 sin respuesta aceptada |
| Worktree local | Actual | 0 archivos sucios que bloqueen; 0 entradas sucias ignoradas |
| Generación del panel | Actual | auditoría de la plataforma lista: true; GitHub omitido: false |
| Publicación | No completa | lanzamiento, npm, plugin, facturación y avisos se rastrean abajo |

## Línea base de crecimiento

| Métrica | Actual | Objetivo | Brecha |
| --- | ---: | ---: | ---: |
| MRR | $1,728/mes | $10,000/mes | $8,272/mes |

Canales de crecimiento: GitHub Sponsors y patrocinadores socios OSS; suscripciones de ECC Tools Pro; contratos de consultoría e implementación; charlas, podcasts, demos en conferencias y seminarios web con socios.

## Lista de verificación de objetivo a artefacto

| Requisito objetivo | Artefacto o puerta | Estado | Evidencia | Brecha |
| --- | --- | --- | --- | --- |
| Mantener los PR públicos por debajo de 20 | `scripts/platform-audit.js` con barrido en vivo de GitHub más registro de limpieza de colas a nivel de propietario | actual | 0 PR abiertos en 5 repositorios rastreados; 0 PR abiertos a nivel de propietario después de la limpieza | repetir `platform:audit` y la búsqueda `gh` a nivel de propietario antes del lanzamiento |
| Mantener los issues públicos por debajo de 20 | `scripts/platform-audit.js` con barrido en vivo de GitHub más registro de limpieza de colas a nivel de propietario | actual | 0 issues abiertos en 5 repositorios rastreados; 0 issues abiertos a nivel de propietario después de la limpieza | repetir `platform:audit` y la búsqueda `gh` a nivel de propietario antes del lanzamiento |
| Responder y gestionar las Discussions del repositorio | resumen de Discussions de `scripts/platform-audit.js` | actual | 0 requieren intervención de mantenedor; 0 Discussions con respuesta aceptada pendiente | repetir antes del lanzamiento |
| Convertir el cierre de ITO-44 en un panel repetible | `npm run operator:dashboard` | completo | existe el script de paquete `operator:dashboard` | mantener el panel generado adjunto a la evidencia de publicación |
| Paquete de vista previa de ECC 2.0 listo | `docs/releases/2.0.0-rc.1/preview-pack-manifest.md` | actual | el manifiesto del paquete de vista previa y la verificación determinista smoke están en el árbol | repetir la verificación smoke del paquete de vista previa en clean-checkout antes de publicar |
| Incluir de forma segura las skills especializadas de Hermes | `docs/HERMES-SETUP.md` y `skills/hermes-imports/SKILL.md` | actual | los artefactos de configuración/importación de Hermes están cubiertos por la verificación smoke del paquete de vista previa | repetir la verificación smoke del paquete de vista previa antes de la revisión del lanzamiento |
| Preparar las rutas de cambio de nombre, plugin de Claude y plugin de Codex | matrix de publicación y nombres más lista de verificación de publicación del nombre de lanzamiento más readiness de publicación | en progreso | existen la matriz de nombres, la lista de verificación de publicación del lanzamiento y las puertas de readiness del plugin | la etiqueta/push reales, el envío al marketplace y la elección final del canal siguen sujetos a aprobación |
| Preparar notas de lanzamiento, artículos, tweets y notificaciones push | archivos sociales y de copia de lanzamiento en `docs/releases/2.0.0-rc.1` | en progreso | están presentes las notas de lanzamiento, el hilo de X, el borrador de LinkedIn y el registro de URLs | las URLs finales en vivo de lanzamiento/npm/plugin/facturación y la aprobación de publicación siguen pendientes |
| Preparar el paquete final de aprobación del propietario | `docs/releases/2.0.0-rc.1/owner-approval-packet-2026-05-19.md` | actual | el paquete de aprobación del propietario cubre decisiones de lanzamiento, paquete, plugin, video, facturación, sociales y salida | revisar las aprobaciones del propietario desde el commit final del lanzamiento antes de cualquier publicación o acción de salida |
| Crear un centro de comando de lanzamiento de hipercrecimiento de segunda fase | `docs/releases/2.0.0/ecc-2-hypergrowth-release-command-center.md` más evidencia del 19 de mayo | actual | el MRR actual, el MRR objetivo, la brecha, la reclamación de lanzamiento, el canal de video, el plan de distribución y los límites de aprobación están en el árbol | actualizar después de cada cambio de MRR, canal o estado de aprobación antes del lanzamiento público |
| Producir la suite de video del lanzamiento de ECC 2.0 | `docs/releases/2.0.0-rc.1/video-suite-production.md` y `npm run release:video-suite` | actual | la puerta de la suite de video está lista con 15/15 activos fuente, 13/13 artefactos de suite, 12/12 candidatos de publicación, autoevaluación principal y cero segmentos de pantalla negra detectados, registrados en la evidencia del 19 de mayo | la aprobación final del propietario, la carga y las URLs públicas del video siguen sujetas a aprobación |
| Preparar el texto para patrocinadores, socios, consultoría, podcasts, charlas y Discussions | `docs/releases/2.0.0-rc.1/partner-sponsor-talks-pack.md` | en progreso | están redactados el outreach a patrocinadores, el DM para socios de plataforma, la introducción de consultoría, el pitch para charlas/podcasts, el anuncio de GitHub Discussion, los ganchos de CTA y la puerta de no enviar | reemplazar las URLs finales después de las puertas de publicación, luego obtener aprobación explícita antes de publicar por salida o desde cuentas personales |
| Avanzar la iteración empresarial de AgentShield | evidencia de PR de AgentShield más hoja de ruta empresarial | en progreso | la promoción de políticas de AgentShield `reviewItems` llegó en `87aec47`; la detección de deriva de hardening del gestor de paquetes llegó en `28d08c7`; los pines de runtime del workflow action se actualizaron en `659f569`; la guía de age-gate de npm se corrigió en `ee585cd`; los outputs de la Action de hardening del gestor de paquetes llegaron en `1124535`; los outputs de la Action de promoción de políticas y la evidencia del resumen de job de smoke runtime llegaron en `1593925`; los payloads de tickets de revisión de flota y las pistas IOC actuales de Mini Shai-Hulud llegaron en `840952a`; ECC-Tools consume esos outputs en `8658951`, expone telemetría legible para operador de estado/paquete/conteo/hash en `16c537f`, y renderiza trazas de auditoría del juez de promoción alojado en `05d4e82`; todo está reflejado en la hoja de ruta GA | profundizar la aprobación/respuesta del operador en vivo después de las puertas de Marketplace/pagos |
| Avanzar los pagos nativos y la app agnóstica al harness de ECC Tools | evidencia de PR de ECC Tools, puerta de facturación, carriles de análisis alojados | en progreso | la puerta de anuncio de facturación, la puerta de anuncio de target seleccionado, la ruta de operador mediante archivo env de la puerta de facturación, la ruta de bearer del operador no disruptiva, los carriles de análisis alojados, el consumo del resumen de flota de AgentShield, las rutas alojadas de evidencia de hallazgos, el enlace de políticas de rutas del harness, la telemetría de outputs de la Action de promoción de políticas, los detalles de output de promoción visibles para el operador, las trazas de auditoría del juez de promoción alojado, el preflight del anuncio de facturación, la lectura KV agregada de facturación de producción, la lectura del target seleccionado de Wrangler, la lectura de facturación de la cuenta objetivo, las puertas de estado de facturación del Marketplace con conocimiento de procedencia, los conteos de procedencia saneados de plan/action del Marketplace, la selección de target Pro listo del Marketplace, los controles alojados de feedback de aprendizaje en equipo y la remediación de alertas Dependabot de ECC-Tools están reflejados en la hoja de ruta GA | repetir la lectura KV y la puerta de anuncio del target seleccionado inmediatamente antes del lanzamiento; mantener la copia de pagos nativos detrás del lanzamiento final, el plugin, la URL y las puertas de aprobación del propietario |
| Auditar, podar o adjuntar trabajo heredado | `docs/stale-pr-salvage-ledger.md` e inventario heredado | actual | el registro de salvamento heredado y el inventario están al día; todos los extremos de localización están adjuntos a Linear ITO-55 para revisión manual por parte del responsable del idioma | repetir el escaneo heredado antes del lanzamiento |
| Mantener detallada la hoja de ruta de Linear y sincronizado el seguimiento del progreso | espejo del proyecto Linear más contrato de sincronización de progreso | actual | la sincronización en vivo de Linear está al día con los comentarios de la puerta de lanzamiento de Marketplace Pro del 20 de mayo en ITO-61 y la hoja de ruta de la plataforma ECC; el contrato de sincronización de progreso define la ruta de ítems de trabajo/estado respaldada por archivos | repetir la actualización de estado de Linear/proyecto y la sincronización local de ítems de trabajo después de cada lote significativo de merges |
| Proveer observabilidad de ECC 2.0 para uso propio | puerta de readiness de observabilidad | completo | existen el comando `observability:ready` y el documento de readiness | la implementación en tiempo de ejecución/del panel puede continuar después de las puertas de lanzamiento |
| Mantener actual el ciclo de protección contra Mini Shai-Hulud/TanStack | vigilancia de la cadena de suministro más runbook más hardening del gestor de paquetes de AgentShield | actual | la vigilancia programada de la cadena de suministro emite artefactos de actualización de fuentes/advisories; el escáner de ECC cubre la persistencia del almacén de tokens `gh-token-monitor`; AgentShield ahora detecta IOCs conocidos de persistencia de herramientas de IA, deriva de lifecycle/token de npm, deriva no compatible de age-key de npm y deriva de cooldown de pnpm/Yarn; la evidencia de vigilancia de current-head y las actualizaciones de evidencia de Linear ITO-57 del 18 de mayo están al día | repetir la actualización de advisory/source y la sincronización de Linear después de cada lote significativo de cadena de suministro |

## Acciones principales

- `naming-and-plugin-publication`: la etiqueta/push reales, el envío al marketplace y la elección final del canal siguen sujetos a aprobación
- `release-notes-and-notifications`: las URLs finales en vivo de lanzamiento/npm/plugin/facturación y la aprobación de publicación siguen pendientes
- `partner-sponsor-talks-pack`: reemplazar las URLs finales después de las puertas de publicación, luego obtener aprobación explícita antes de publicar por salida o desde cuentas personales
- `agentshield-enterprise-iteration`: profundizar la aprobación/respuesta del operador en vivo después de las puertas de Marketplace/pagos
- `ecc-tools-next-level`: repetir la lectura KV y la puerta de anuncio del target seleccionado inmediatamente antes del lanzamiento; mantener la copia de pagos nativos detrás del lanzamiento final, el plugin, la URL y las puertas de aprobación del propietario

## Próximo orden de trabajo

1. Regenerar este panel desde el commit final del lanzamiento antes de registrar la evidencia de publicación.
2. Revisar el paquete de aprobación del propietario desde el commit final del lanzamiento y aprobar, diferir o bloquear cada puerta de publicación y salida.
3. Revisar los candidatos del video principal de lanzamiento aprobados por el propietario, elegir los cortes finales, cargarlos después de la aprobación y adjuntar las URLs públicas del video al paquete de lanzamiento.
4. Reemplazar las URLs finales de lanzamiento, npm, plugin, facturación y video en el paquete de socios/patrocinadores/charlas, luego obtener aprobación explícita antes de publicar.
5. Repetir la sincronización de estado de Linear/proyecto de ITO-57 después del próximo lote significativo de merges o actualización de fuente de advisories.
6. Repetir la lectura KV y la puerta de anuncio de facturación del target seleccionado inmediatamente antes del lanzamiento; mantener la copia de pagos nativos detrás del lanzamiento final, el plugin, la URL y las puertas de aprobación del propietario.
