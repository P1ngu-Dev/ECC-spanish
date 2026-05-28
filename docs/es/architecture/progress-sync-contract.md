# Contrato de sincronización de progreso

ECC 2.0 sigue el estado de ejecución en GitHub, Linear, los handoffs locales y
la hoja de ruta del repo. Este contrato define la evidencia mínima necesaria
antes de que una actualización de estado pueda afirmar que una línea está al día.

## Fuentes de verdad

| Surface | Rol | Regla actual |
| --- | --- | --- |
| PRs/issues/discussions de GitHub | Cola pública y estado de revisión | Vuelve a comprobar los conteos en vivo antes de cada lote de merge significativo y antes de aprobar un release. |
| Proyecto de Linear | Hoja de ruta ejecutiva y actualización de estado para stakeholders | Usa documentos del proyecto y comentarios de proyecto/issue porque las actualizaciones de estado del proyecto están deshabilitadas en este workspace; crea o reutiliza issues para líneas de ejecución duraderas. |
| Handoff local | Continuidad duradera del operador | Actualiza el handoff activo después de cada lote de merge, vaciado de cola, gate de release omitido o acción externa bloqueada. |
| Hoja de ruta del repo | Espejo auditable de planificación | Mantén `docs/ECC-2.0-GA-ROADMAP.md` alineado con la evidencia de PRs mergeados y los gates sin resolver. |
| `scripts/work-items.js` | Puente de tracker local | Sincroniza PRs/issues de GitHub en el almacén SQLite de work-items para instantáneas de estado y seguimientos bloqueados. |

## Líneas de flujo

El espejo del repo usa estas líneas de flujo para que el trabajo de ECC no se
convierta en un solo backlog indiferenciado:

- Higiene de cola y rescate de trabajo obsoleto
- Release, naming, publicación de plugins y anuncios
- Cumplimiento de adaptadores de harness
- Observabilidad local, HUD/estado y control de sesión
- Evaluator/RAG y bucles de harness auto-mejorables
- Plataforma de seguridad enterprise AgentShield
- Facturación de ECC Tools, verificaciones de riesgo de PR, análisis profundo y sincronización con Linear
- Auditoría de artefactos legados y colas de traducción/revisión manual

Cada línea de flujo necesita un artefacto dueño, una fuente de evidencia actual y
una siguiente acción. Una línea no está al día si falta cualquiera de esos tres
campos.

## Actualización de lote de merge significativo

Después de un lote de merge significativo, actualiza Linear y el handoff con:

1. Conteos actuales de la cola pública para los repos de GitHub rastreados.
2. Números de PR mergeados, IDs de commit y evidencia de validación.
3. Gates de release modificados, si los hay.
4. Trabajo aplazado o omitido y la razón explícita.
5. Uno o dos siguientes slices de implementación.

Cuando las actualizaciones de estado de Linear no estén disponibles, usa un
documento del proyecto más comentarios de proyecto/issue en lugar de crear
issues de marcador de posición. Hay capacidad de issues para líneas de ejecución
duraderas, pero no uses esa capacidad como sustituto de un estado de proyecto
respaldado por evidencia. Crea o reutiliza issues con título exacto solo cuando
la línea necesite un responsable duradero, y vincula esos issues a la evidencia
del repo.

## Límite de tiempo real

La ruta local de tiempo real está respaldada por archivos de forma predeterminada:

- `node scripts/work-items.js sync-github --repo <owner/repo>` importa el estado
  actual de PRs e issues de GitHub al almacén SQLite de work-items.
- `node scripts/status.js --json` y `node scripts/work-items.js list --json`
  exponen el estado local para un HUD, un handoff o una sincronización posterior
  con Linear.
- Linear sigue siendo la superficie externa de estado; el repo no requiere
  telemetría alojada para estar listo para release.

La telemetría alojada, como PostHog, puede añadirse más adelante, pero debe
consumir el mismo modelo de eventos en lugar de convertirse en una segunda
fuente de verdad.

## Gate de release

No publiques, etiquetes, anuncies, envíes paquetes de marketplace ni afirmes
disponibilidad de plugins basándote solo en este contrato. La preparación para
release todavía requiere los documentos de evidencia de publication-readiness,
comprobaciones frescas de cola, comprobaciones de paquetes, comprobaciones de
plugins y aprobación explícita del maintainer.
