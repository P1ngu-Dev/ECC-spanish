# Contrato de HUD de estado y control de sesión

Este contrato define el payload de estado portable que ECC usa para superficies
locales del operador, handoffs y futuros HUDs. Es intencionalmente neutro al
harness: una statusline de Claude Code, un panel de Codex, una sesión de dmux,
una ejecución de OpenCode o un flujo solo terminal pueden emitir datos parciales
sin cambiar los nombres de los campos.

El ejemplo canónico vive en
[`examples/hud-status-contract.json`](../../examples/hud-status-contract.json).

## Forma del payload

Cada payload de estado usa `schema_version: "ecc.hud-status.v1"` y mantiene
estables estas secciones de nivel superior:

| Field | Propósito | Fuente primaria |
|---|---|---|
| `context` | Modelo, harness, repo, rama, worktree, id de sesión y presión de la ventana de contexto | stdin de statusline, git, adapters de sesión |
| `toolCalls` | Conteos recientes de herramientas, llamadas pendientes, llamadas obsoletas y último evento de herramienta | `loop-status`, `tool-usage.jsonl`, puente de hooks |
| `activeAgents` | Trabajadores/subagentes actuales, estado en runtime, rama, worktree, objetivo y rutas de handoff | snapshots de dmux/orquestación |
| `todos` | Tarea actual en curso y conteos de todos | todos de Claude, archivos de tareas locales, metadatos de plan |
| `checks` | Estado de validación local y remota con URLs de comandos/comprobación cuando estén disponibles | CI, comandos locales, gates de release |
| `cost` | Gasto de sesión, conteos de tokens, presupuesto y tendencia | tracker de coste, puente de métricas |
| `risk` | Estado de atención, presión de conflicto, llamadas obsoletas, worktree sucio y flags de revisión manual | gates de preparación, git, estado de cola |
| `queueState` | Conteos de PR/issue/discussion de GitHub, cola de conflictos, cola de merge y cola de rescate de stale | sincronización de GitHub, work items |
| `sessionControls` | Acciones de operador soportadas para el target actual | CLI de ECC, dmux, git/GitHub |
| `sync` | Estado de publicación de Linear, GitHub y handoff | actualizaciones de estado, work items, escritor de handoff |

Los campos pueden ser `null`, arrays vacíos o `"unknown"` cuando un harness no
puede exponer la señal. Los productores no deben inventar nombres incompatibles.
Los consumidores deben renderizar las secciones faltantes como no disponibles,
no como verdes.

## Controles de sesión

El vocabulario mínimo de control de sesión es:

| Control | Significado |
|---|---|
| `create` | Iniciar una nueva ejecución aislada, worktree o plan de orquestación |
| `resume` | Volver a adjuntar una sesión existente o un target histórico |
| `status` | Emitir el payload actual sin mutar el estado |
| `stop` | Solicitar una parada ordenada o marcar la sesión como completada |
| `diff` | Mostrar el diff actual del working tree o del trabajador |
| `pr` | Abrir o inspeccionar el pull request vinculado |
| `mergeQueue` | Mostrar ítems listos para merge, bloqueados y en espera de checks |
| `conflictQueue` | Mostrar PRs o worktrees sucios/en conflicto que necesitan integración |

`sessionControls.supported` lista los controles disponibles para el harness
actual. `sessionControls.blocked` explica los controles no disponibles, por
ejemplo un token de GitHub ausente, ninguna sesión tmux o un adapter de solo lectura.

## Contrato de sincronización

La sección de sync separa trackers duraderos:

- `Linear` registra el id de actualización de estado del proyecto, la salud y
  si la creación de issues está bloqueada por la capacidad del workspace.
- `GitHub` registra el repo actual, los conteos de cola de PR/issue/discussion y
  el último PR mergeado o abierto vinculado a la sesión.
- `handoff` registra la ruta duradera del handoff Markdown y si se escribió
  después del último lote.

Esto hace explícito el seguimiento de progreso en tiempo real sin requerir que
cada ejecución cree issues de Linear o comentarios de GitHub. Cuando la
capacidad de issues de Linear está bloqueada, el payload de estado todavía puede
demostrar progreso mediante actualizaciones del proyecto y handoffs del repo.

## Implementaciones actuales

- `ecc status --json` expone readiness, sesiones activas, ejecuciones de skills,
  salud de instalación, gobernanza y work items vinculados desde el almacén de
  estado SQLite.
- `ecc loop-status --json --write-dir <dir>` escribe snapshots de transcript en
  vivo y señales de atención para loops de larga duración.
- `ecc session-inspect <target> --write <path>` emite snapshots canónicos de
  sesión desde los adapters de dmux e historial de Claude.
- `scripts/hooks/ecc-statusline.js` renderiza señales compactas de modelo, tarea,
  coste, herramienta, archivo, duración, directorio y presión de contexto dentro
  de Claude Code.

El payload `ecc.hud-status.v1` es el contrato outer común al que estas
superficies pueden proyectarse antes de que ECC crezca hasta tener un HUD de
pantalla completa dedicado.
