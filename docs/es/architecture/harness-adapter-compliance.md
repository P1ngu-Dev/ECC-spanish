# Matriz de cumplimiento de adaptadores de harness

Esta matriz es la puerta de entrada pública para equipos que quieren usar ECC en
más de un harness de coding. Convierte la arquitectura cross-harness en una
tarjeta de puntuación práctica: qué funciona hoy, qué es solo de instrucciones,
qué necesita un adapter y qué evidencia debe recopilar un operador antes de
confiar en una configuración.

Las unidades duraderas de ECC permanecen en fuentes compartidas:

- `skills/*/SKILL.md`
- `rules/`
- `commands/`
- `hooks/hooks.json`
- `scripts/hooks/`
- configs de referencia MCP
- contratos de sesión y observabilidad

Los archivos específicos del harness solo deben adaptar la carga, la forma del
evento, los nombres de comandos o los límites de la plataforma.

## Estados de cumplimiento

| State | Significado |
| --- | --- |
| Native | ECC puede instalar o verificar la superficie directamente para este harness. |
| Adapter-backed | ECC tiene un adapter, plugin o superficie de paquete delgada, pero la paridad difiere según el harness. |
| Instruction-backed | ECC puede proporcionar la guía y los archivos, pero el harness no expone la superficie runtime de hooks/sesión que ECC necesita para hacer cumplir las reglas. |
| Reference-only | La herramienta es útil como presión de diseño o runtime externo, pero ECC todavía no ofrece un instalador o adapter directo para ella. |

## Matriz

La matriz de abajo se renderiza desde
`scripts/lib/harness-adapter-compliance.js` y se verifica con
`npm run harness:adapters -- --check`.

<!-- harness-adapter-compliance:matrix-start -->
| Harness or runtime | State | Supported assets | Unsupported or different surfaces | Install or onramp | Verification command | Risk notes |
| --- | --- | --- | --- | --- | --- | --- |
| Claude Code | Native | Assets del plugin Claude; skills; commands; hooks; config MCP; workflows orientados a statusline | Los hooks nativos de Claude no implican paridad en otros harnesses | `./install.sh --profile minimal --target claude`; instalación del plugin de Claude | `npm run harness:audit -- --format json`; `node scripts/session-inspect.js --list-adapters` | Evita cargar todos los skills por defecto; mantén los hooks opt-in e inspeccionables. |
| Codex | Instruction-backed | `AGENTS.md`; metadatos del plugin Codex; skills; config MCP de referencia; patrones de comandos | La aplicación nativa de hooks y la semántica de slash-command de Claude no son equivalentes | `./install.sh --profile minimal --target codex`; revisión local de `AGENTS.md` | `npm run harness:audit -- --format json` | Trata los hooks como texto de política salvo que exista una superficie nativa de hooks en Codex. |
| OpenCode | Adapter-backed | Metadatos de paquete/plugin de OpenCode; skills compartidos; config MCP; patrones de adapter de eventos | Los nombres de eventos, el empaquetado de plugins y el dispatch de comandos difieren de Claude Code | Superficie de paquete o plugin de OpenCode desde este repo | `node tests/scripts/build-opencode.test.js`; `npm run harness:audit -- --format json` | Mantén la lógica de hooks en scripts compartidos y adapta solo la forma del evento en el borde. |
| Cursor | Adapter-backed | Rules de Cursor; skills locales del proyecto; adapter de hooks; scripts compartidos | Los eventos de hook de Cursor y la carga de reglas difieren de Claude Code | `./install.sh --profile minimal --target cursor` | `node tests/lib/install-targets.test.js`; `npm run harness:audit -- --format json` | Los adapters de Cursor deben preservar las reglas existentes del proyecto y evitar sobrescrituras silenciosas. |
| Gemini | Instruction-backed | Instrucciones locales del proyecto Gemini; skills compartidos; rules; docs de compatibilidad | No hay paridad completa de hooks de ECC; los ports del ecosistema deben documentar el desvío respecto a ECC upstream | `./install.sh --profile minimal --target gemini` | `node tests/lib/install-targets.test.js` | Trata los ports de Gemini como adapters del ecosistema hasta que se validen de extremo a extremo dentro de Gemini CLI. |
| Zed | Adapter-backed | Ajustes del proyecto Zed; rules del proyecto aplanadas; skills compartidos; commands; agents | Los agentes externos de Zed y los permisos nativos del Agent Panel no son hooks de Claude | `./install.sh --profile minimal --target zed` | `node tests/lib/install-targets.test.js`; `npm run harness:audit -- --format json` | Mantén los ajustes del proyecto conservadores y no copies secretos BYOK/OpenRouter en `.zed/`. |
| dmux | Adapter-backed | snapshots de sesión; estado de orquestación tmux/worktree; exports de handoff | dmux es un runtime de orquestación, no un target de instalación para skills/rules | `node scripts/session-inspect.js --list-adapters`; inspección del target de sesión de dmux | `node tests/lib/session-adapters.test.js` | Trata los eventos de dmux como señales de sesión/runtime, no como sustituto de la validación del repo. |
| Orca | Reference-only | ciclo de vida de worktree; estado de revisión; notificación; presión de diseño de identidad del provider | Hoy no hay instalador ni adapter directo de ECC | Úsalo como target de comparación para requisitos de estado de worktree/sesión | `npm run observability:ready` | No importes supuestos específicos del producto; convierte las lecciones en campos de evento de ECC. |
| Superset | Reference-only | presets de workspace; bucles de review con agentes paralelos; presión de diseño de aislamiento de worktree | Hoy no hay instalador ni adapter directo de ECC | Úsalo como target de comparación para la taxonomía de presets de workspace | `npm run observability:ready` | Mantén ECC portable; no exijas un workspace de escritorio para obtener valor básico. |
| Ghast | Reference-only | agrupación de panes nativa de terminal; agrupación por cwd; búsqueda; notificaciones | Hoy no hay instalador ni adapter directo de ECC | Úsalo como target de comparación para agrupación de sesiones orientada a terminal | `node scripts/session-inspect.js --list-adapters` | Preserva la ergonomía de terminal antes de añadir supuestos de UI visual. |
| Terminal-only | Native | skills; rules; commands; scripts; auditoría de harness; readiness de observabilidad; handoffs | Sin UI externa, sin control automático de sesión salvo que se ejecuten scripts explícitamente | Clona el repo; ejecuta comandos directamente; usa perfil mínimo para instalaciones de proyecto | `npm run harness:audit -- --format json`; `npm run observability:ready` | Este es el contrato de respaldo; cada adapter de nivel superior debería degradar a él. |
<!-- harness-adapter-compliance:matrix-end -->

