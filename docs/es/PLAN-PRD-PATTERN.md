# Patrón Plan-PRD: flujo de planificación basado en Markdown por etapas

Un flujo de planificación ligero, alineado con el SDLC, en el que cada fase del ciclo de vida produce un **archivo de preparación** en Markdown que se puede confirmar en el repositorio y que el siguiente comando consume.

> Versión corta: `/plan-prd` escribe un PRD, `/plan` escribe un plan, la skill `tdd-workflow` lo implementa y `/pr` lo entrega. Cada flecha es un archivo en disco, no una conversación en memoria.

## Funcionalidad: archivos de preparación en Markdown

Cada artefacto de planificación es un archivo `.md` simple bajo `.claude/`:

```
.claude/
  prds/      # Documentos de requisitos del producto creados por /plan-prd
  plans/     # Planes de implementación creados por /plan
  reviews/   # Artefactos de revisión de código creados por /code-review
```

Estos archivos son:

- **Markdown simple** — legible para humanos, difeable en PRs, apto para grep en la CLI.
- **Se pueden confirmar** — agrégalos al repo junto con el código para que la intención viaje con la implementación.
- **Componibles** — cada comando acepta el archivo de la etapa anterior como su `$ARGUMENTS`, por lo que la cadena de herramientas se compone mediante rutas y no mediante estado en contexto.
- **Reanudables** — cierra la sesión, abre una nueva mañana y vuelve a pasar la ruta del archivo.

## Flujo

```
┌───────────────────────────┐
│ /plan-prd "<idea>"        │  Fase de requisitos
│  → .claude/prds/X.prd.md  │   Problema · Usuarios · Hipótesis · Alcance
└─────────────┬─────────────┘
              │
              ▼
┌───────────────────────────┐
│ /plan <ruta-prd>          │  Fase de diseño
│  → .claude/plans/X.plan.md│   Patrones · Archivos · Tareas · Validación
└─────────────┬─────────────┘
              │
              ▼
┌───────────────────────────┐
│ skill tdd-workflow         │  Fase de implementación
│  → código + pruebas       │   Primero pruebas, diff mínimo
└─────────────┬─────────────┘
              │
              ▼
┌───────────────────────────┐
│ /pr                        │  Fase de entrega
│  → PR en GitHub            │   Enlaces de vuelta al PRD + plan
└───────────────────────────┘
```

Cada bloque es una **puerta**. Puedes:

- Detenerte entre puertas — el artefacto persiste.
- Reiniciar desde cualquier puerta usando la ruta del artefacto.
- Omitir puertas para trabajo pequeño — alimentar `/plan` con texto libre e ignorar `/plan-prd`.
- Ejecutar una puerta de forma independiente — `/plan "refactor X"` produce un plan conversacional sin artefacto.

## Por qué `/plan-prd` es adicional a `/plan`

Responden preguntas distintas. Mezclarlos provoca expansión de alcance.

| Comando | Responde | Fase del SDLC | Artefacto |
|---|---|---|---|
| `/plan-prd` | *¿Qué problema? ¿Para quién? ¿Cómo sabemos que terminamos?* | Requisitos | `.claude/prds/{name}.prd.md` |
| `/plan` | *¿Qué archivos, patrones y tareas satisfacen el requisito?* | Diseño + estrategia de implementación | `.claude/plans/{name}.plan.md` (modo PRD) o en línea (modo texto) |

### ¿Por qué no combinarlos?

- **Separación de responsabilidades.** Los PRD preguntan *por qué*; los planes preguntan *cómo*. Agruparlos crea un comando enorme que hace ambas cosas mal, como demostró el antiguo par `/prp-prd` → `/prp-plan` (interrogatorio de 8 fases con tablas de fase de implementación mezcladas en los requisitos).
- **Audiencias distintas.** Quien revisa un PRD no necesita rutas de archivos ni comandos de type-check. Quien lee un plan no necesita la fase de investigación de mercado.
- **Vida útil distinta.** Un PRD puede permanecer estable mientras su plan se reescribe varias veces a medida que cambian las suposiciones de implementación.
- **Paso opcional.** Muchos cambios (correcciones de errores, refactors pequeños, adiciones de un solo archivo) no necesitan un PRD. `/plan` por sí solo basta. Forzar un PRD en cada cambio es burocracia.

