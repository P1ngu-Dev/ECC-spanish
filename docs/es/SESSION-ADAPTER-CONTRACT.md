# Contrato del Adaptador de Sesión

Este documento define el contrato canónico de instantánea de sesión de ECC para
`ecc.session.v1`.

El contrato se implementa en
`scripts/lib/session-adapters/canonical-session.js`. Este documento es la
especificación normativa para adaptadores y consumidores.

## Propósito

ECC tiene múltiples fuentes de sesión:

- sesiones de worktree orquestadas por tmux
- historial de sesiones local de Claude
- futuros harnesses y backends del plano de control

Los adaptadores normalizan esas fuentes en una única forma de instantánea segura
para el plano de control, de modo que la inspección, la persistencia y las
futuras capas de UI no dependan de archivos específicos del harness ni de
detalles de ejecución.

## Instantánea Canónica

Todo adaptador DEBE devolver un objeto serializable en JSON con esta forma de
nivel superior:

```json
{
  "schemaVersion": "ecc.session.v1",
  "adapterId": "dmux-tmux",
  "session": {
    "id": "workflow-visual-proof",
    "kind": "orchestrated",
    "state": "active",
    "repoRoot": "/tmp/repo",
    "sourceTarget": {
      "type": "session",
      "value": "workflow-visual-proof"
    }
  },
  "workers": [
    {
      "id": "seed-check",
      "label": "seed-check",
      "state": "running",
      "health": "healthy",
      "branch": "feature/seed-check",
      "worktree": "/tmp/worktree",
      "runtime": {
        "kind": "tmux-pane",
        "command": "codex",
        "pid": 1234,
        "active": false,
        "dead": false
      },
      "intent": {
        "objective": "Inspect seeded files.",
        "seedPaths": ["scripts/orchestrate-worktrees.js"]
      },
      "outputs": {
        "summary": [],
        "validation": [],
        "remainingRisks": []
      },
      "artifacts": {
        "statusFile": "/tmp/status.md",
        "taskFile": "/tmp/task.md",
        "handoffFile": "/tmp/handoff.md"
      }
    }
  ],
  "aggregates": {
    "workerCount": 1,
    "states": {
      "running": 1
    },
    "healths": {
      "healthy": 1
    }
  }
}
```

## Campos Requeridos

### Nivel superior

| Campo | Tipo | Notas |
| --- | --- | --- |
| `schemaVersion` | string | DEBE ser exactamente `ecc.session.v1` para este contrato |
| `adapterId` | string | Identificador estable del adaptador, como `dmux-tmux` o `claude-history` |
| `session` | object | Metadatos canónicos de la sesión |
| `workers` | array | Registros canónicos de workers; puede estar vacío |
| `aggregates` | object | Conteos derivados de workers |

### `session`

| Campo | Tipo | Notas |
| --- | --- | --- |
| `id` | string | Identificador estable dentro del dominio del adaptador |
| `kind` | string | Familia de sesión de alto nivel, como `orchestrated` o `history` |
| `state` | string | Estado canónico de la sesión |
| `sourceTarget` | object | Procedencia del destino que abrió la sesión |

### `session.sourceTarget`

| Campo | Tipo | Notas |
| --- | --- | --- |
| `type` | string | Clase de búsqueda, como `plan`, `session`, `claude-history`, `claude-alias` o `session-file` |
| `value` | string | Valor bruto del destino o ruta resuelta |

### `workers[]`

| Campo | Tipo | Notas |
| --- | --- | --- |
| `id` | string | Identificador estable del worker en el alcance del adaptador |
| `label` | string | Etiqueta visible para el operador |
| `state` | string | Estado canónico del worker (ciclo de vida) |
| `health` | string | Salud canónica del worker (condición operativa) |
| `runtime` | object | Metadatos de ejecución/runtime |
| `intent` | object | Por qué existe este worker/sesión |
| `outputs` | object | Resultados estructurados y verificaciones |
| `artifacts` | object | Referencias a archivos/rutas propiedad del adaptador |

### `workers[].runtime`

| Campo | Tipo | Notas |
| --- | --- | --- |
| `kind` | string | Familia de runtime, como `tmux-pane` o `claude-session` |
| `active` | boolean | Si el runtime está activo ahora |
| `dead` | boolean | Si se sabe que el runtime está muerto/terminado |

### `workers[].intent`

| Campo | Tipo | Notas |
| --- | --- | --- |
| `objective` | string | Objetivo principal o título |
| `seedPaths` | string[] | Rutas de seed o contexto asociadas con el worker/sesión |

### `workers[].outputs`

| Campo | Tipo | Notas |
| --- | --- | --- |
| `summary` | string[] | Resultados completados o elementos de resumen |
| `validation` | string[] | Evidencia de validación o comprobaciones |
| `remainingRisks` | string[] | Riesgos abiertos, seguimientos o notas |

### `aggregates`

| Campo | Tipo | Notas |
| --- | --- | --- |
| `workerCount` | integer | DEBE ser igual a `workers.length` |
| `states` | object | Mapa de conteos derivado de `workers[].state` |
| `healths` | object | Mapa de conteos derivado de `workers[].health` |

## Campos Opcionales

Los campos opcionales PUEDEN omitirse, pero si se emiten DEBEN conservar el tipo
documentado:

| Campo | Tipo | Notas |
| --- | --- | --- |
| `session.repoRoot` | `string \| null` | Raíz del repo/worktree cuando se conozca |
| `workers[].branch` | `string \| null` | Nombre de la rama cuando se conozca |
| `workers[].worktree` | `string \| null` | Ruta del worktree cuando se conozca |
| `workers[].runtime.command` | `string \| null` | Comando activo cuando se conozca |
| `workers[].runtime.pid` | `number \| null` | ID de proceso cuando se conozca |
| `workers[].artifacts.*` | definido por el adaptador | Rutas de archivos o referencias estructuradas propiedad del adaptador |

