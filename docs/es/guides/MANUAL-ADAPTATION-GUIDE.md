# Guía de adaptación manual para harnesses no nativos

Usa esta guía cuando quieras el comportamiento de ECC dentro de un harness que no carga de forma nativa las estructuras `.claude/`, `.codex/`, `.opencode/`, `.cursor/` o `.agent/`.

Este es el camino alternativo para herramientas como Grok y otras interfaces tipo chat que pueden aceptar prompts del sistema, archivos cargados o instrucciones pegadas, pero no pueden ejecutar directamente las superficies de instalación nativas del repositorio.

## Cuándo usar esto

Usa la adaptación manual cuando el harness de destino:

- no carga automáticamente carpetas del repositorio
- no admite comandos slash personalizados
- no admite hooks
- no admite activación de skills locales del repositorio
- tiene acceso parcial o nulo al sistema de archivos o a las herramientas

Prefiere un destino ECC de primera clase siempre que exista uno:

- Claude Code
- Codex
- Cursor
- OpenCode
- CodeBuddy
- Antigravity

Usa esta guía solo cuando necesites comportamiento ECC en un harness no nativo.

## Lo que estás reproduciendo

Cuando adaptas ECC manualmente, intentas preservar cuatro cosas:

1. Contexto enfocado en lugar de volcar todo el repositorio.
2. Señales de activación de skills en lugar de esperar que el modelo adivine el flujo.
3. Intención de los comandos incluso cuando el harness no tiene sistema de comandos slash.
4. Disciplina de hooks incluso cuando el harness no tiene automatización nativa.

No estás intentando reflejar cada archivo del repositorio. Estás intentando recrear el comportamiento útil con el paquete de contexto más pequeño posible.

## La alternativa nativa de ECC

Predetermina la selección manual desde el propio repositorio.

Empieza solo con los archivos que realmente necesites:

- un skill de lenguaje o framework
- un skill de flujo de trabajo
- un skill de dominio si la tarea es especializada
- un agente o comando solo si el harness se beneficia de una orquestación explícita

Buenos ejemplos mínimos:

- Trabajo de una feature en Python:
  - `skills/python-patterns/SKILL.md`
  - `skills/tdd-workflow/SKILL.md`
  - `skills/verification-loop/SKILL.md`
- Trabajo de API en TypeScript:
  - `skills/backend-patterns/SKILL.md`
  - `skills/security-review/SKILL.md`
  - `skills/tdd-workflow/SKILL.md`
- Trabajo de contenido/salida:
  - `skills/brand-voice/SKILL.md`
  - `skills/content-engine/SKILL.md`
  - `skills/crosspost/SKILL.md`

Si el harness admite carga de archivos, carga solo esos archivos.

Si el harness solo admite contexto pegado, extrae las secciones relevantes y pega un paquete comprimido en lugar de los archivos completos en bruto.

## Empaquetado manual de contexto

No necesitas herramientas adicionales para hacer esto.

Usa el repositorio directamente:

```bash
cd /path/to/everything-claude-code

sed -n '1,220p' skills/tdd-workflow/SKILL.md > /tmp/ecc-context.md
printf '\n\n---\n\n' >> /tmp/ecc-context.md
sed -n '1,220p' skills/backend-patterns/SKILL.md >> /tmp/ecc-context.md
printf '\n\n---\n\n' >> /tmp/ecc-context.md
sed -n '1,220p' skills/security-review/SKILL.md >> /tmp/ecc-context.md
```

También puedes usar `rg` para identificar los skills correctos antes de empaquetar:

```bash
rg -n "When to use|Use when|Trigger" skills -g 'SKILL.md'
```

Opcional: si ya usas un empaquetador de repos como `repomix`, puede ayudar a comprimir archivos seleccionados en un solo documento de entrega. Es una herramienta de conveniencia, no la ruta canónica de ECC.

## Reglas de compresión

Cuando empaquetes ECC manualmente para otro harness:

- conserva el planteamiento de la tarea
- conserva las condiciones de activación
- conserva los pasos del flujo de trabajo
- conserva los ejemplos críticos
- elimina primero la prosa repetitiva
- elimina después las variantes no relacionadas
- evita pegar directorios completos cuando basten uno o dos skills

Si necesitas un formato de prompt más ajustado, convierte las partes esenciales en un bloque estructurado compacto:

```xml
<skill name="tdd-workflow">
  <when>Nueva feature, corrección de bug o refactor que deba hacerse con pruebas primero.</when>
  <steps>
    <step>Escribe una prueba que falle.</step>
    <step>Haz que pase con el cambio más pequeño.</step>
    <step>Refactoriza y vuelve a ejecutar la validación.</step>
  </steps>
</skill>
```

## Reproducción de comandos

Si el harness no tiene sistema de comandos slash, define un pequeño registro de comandos en el prompt del sistema o en el preámbulo de la sesión.

Ejemplo:

```text
Registro de comandos:
- /plan -> usa razonamiento estilo planner, produce un plan breve de ejecución y luego actúa
- /tdd -> sigue el skill tdd-workflow
- /review -> cambia a modo code-review y enumera primero los hallazgos
- /verify -> ejecuta un loop de verificación antes de declarar completado
```

No estás implementando comandos reales. Estás proporcionando al harness manejadores de invocación explícitos que se asignan al comportamiento de ECC.

## Reproducción de hooks

Si el harness no tiene hooks nativos, mueve la intención del hook a las instrucciones permanentes.

Ejemplo:

```text
Antes de escribir código:
1. Comprueba si debe activarse un skill relevante.
2. Comprueba si hay cambios sensibles para la seguridad.
3. Prioriza pruebas antes de la implementación cuando sea posible.

Antes de finalizar:
1. Relee la solicitud del usuario.
2. Verifica las rutas principales modificadas.
3. Indica qué se validó realmente y qué no.
```

Eso no recrea la automatización real, pero sí captura la disciplina operativa de ECC.

## Matriz de capacidades del harness

| Capacidad | Destinos ECC de primera clase | Destinos de adaptación manual |
| --- | --- | --- |
| Instalación basada en carpetas | Nativa | No |
| Comandos slash | Nativos | Simulados en el prompt |
| Hooks | Nativos | Simulados en el prompt |
| Activación de skills | Nativa | Manual |
| Herramientas locales del repositorio | Nativas | Depende del harness |
| Empaquetado de contexto | Opcional | Requerido |

## Configuración práctica estilo Grok

1. Elige el paquete útil más pequeño.
2. Empaqueta los archivos de skills de ECC seleccionados en una sola carga o bloque pegado.
3. Añade un registro breve de comandos.
4. Añade instrucciones permanentes de “intención de hook”.
5. Empieza con una tarea y verifica que el harness siga el flujo antes de escalar.

Preambulo inicial de ejemplo:

```text
Estás operando con un paquete ECC adaptado manualmente.

Skills activos:
- backend-patterns
- tdd-workflow
- security-review

Registro de comandos:
- /plan
- /tdd
- /verify

Antes de escribir código, sigue las instrucciones de los skills activos.
Antes de finalizar, verifica qué cambió e informa cualquier brecha restante.
```

## Limitaciones

La adaptación manual es útil, pero sigue siendo de segunda clase en comparación con los destinos nativos.

Pierdes:

- instalación y sincronización automáticas
- ejecución nativa de hooks
- verdadero enrutamiento de comandos
- descubrimiento fiable de skills en tiempo de ejecución
- orquestación integrada de múltiples agentes o worktrees

Así que la regla es simple:

- usa la adaptación manual para llevar el comportamiento de ECC a harnesses no nativos
- usa destinos ECC nativos siempre que quieras el sistema completo

## Trabajo relacionado

- [Issue #1186](https://github.com/affaan-m/everything-claude-code/issues/1186)
- [Discussion #1077](https://github.com/affaan-m/everything-claude-code/discussions/1077)
- [Antigravity Guide](./ANTIGRAVITY-GUIDE.md)
- [Troubleshooting](../reference/TROUBLESHOOTING.md)
