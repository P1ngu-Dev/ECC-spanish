# Guía de optimización de tokens

Configuraciones y hábitos prácticos para reducir el consumo de tokens, extender la calidad de la sesión y hacer más trabajo dentro de los límites diarios.

> Ver también: `rules/common/performance.md` para la estrategia de selección de modelo, `skills/strategic-compact/` para sugerencias automáticas de compactación.

---

## Configuraciones recomendadas

Estos son valores predeterminados recomendados para la mayoría de los usuarios. Los usuarios avanzados pueden ajustar los valores según su carga de trabajo; por ejemplo, configurando `MAX_THINKING_TOKENS` más bajo para tareas simples o más alto para trabajo arquitectónico complejo.

Agrega esto a tu `~/.claude/settings.json`:

```json
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_CODE_SUBAGENT_MODEL": "haiku"
  }
}
```

### Qué hace cada configuración

| Configuración | Predeterminado | Recomendado | Efecto |
|---------|---------|-------------|--------|
| `model` | opus | **sonnet** | Sonnet maneja bien ~80% de las tareas de programación. Cambia a Opus con `/model opus` para razonamiento complejo. ~60% menos costo. |
| `MAX_THINKING_TOKENS` | 31,999 | **10,000** | El pensamiento extendido reserva hasta 31,999 tokens de salida por solicitud para razonamiento interno. Reducirlo recorta el costo oculto en ~70%. Configúralo en `0` para desactivarlo en tareas triviales. |
| `CLAUDE_CODE_SUBAGENT_MODEL` | _(hereda del principal)_ | **haiku** | Los subagentes (Task tool) ejecutan en este modelo. Haiku es ~80% más barato y suficiente para exploración, lectura de archivos y ejecución de pruebas. |
| `ECC_CONTEXT_MONITOR_COST_WARNINGS` | on | **off para usuarios de suscripción** | Suprime las advertencias de costo estimado por API dirigidas al agente, manteniendo las advertencias de agotamiento de contexto, alcance y bucles. |

### Nota de la comunidad sobre los reemplazos de auto-compactación

Algunas compilaciones recientes de Claude Code tienen reportes de la comunidad de que `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` solo puede reducir el umbral de compactación, lo que significa que valores por debajo del predeterminado podrían compactar antes en lugar de después. Si eso ocurre en tu entorno, elimina el reemplazo y confía en `/compact` manual más la guía de `strategic-compact` de ECC. Ver [Solución de problemas](./TROUBLESHOOTING.md).

### Activar o desactivar el pensamiento extendido

- **Alt+T** (Windows/Linux) o **Option+T** (macOS) — alternar activado/desactivado
- **Ctrl+O** — ver la salida de pensamiento (modo detallado)

---

## Selección de modelo

Usa el modelo correcto para la tarea:

| Modelo | Ideal para | Costo |
|-------|----------|------|
| **Haiku** | Exploración de subagentes, lectura de archivos, búsquedas simples | Más bajo |
| **Sonnet** | Programación diaria, revisiones, escritura de pruebas, implementación | Medio |
| **Opus** | Arquitectura compleja, razonamiento de varios pasos, depuración de problemas sutiles | Más alto |

Cambia de modelo durante la sesión:

```
/model sonnet     # predeterminado para la mayoría del trabajo
/model opus       # razonamiento complejo
/model haiku      # búsquedas rápidas
```

---

## Gestión del contexto

### Comandos

| Comando | Cuándo usarlo |
|---------|-------------|
| `/clear` | Entre tareas no relacionadas. El contexto obsoleto desperdicia tokens en cada mensaje posterior. |
| `/compact` | En puntos lógicos de separación de tareas (después de planificar, después de depurar, antes de cambiar de foco). |
| `/cost` | Revisar el gasto de tokens de la sesión actual. |

### Advertencias de estimación de costo por API

El monitor de contexto de ECC puede emitir estimaciones de costo por API a partir de telemetría local de hooks. Si estás en una suscripción de Claude y esas estimaciones no reflejan tu factura real, desactiva solo las advertencias de costo dirigidas al agente:

```bash
export ECC_CONTEXT_MONITOR_COST_WARNINGS=off
```

Windows PowerShell:

```powershell
[Environment]::SetEnvironmentVariable('ECC_CONTEXT_MONITOR_COST_WARNINGS', 'off', 'User')
```

Esto no desactiva las advertencias de agotamiento de contexto, las advertencias de alcance, las advertencias de bucles, `/cost` ni los archivos de telemetría de costo.

### Compactación estratégica

La skill `strategic-compact` (en `skills/strategic-compact/`) sugiere `/compact` en intervalos lógicos en lugar de depender de la auto-compactación, que puede activarse a mitad de una tarea. Consulta el README de la skill para las instrucciones de configuración del hook.

**Cuándo compactar:**
- Después de explorar, antes de implementar
- Después de completar un hito
- Después de depurar, antes de continuar con trabajo nuevo
- Antes de un cambio importante de contexto

**Cuándo NO compactar:**
- A mitad de una implementación de cambios relacionados
- Mientras depuras un problema activo
- Durante una refactorización de varios archivos

### Los subagentes protegen tu contexto

Usa subagentes (Task tool) para exploración en lugar de leer muchos archivos en tu sesión principal. El subagente lee 20 archivos pero solo devuelve un resumen; tu contexto principal se mantiene limpio.

---

## Gestión del servidor MCP

Cada servidor MCP habilitado agrega definiciones de herramientas a tu ventana de contexto. El README advierte: **mantén menos de 10 habilitados por proyecto**.

Consejos:
- Ejecuta `/mcp` para ver los servidores activos y su costo de contexto
- Usa `/mcp` para deshabilitar servidores MCP de Claude Code cuando quieras un cambio en tiempo de ejecución. Claude Code conserva esas deshabilitaciones de tiempo de ejecución en `~/.claude.json`.
- Prefiere herramientas CLI cuando estén disponibles (`gh` en lugar de GitHub MCP, `aws` en lugar de AWS MCP)
- No dependas de `.claude/settings.json` ni `.claude/settings.local.json` para deshabilitar servidores MCP de Claude Code ya cargados; usa `/mcp` para eso.
- `ECC_DISABLED_MCPS` solo afecta la salida de configuración MCP generada por ECC durante flujos de instalación/sincronización, como `install.sh`, `npx ecc-install` y la combinación de MCP de Codex. No es un interruptor en vivo de Claude Code.
- El servidor MCP `memory` está configurado por defecto pero no es usado por ninguna skill, agente o hook; considera deshabilitarlo

---

## Advertencia de costo para Agent Teams

[Agent Teams](https://code.claude.com/docs/en/agent-teams) (experimental) crea múltiples ventanas de contexto independientes. Cada compañero consume tokens por separado.

- Úsalo solo para tareas donde el paralelismo aporte un valor claro (trabajo en varios módulos, revisiones paralelas)
- Para tareas secuenciales simples, los subagentes (Task tool) son más eficientes en tokens
- Actívalo con: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` en settings

---

## Futuro: integración de configure-ecc

El asistente de instalación `configure-ecc` podría ofrecer configurar estas variables de entorno durante la instalación, con explicaciones sobre las compensaciones de costo. Esto ayudaría a los usuarios nuevos a optimizar desde el primer día en lugar de descubrir estas configuraciones después de alcanzar límites.

---

## Referencia rápida

```bash
# Flujo de trabajo diario
/model sonnet              # Empieza aquí
/model opus                # Solo para razonamiento complejo
/clear                     # Entre tareas no relacionadas
/compact                   # En puntos lógicos de pausa
/cost                      # Revisar gasto

# Variables de entorno (agregar al bloque "env" en ~/.claude/settings.json)
MAX_THINKING_TOKENS=10000
CLAUDE_CODE_SUBAGENT_MODEL=haiku
CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```