### Cuándo usar cada uno

Usa `/plan-prd` cuando:

- El alcance es incierto o está en disputa.
- Varias partes interesadas deben alinearse sobre el problema antes de plantear la solución.
- El cambio es lo bastante grande como para que escribir la hipótesis cueste menos que reabrir el alcance en mitad de la implementación.

Usa `/plan` directamente cuando:

- Los requisitos ya están claros (un reporte de error, un refactor acotado, una migración conocida).
- El trabajo es lo bastante pequeño como para que un plan conversacional + una puerta de confirmación sea suficiente.
- Ya tienes un PRD — pásalo a `/plan` y omite `/plan-prd`.

## Uso

### Flujo completo (funcionalidad con alcance poco claro)

```bash
# 1. Redacta el PRD
/plan-prd "Límites de tasa por usuario en la API pública"

# → .claude/prds/per-user-rate-limits.prd.md creado
# Responde las preguntas de encuadre, aporta evidencia, define hipótesis y alcance.

# 2. Elige el siguiente hito pendiente y produce un plan
/plan .claude/prds/per-user-rate-limits.prd.md

# → .claude/plans/per-user-rate-limits.plan.md creado
# El plan incluye patrones a replicar, archivos a modificar y comandos de validación.
# La tabla Delivery Milestones del PRD actualiza la fila seleccionada a `in-progress`.

# 3. Implementa primero con pruebas
Usa la skill tdd-workflow

# 4. Abre el PR
/pr
# → El cuerpo del PR referencia automáticamente .claude/prds/... y .claude/plans/...
```

### Flujo rápido (el alcance ya está claro)

```bash
/plan "Agregar reintento con backoff exponencial al notificador"
# Planificación conversacional, sin artefacto.
# Confirma y luego usa la skill tdd-workflow.
```

### Referenciar un PRD existente desde otro lugar

```bash
# El PRD fue escrito por otra persona y vive en tu repo
/plan docs/rfcs/0042-rate-limiting.prd.md
```

`/plan` detecta cualquier ruta `.prd.md` y cambia al modo artefacto, analizando la tabla Delivery Milestones.

## Por qué los archivos de preparación superan al estado en contexto

- **Transferibles**: pasa la ruta del PRD a una sesión nueva y quedas al día — sin repetir una conversación larga.
- **Auditables**: quien revisa el PR ve *lo que intentaste* junto a *lo que construiste*.
- **Versionables**: el archivo de preparación evoluciona en la historia de git, igual que el código.
- **Procesables por máquina**: `/plan` elige programáticamente el siguiente hito pendiente; `/pr` enlaza programáticamente los artefactos en el cuerpo del PR. Sin ingeniería de prompts.

## Comandos relacionados

- `/plan-prd` — requisitos (punto de entrada de este patrón).
- `/plan` — planificación (consume PRD o texto libre).
- skill `tdd-workflow` — implementación primero con pruebas.
- `/pr` — abre un PR que referencia PRD y planes.
- `/code-review` — revisa diffs locales o PRs; detecta automáticamente `.claude/prds/` y `.claude/plans/` como contexto.

## Compatibilidad

Este patrón añade comandos nativos de ECC basados en archivos de preparación junto con el conjunto existente de comandos `prp-*`. Los comandos PRP heredados siguen disponibles para flujos PRP más profundos y para usuarios que ya tienen artefactos `.claude/PRPs/`.

- `/plan-prd` es el punto de entrada ligero de requisitos para `.claude/prds/`.
- `/plan` puede consumir archivos `.prd.md` y producir artefactos `.claude/plans/` sin requerir el diseño heredado del directorio PRP.
- `/pr` es el comando nativo de ECC para crear PRs y puede referenciar `.claude/prds/` y `.claude/plans/`.
- `/prp-prd`, `/prp-plan`, `/prp-implement`, `/prp-commit` y `/prp-pr` siguen siendo comandos válidos del flujo heredado/profundo.