## Onramp de tarjeta de puntuación

Usa esta secuencia antes de pedirle a ECC que haga más autónoma una
configuración de equipo o repo:

```bash
npm run harness:adapters -- --check
npm run harness:audit -- --format json
npm run observability:ready
node scripts/session-inspect.js --list-adapters
node scripts/loop-status.js --json --write-dir .ecc/loop-status
```

Lee el resultado como una tarjeta de puntuación de configuración, no como una
insignia de producto:

- `harness:adapters -- --check` demuestra que esta matriz pública sigue
  coincidiendo con los datos fuente del adapter y los campos de evidencia
  requeridos.
- `harness:audit` puntúa cobertura de herramientas, eficiencia de contexto,
  gates de calidad, persistencia de memoria, cobertura de evals, guardrails de
  seguridad y eficiencia de coste.
- `observability:ready` demuestra que el repo todavía expone señales locales de
  estado, sesión, actividad de herramientas, registro de riesgo y onramp de
  release.
- `session-inspect --list-adapters` muestra qué superficies de sesión son
  realmente inspeccionables en el entorno actual.
- `loop-status --json` crea un payload de handoff/estado legible por máquina
  para ejecuciones autónomas más largas.

## Contrato de tarjeta de puntuación respaldada por datos

Cada registro de adapter expone:

- `id`
- `state`
- `supported_assets`
- `unsupported_surfaces`
- `install_or_onramp`
- `verification_commands`
- `risk_notes`
- `last_verified_at`
- `owner`
- `source_docs`

El validador falla si una afirmación pública de adapter no tiene ruta de
instalación, comando de verificación, nota de riesgo, owner, doc fuente o fecha
de verificación.

## Reglas operativas

- Prefiere adapters pequeños y aditivos frente a forks específicos del harness
  del mismo workflow.
- No declares un harness como nativo hasta que el adapter tenga una ruta de
  instalación y un comando de verificación.
- Mantén honestas las superficies de Codex, Gemini y Zed cuando la aplicación es
  de instrucciones y no de runtime.
- Trata las herramientas de referencia como presión de diseño hasta que ECC tenga
  un adapter directo.
- Mantén sana la ruta solo terminal; es el piso de portabilidad.
