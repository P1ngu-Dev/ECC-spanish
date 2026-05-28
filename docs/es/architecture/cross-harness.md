# Arquitectura cross-harness

ECC es la capa reusable de workflow. Los harnesses son superficies de ejecución.

El objetivo es mantener las partes duraderas del trabajo agentic en un solo repo:

- skills
- rules e instrucciones
- hooks donde el harness los soporte
- configuración MCP
- manifiestos de instalación
- patrones de sesión y orquestación

Claude Code, Codex, OpenCode, Cursor, Gemini y futuros harnesses deberían adaptar esos assets en el borde en lugar de requerir un nuevo modelo de workflow para cada herramienta.

Para la matriz de soporte y el flujo de trabajo de tarjeta de puntuación orientados al operador, consulta
[Matriz de cumplimiento de adaptadores de harness](harness-adapter-compliance.md).

## Modelo de portabilidad

| Surface | Shared Source | Harness Adapter | Current Status |
|---------|---------------|-----------------|----------------|
| Skills | `skills/*/SKILL.md` | Claude plugin, Codex plugin, `.agents/skills`, copias de skills de Cursor, plugin/config de OpenCode | Soportado con empaquetado específico del harness |
| Rules and instructions | `rules/`, `AGENTS.md`, docs traducidos | Instalación de rules de Claude, `AGENTS.md` de Codex, rules de Cursor, instrucciones de OpenCode | Soportado, pero no idéntico entre harnesses |
| Hooks | `hooks/hooks.json`, `scripts/hooks/` | Hooks nativos de Claude, eventos de plugin de OpenCode, adapter de hooks de Cursor | Basado en hooks en Claude/OpenCode/Cursor; basado en instrucciones en Codex |
| MCPs | `.mcp.json`, `mcp-configs/` | Importación nativa de config MCP por harness | Soportado donde el harness expone MCP |
| Commands | `commands/`, scripts CLI | Slash commands de Claude, shims de compatibilidad, entrypoints CLI | Soportado, pero la semántica de comandos varía |
| Sessions | `ecc2/`, adapters de sesión, scripts de orquestación | TUI/daemon, orquestación tmux/worktree, runners específicos del harness | Alpha |

## Qué viaja sin cambios

`SKILL.md` es la unidad más portable.

Un buen skill de ECC debería:

- usar frontmatter YAML con `name`, `description` y `origin`
- describir cuándo usar el skill
- declarar tools o conectores requeridos sin incrustar secretos
- mantener los ejemplos relativos al repo o genéricos
- evitar suposiciones de comandos solo de harness salvo que la sección esté claramente etiquetada

El mismo skill fuente puede instalarse en múltiples harnesses porque es
principalmente instrucciones, restricciones y forma de workflow.

## Qué se adapta

Cada harness tiene un comportamiento distinto de carga y aplicación:

- Claude Code carga assets de plugin y tiene ejecución nativa de hooks.
- Codex lee `AGENTS.md`, metadatos de plugin, skills y config MCP, pero la
  paridad de hooks es impulsada por instrucciones.
- OpenCode tiene un sistema de plugins/eventos que puede reutilizar la lógica de
  hooks de ECC mediante una capa de adapter.
- Cursor usa su propio layout de rules y hooks, así que ECC mantiene superficies
  traducidas bajo `.cursor/`.
- El soporte de Gemini está orientado a instalación/instrucciones y debe tratarse
  como una superficie de compatibilidad, no como paridad completa de hooks.

Los adapters deben seguir siendo delgados. El comportamiento compartido vive en
`skills/`, `rules/`, `hooks/`, `scripts/` y `mcp-configs/`.

## Límite Hermes

Hermes no es el runtime público de ECC.

Hermes es una shell de operador que puede consumir assets de ECC:

- importar skills seleccionados de ECC a un directorio de skills de Hermes
- usar convenciones MCP de ECC para acceso a herramientas
- enrutar workflows de chat, CLI, cron y handoff a través de patrones reutilizables de ECC
- destilar el trabajo local repetido del operador de vuelta a skills saneados de ECC

El repo público debe publicar patrones reutilizables, no estado local de Hermes.

Sí publica:

- docs de setup saneados
- prompts demo relativos al repo
- skills generales de operador
- ejemplos que no dependan de credenciales privadas

No publiques:

- tokens OAuth o API keys
- exports brutos de `~/.hermes`
- memoria personal del workspace
- datasets privados
- packs de automatización solo local que no hayan sido revisados

## Ejemplo trabajado

Usa `skills/hermes-imports/SKILL.md` como la misma fuente de skill en todos los harnesses.

El workflow es:

1. Autoriza el comportamiento duradero una sola vez en `skills/hermes-imports/SKILL.md`.
2. Mantén secretos, rutas locales y memoria cruda del operador fuera del skill.
3. Deja que cada harness adapte cómo se carga el skill.
4. Prueba la fuente del skill y los metadatos orientados al harness por separado.

Claude Code obtiene el skill a través de la superficie del plugin de Claude y
puede hacer cumplir hooks relacionados de forma nativa.

Codex lee las instrucciones del repo, `.codex-plugin/plugin.json` y la config MCP
de referencia. La misma fuente del skill sigue describiendo el workflow, pero la
paridad de hooks depende de instrucciones salvo que Codex añada una superficie
nativa de hooks.

OpenCode obtiene el skill a través de la superficie de paquete/plugin de
OpenCode. El manejo de eventos puede reutilizar la lógica de hooks de ECC a
través de la capa de adapter, mientras el texto del skill permanece sin cambios.

Si un cambio exige editar tres copias del mismo workflow para harnesses
diferentes, la fuente compartida está en el lugar equivocado. Devuelve el
workflow a `skills/` y adapta solo la carga, la forma del evento o el ruteo de
comandos en el borde del harness.

## Hoy vs. después

Soportado hoy:

- fuente de skill compartida en `skills/`
- empaquetado de plugin de Claude Code
- metadatos de plugin de Codex y config MCP de referencia
- superficie de paquete/plugin de OpenCode
- rules, hooks y skills adaptados para Cursor
- `ecc2/` como un control plane Rust alpha

Aún madurando:

- paridad exacta de hooks en todos los harnesses
- sincronización automática de skills en Hermes
- empaquetado de release para `ecc2/`
- semántica de reanudación cross-harness de sesiones
- capas más profundas de memoria y planificación de operador

## Regla para trabajo nuevo

Cuando añadas un workflow, pon primero el comportamiento duradero en ECC.

Usa archivos específicos del harness solo para:

- cargar el asset compartido
- adaptar la forma del evento
- mapear nombres de comando
- manejar límites de plataforma

Si un workflow solo funciona en un harness, documenta ese límite directamente.
