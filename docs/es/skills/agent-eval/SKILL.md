---
name: agent-eval
description: Head-to-head comparison of coding agents (Claude Code, Aider, Codex, etc.) on custom tasks with pass rate, cost, time, and consistency metrics
origin: ECC
tools: Read, Write, Edit, Bash, Grep, Glob
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->
# Habilidad de evaluación del agente

Una herramienta CLI liviana para comparar agentes de codificación cara a cara en tareas reproducibles. Cada pregunta "¿qué agente de codificación es mejor?" la comparación se basa en vibraciones: esta herramienta la sistematiza.

## Cuándo activar

- Comparación de agentes de codificación (Claude Code, Aider, Codex, etc.) en su propia base de código
- Medir el desempeño del agente antes de adoptar una nueva herramienta o modelo.
- Ejecutar comprobaciones de regresión cuando un agente actualiza su modelo o herramientas.
- Producir decisiones de selección de agentes respaldadas por datos para un equipo.

## Instalación

> **Nota:** Instale agente-eval desde su repositorio después de revisar la fuente.

## Conceptos básicos

### Definiciones de tareas YAML

Definir tareas de forma declarativa. Cada tarea especifica qué hacer, qué archivos tocar y cómo juzgar el éxito:

```yaml
name: add-retry-logic
description: Add exponential backoff retry to the HTTP client
repo: ./my-project
files:
  - src/http_client.py
prompt: |
  Add retry logic with exponential backoff to all HTTP requests.
  Max 3 retries. Initial delay 1s, max delay 30s.
judge:
  - type: pytest
    command: pytest tests/test_http_client.py -v
  - type: grep
    pattern: "exponential_backoff|retry"
    files: src/http_client.py
commit: "abc1234"  # pin to specific commit for reproducibility
```
### Aislamiento del árbol de trabajo de Git

Cada ejecución de agente obtiene su propio árbol de trabajo de git, sin necesidad de Docker. Esto proporciona aislamiento de reproducibilidad para que los agentes no puedan interferir entre sí ni corromper el repositorio base.

### Métricas recopiladas

| Métrica | Qué mide |
|--------|-----------------|
| Tasa de aprobación | ¿El agente produjo un código que pasa al juez? |
| Costo | Gasto de API por tarea (cuando esté disponible) |
| Hora | Segundos del reloj de pared hasta su finalización |
| Consistencia | Tasa de aprobación en ejecuciones repetidas (por ejemplo, 3/3 = 100%) |

## Flujo de trabajo

### 1. Definir tareas

Cree un directorio `tasks/` con archivos YAML, uno por tarea:

```bash
mkdir tasks
# Write task definitions (see template above)
```
### 2. Ejecutar agentes

Ejecute agentes contra sus tareas:

```bash
agent-eval run --task tasks/add-retry-logic.yaml --agent claude-code --agent aider --runs 3
```
Cada carrera:
1. Crea un nuevo árbol de trabajo de git a partir de la confirmación especificada.
2. Entrega el mensaje al agente.
3. Ejecuta los criterios del juez.
4. Registros de aprobación/rechazo, costo y tiempo

### 3. Comparar resultados

Generar un informe comparativo:

```bash
agent-eval report --format table
```

```
Task: add-retry-logic (3 runs each)
┌──────────────┬───────────┬────────┬────────┬─────────────┐
│ Agent        │ Pass Rate │ Cost   │ Time   │ Consistency │
├──────────────┼───────────┼────────┼────────┼─────────────┤
│ claude-code  │ 3/3       │ $0.12  │ 45s    │ 100%        │
│ aider        │ 2/3       │ $0.08  │ 38s    │  67%        │
└──────────────┴───────────┴────────┴────────┴─────────────┘
```
## Tipos de jueces

### Basado en código (determinista)

```yaml
judge:
  - type: pytest
    command: pytest tests/ -v
  - type: command
    command: npm run build
```
### Basado en patrones

```yaml
judge:
  - type: grep
    pattern: "class.*Retry"
    files: src/**/*.py
```
### Basado en modelos (LLM-como-juez)

```yaml
judge:
  - type: llm
    prompt: |
      Does this implementation correctly handle exponential backoff?
      Check for: max retries, increasing delays, jitter.
```
## Mejores prácticas

- **Comienza con 3 a 5 tareas** que representen tu carga de trabajo real, no ejemplos de juguete.
- **Realice al menos 3 pruebas** por agente para capturar la variación; los agentes no son deterministas
- **Fija el compromiso** en tu tarea YAML para que los resultados sean reproducibles a lo largo de días/semanas.
- **Incluye al menos un juez determinista** (pruebas, compilación) por tarea: los jueces de LLM agregan ruido
- **Seguimiento del costo junto con la tasa de aprobación**: un agente del 95 % a 10 veces el costo puede no ser la opción correcta
- **Versione las definiciones de sus tareas**: son elementos de prueba, trátelos como código

## Enlaces

- Repositorio: [github.com/joaquinhuigomez/agent-eval](https://github.com/joaquinhuigomez/agent-eval)