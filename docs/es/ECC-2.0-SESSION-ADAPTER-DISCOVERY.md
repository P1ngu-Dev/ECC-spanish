# Descubrimiento del Adaptador de Sesión ECC 2.0

## Propósito

Este documento convierte la dirección del plano de control de ECC 2.0 del 11 de marzo en un diseño concreto de adaptador y snapshot, fundamentado en el código de orquestación que ya existe en este repositorio.

## Substrato Implementado Actual

El repositorio ya cuenta con un substrato real de orquestación de primera pasada:

- `scripts/lib/tmux-worktree-orchestrator.js`
  aprovisiona paneles de tmux más worktrees de git aislados
- `scripts/orchestrate-worktrees.js`
  es el lanzador de sesión actual
- `scripts/lib/orchestration-session.js`
  recopila snapshots de sesión legibles por máquina
- `scripts/orchestration-status.js`
  exporta esos snapshots desde un nombre de sesión o archivo de plan
- `commands/sessions.md`
  ya expone conceptos adyacentes de historial de sesión desde el almacén local de Claude
- `scripts/lib/session-adapters/canonical-session.js`
  define la capa canónica de normalización `ecc.session.v1`
- `scripts/lib/session-adapters/dmux-tmux.js`
  envuelve el recopilador actual de snapshots de orquestación como el adaptador `dmux-tmux`
- `scripts/lib/session-adapters/claude-history.js`
  normaliza el historial de sesiones local de Claude como un segundo adaptador
- `scripts/lib/session-adapters/registry.js`
  selecciona adaptadores a partir de objetivos explícitos y tipos de objetivo
- `scripts/session-inspect.js`
  emite snapshots canónicos de solo lectura de la sesión a través del registro de adaptadores

En la práctica, ECC ya puede responder:

- qué workers existen en una sesión orquestada por tmux
- a qué panel está adjunto cada worker
- qué archivos de tarea, estado y handoff existen para cada worker
- si la sesión está activa y cuántos paneles/workers existen
- cómo se veía la sesión local más reciente de Claude en la misma forma canónica de snapshot que las sesiones de orquestación

Eso es suficiente para demostrar el substrato. Todavía no es suficiente para calificar como un plano de control general de ECC 2.0.

## Qué Modela Realmente El Snapshot Actual

El modelo de snapshot actual que sale de `scripts/lib/orchestration-session.js` tiene estos campos efectivos:

```json
{
  "sessionName": "workflow-visual-proof",
  "coordinationDir": ".../.claude/orchestration/workflow-visual-proof",
  "repoRoot": "...",
  "targetType": "plan",
  "sessionActive": true,
  "paneCount": 2,
  "workerCount": 2,
  "workerStates": {
    "running": 1,
    "completed": 1
  },
  "panes": [
    {
      "paneId": "%95",
      "windowIndex": 1,
      "paneIndex": 0,
      "title": "seed-check",
      "currentCommand": "codex",
      "currentPath": "/tmp/worktree",
      "active": false,
      "dead": false,
      "pid": 1234
    }
  ],
  "workers": [
    {
      "workerSlug": "seed-check",
      "workerDir": ".../seed-check",
      "status": {
        "state": "running",
        "updated": "...",
        "branch": "...",
        "worktree": "...",
        "taskFile": "...",
        "handoffFile": "..."
      },
      "task": {
        "objective": "...",
        "seedPaths": ["scripts/orchestrate-worktrees.js"]
      },
      "handoff": {
        "summary": [],
        "validation": [],
        "remainingRisks": []
      },
      "files": {
        "status": ".../status.md",
        "task": ".../task.md",
        "handoff": ".../handoff.md"
      },
      "pane": {
        "paneId": "%95",
        "title": "seed-check"
      }
    }
  ]
}
```

Esto ya es una carga útil útil para el operador. La limitación principal es que está vinculada implícitamente a un solo estilo de ejecución:

- identidad del panel tmux
- el slug del worker equivale al título del panel
- archivos de coordinación en Markdown
- reglas de búsqueda por archivo de plan o nombre de sesión

## Brecha Entre ECC 1.x Y ECC 2.0

ECC 1.x actualmente tiene dos superficies distintas de “sesión”:

1. historial local de sesiones de Claude
2. snapshots de sesión/runtime de orquestación

Esas superficies son adyacentes, pero no están unificadas.

La capa faltante de ECC 2.0 es una frontera de adaptador de sesión neutral al harness que pueda normalizar:

- workers orquestados por tmux
- sesiones simples de Claude
- sesiones de worktree de Codex
- sesiones de OpenCode
- futuras sesiones remotas o de GitHub/App

Sin esa capa de adaptador, cualquier futura interfaz de operador tendría que leer directamente detalles específicos de tmux y Markdown de coordinación.

## Frontera Del Adaptador

ECC 2.0 debería introducir un contrato canónico de adaptador de sesión.

Interfaz mínima sugerida:

```ts
type SessionAdapter = {
  id: string;
  canOpen(target: SessionTarget): boolean;
  open(target: SessionTarget): Promise<AdapterHandle>;
};

type AdapterHandle = {
  getSnapshot(): Promise<CanonicalSessionSnapshot>;
  streamEvents?(onEvent: (event: SessionEvent) => void): Promise<() => void>;
  runAction?(action: SessionAction): Promise<ActionResult>;
};
```

### Forma Canónica Del Snapshot

Carga útil canónica de primera pasada sugerida:

```json
{
  "schemaVersion": "ecc.session.v1",
  "adapterId": "dmux-tmux",
  "session": {
    "id": "workflow-visual-proof",
    "kind": "orchestrated",
    "state": "active",
    "repoRoot": "...",
    "sourceTarget": {
      "type": "plan",
      "value": ".claude/plan/workflow-visual-proof.json"
    }
  },
  "workers": [
    {
      "id": "seed-check",
      "label": "seed-check",
      "state": "running",
      "branch": "...",
      "worktree": "...",
      "runtime": {
        "kind": "tmux-pane",
        "command": "codex",
        "pid": 1234,
        "active": false,
        "dead": false
      },
      "intent": {
        "objective": "...",
        "seedPaths": ["scripts/orchestrate-worktrees.js"]
      },
      "outputs": {
        "summary": [],
        "validation": [],
        "remainingRisks": []
      },
      "artifacts": {
        "statusFile": "...",
        "taskFile": "...",
        "handoffFile": "..."
      }
    }
  ],
  "aggregates": {
    "workerCount": 2,
    "states": {
      "running": 1,
      "completed": 1
    }
  }
}
```

Esto conserva la señal útil que ya existe, mientras elimina los detalles específicos de tmux del contrato del plano de control.

## Primeros Adaptadores A Soportar

### 1. `dmux-tmux`

Envuelve la lógica que ya vive en `scripts/lib/orchestration-session.js`.

Este es el primer adaptador más fácil porque el substrato ya es real.

### 2. `claude-history`

Normaliza los datos que `commands/sessions.md` y las utilidades existentes del gestor de sesiones ya exponen:

- id / alias de sesión
- branch
- worktree
- ruta del proyecto
- recencia / tamaño de archivo / conteos de elementos

Esto proporciona una base no orquestada para ECC 2.0.

### 3. `codex-worktree`

Usa la misma forma canónica, pero respaldada por metadatos de ejecución nativos de Codex en lugar de suposiciones de tmux cuando estén disponibles.

### 4. `opencode`

Usa la misma frontera de adaptador una vez que los metadatos de sesión de OpenCode sean lo bastante estables para normalizarlos.

## Qué Debe Permanecer Fuera De La Capa De Adaptador

La capa de adaptador no debe ser responsable de:

- lógica de negocio para secuenciación de merges
- diseño de la UI del operador
- decisiones de precios o monetización
- selección del perfil de instalación
- la propia orquestación del ciclo de vida de tmux

Su trabajo es más acotado:

- detectar objetivos de sesión
- cargar snapshots normalizados
- opcionalmente transmitir eventos en tiempo de ejecución
- opcionalmente exponer acciones seguras

## Diseño Actual De Archivos

La capa de adaptador ahora vive en:

```text
scripts/lib/session-adapters/
  canonical-session.js
  dmux-tmux.js
  claude-history.js
  registry.js
scripts/session-inspect.js
tests/lib/session-adapters.test.js
tests/scripts/session-inspect.test.js
```

El parser actual de snapshots de orquestación ahora se consume como una implementación de adaptador, en lugar de permanecer como el único contrato del producto.

## Próximos Pasos Inmediatos

1. Agregar un tercer adaptador, probablemente `codex-worktree`, para que la abstracción vaya más allá de tmux y Claude-history.
2. Decidir si los snapshots canónicos necesitan campos separados `state` y `health` antes de comenzar el trabajo de UI.
3. Decidir si la transmisión de eventos pertenece a v1 o si debe quedar fuera hasta que la capa de snapshot demuestre su valor.
4. Construir paneles orientados al operador solo sobre el registro de adaptadores, no leyendo directamente los internos de orquestación.

## Preguntas Abiertas

1. ¿La identidad del worker debería identificarse por slug del worker, branch o UUID estable?
2. ¿Necesitamos campos separados `state` y `health` en la capa canónica?
3. ¿La transmisión de eventos debe formar parte de v1, o ECC 2.0 debería lanzar primero solo snapshots?
4. ¿Cuánta información de ruta debería redaccionarse antes de que los snapshots salgan de la máquina local?
5. ¿El registro de adaptadores debería vivir dentro de este repositorio a largo plazo, o moverse a la futura app del plano de control de ECC 2.0 una vez que la interfaz se estabilice?

## Recomendación

Trata la implementación actual de tmux/worktree como el adaptador `0`, no como la superficie final del producto.

El camino más corto hacia ECC 2.0 es:

1. preservar el substrato actual de orquestación
2. envolverlo en un contrato canónico de adaptador de sesión
3. agregar un adaptador que no sea tmux
4. solo entonces empezar a construir paneles de operador sobre esa base