Los campos opcionales específicos del adaptador pertenecen dentro de `runtime`,
`artifacts` u otros objetos anidados documentados. Los adaptadores NO DEBEN
inventar nuevos campos de nivel superior sin actualizar este contrato.

## Semántica de Estados

El contrato mantiene intencionalmente `session.state` y `workers[].state` lo
suficientemente flexibles para varios harnesses, pero los adaptadores actuales
usan estos valores:

- `dmux-tmux`
  - estados de sesión: `active`, `completed`, `failed`, `idle`, `missing`
  - estados de worker: derivados de archivos de estado del worker, por ejemplo
    `running` o `completed`
- `claude-history`
  - estado de sesión: `recorded`
  - estado de worker: `recorded`

Los consumidores DEBEN tratar las cadenas de estado desconocidas como valores
válidos específicos del adaptador y degradar con elegancia.

## Estrategia de Versionado

`schemaVersion` es la única puerta de compatibilidad. Los consumidores DEBEN
ramificarse en función de ella.

### Permitido en `ecc.session.v1`

- agregar nuevos campos anidados opcionales
- agregar nuevos IDs de adaptador
- agregar nuevos valores de cadena para estados
- agregar nuevos valores de cadena para health
- agregar nuevas claves de artifact dentro de `workers[].artifacts`

### Requiere una nueva versión de esquema

- eliminar un campo requerido
- renombrar un campo
- cambiar el tipo de un campo
- cambiar el significado de un campo existente de forma no compatible
- mover datos de un campo a otro manteniendo la misma cadena de versión

Si ocurre cualquiera de esas cosas, el productor DEBE emitir una nueva cadena
de versión como `ecc.session.v2`.

## Requisitos de Cumplimiento del Adaptador

Todo adaptador de sesión de ECC DEBE:

1. Emitir `schemaVersion: "ecc.session.v1"` exactamente.
2. Devolver una instantánea que satisfaga todos los campos y tipos requeridos.
3. Usar `null` para valores escalares opcionales desconocidos y arreglos vacíos
   para valores de lista desconocidos.
4. Mantener los detalles específicos del adaptador anidados bajo `runtime`,
   `artifacts` u otros objetos anidados documentados.
5. Asegurar que `aggregates.workerCount === workers.length`.
6. Asegurar que `aggregates.states` coincida con los estados emitidos de los
   workers.
7. Asegurar que `aggregates.healths` coincida con los valores de health emitidos
   de los workers.
7. Producir solo valores JSON serializables simples.
8. Validar la forma canónica antes de la persistencia o del uso posterior.
9. Persistir la instantánea canónica normalizada a través del shim de grabación
   de sesión. En este repo, ese shim primero intenta `scripts/lib/state-store` y
   recurre a un archivo de grabación JSON solo cuando el módulo de state store
   todavía no está disponible.

## Expectativas del Consumidor

Los consumidores DEBERÍAN:

- depender solo de los campos documentados para `ecc.session.v1`
- ignorar los campos opcionales desconocidos
- tratar `adapterId`, `session.kind` y `runtime.kind` como pistas de enrutado
  más que como enums exhaustivos
- esperar claves de artifact específicas del adaptador dentro de
  `workers[].artifacts`

Los consumidores NO DEBEN:

- inferir comportamiento específico del harness a partir de campos no
  documentados
- asumir que todos los adaptadores tienen panes de tmux, worktrees de git o
  archivos de coordinación en Markdown
- rechazar instantáneas solo porque una cadena de estado no sea familiar

## Mapeos Actuales de Adaptadores

### `dmux-tmux`

- Origen: `scripts/lib/orchestration-session.js`
- ID de sesión: nombre de la sesión de orquestación
- Kind de sesión: `orchestrated`
- Destino fuente de la sesión: ruta del plan o nombre de sesión
- Kind de runtime del worker: `tmux-pane`
- Artifacts: `statusFile`, `taskFile`, `handoffFile`

### `claude-history`

- Origen: `scripts/lib/session-manager.js`
- ID de sesión: short id de Claude cuando esté presente; de lo contrario, ID
  derivado del nombre de archivo de la sesión
- Kind de sesión: `history`
- Destino fuente de la sesión: destino explícito de historial, alias o archivo
  de sesión `.tmp`
- Kind de runtime del worker: `claude-session`
- Rutas seed de intent: analizadas desde `### Context to Load`
- Artifacts: `sessionFile`, `context`

## Referencia de Validación

La implementación del repositorio valida:

- estructura de objeto requerida
- campos de cadena requeridos
- flags booleanos de runtime
- outputs y rutas seed como arreglos de strings
- consistencia de conteos de aggregates

Los adaptadores deben tratar los fallos de validación como errores del contrato,
no como errores de entrada del usuario.

## Comportamiento del Respaldo de Grabación

El grabador de respaldo JSON es un shim temporal de compatibilidad para el
período previo a que llegue el state store dedicado. Su comportamiento es:

- la instantánea más reciente siempre se reemplaza en el lugar
- los registros de historial solo guardan cuerpos de instantánea distintos
- las lecturas repetidas sin cambios no agregan entradas duplicadas al historial

Esto evita que `session-inspect` y otras lecturas de sondeo hagan crecer el
historial sin límite para la misma instantánea de sesión sin cambios.
