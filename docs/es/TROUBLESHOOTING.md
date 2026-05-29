# Solución de problemas

Soluciones alternativas reportadas por la comunidad para errores actuales de Claude Code que pueden afectar a los usuarios de ECC.

Estos son comportamientos de Claude Code aguas arriba, no errores de ECC. Las entradas de abajo resumen las soluciones alternativas probadas en producción recopiladas en [issue #644](https://github.com/affaan-m/everything-claude-code/issues/644) en Claude Code `v2.1.79` (macOS, uso intensivo de hooks, conectores MCP habilitados). Trátalas como medidas provisionales pragmáticas hasta que lleguen las correcciones aguas arriba.

## Soluciones alternativas de la comunidad para errores abiertos de Claude Code

### Etiquetas falsas de "Hook Error" en hooks que por lo demás se ejecutan correctamente

**Síntomas:** El hook se ejecuta correctamente, pero Claude Code sigue mostrando `Hook Error` en la transcripción.

**Lo que ayuda:**

- Consume stdin al inicio del hook (`input=$(cat)` en hooks de shell) para que el proceso padre no vea una tubería sin consumir.
- Para hooks simples de permitir/bloquear, envía diagnósticos legibles para humanos a stderr y mantén stdout silencioso a menos que tu implementación del hook requiera explícitamente stdout estructurado.
- Redirige stderr ruidoso de procesos secundarios cuando no sea accionable.
- Usa los códigos de salida correctos: `0` permite, `2` bloquea, y otros códigos distintos de cero se tratan como errores.

**Ejemplo:**

```bash
# Good: block with stderr message and exit 2
input=$(cat)
echo "[BLOCKED] Reason here" >&2
exit 2
```

### Compactación antes de lo esperado con `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE`

**Síntomas:** Bajar `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` hace que la compactación ocurra antes, no después.

**Lo que ayuda:**

- En algunas versiones actuales de Claude Code, valores más bajos pueden reducir el umbral de compactación en lugar de ampliarlo.
- Si quieres más margen de trabajo, elimina `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` y prefiere `/compact` manual en límites lógicos de la tarea.
- Usa la guía de ECC `strategic-compact` en lugar de forzar un umbral más bajo de auto-compactación.

### Los conectores MCP parecen conectados, pero fallan después de la compactación

**Síntomas:** Las herramientas MCP de Gmail o Google Drive fallan después de la compactación aunque el conector siga pareciendo autenticado en la interfaz.

**Lo que ayuda:**

- Desactiva y vuelve a activar el conector afectado después de la compactación.
- Si tu versión de Claude Code lo admite, añade un hook de recordatorio `PostCompact` que te avise para volver a comprobar la autenticación del conector después de la compactación.
- Trátalo como un paso de recuperación del estado de autenticación, no como una solución permanente.

### Las ediciones de hooks no se recargan automáticamente

**Síntomas:** Los cambios en los hooks de `settings.json` no surten efecto hasta que se reinicia la sesión.

**Lo que ayuda:**

- Reinicia la sesión de Claude Code después de cambiar los hooks.
- Algunos usuarios avanzados a veces automatizan un comando local `/reload` alrededor de `kill -HUP $PPID`, pero ECC no lo incluye porque depende del shell y no es universalmente fiable.

### Respuestas repetidas `529 Overloaded`

**Síntomas:** Claude Code comienza a fallar bajo alta presión de hooks, herramientas o contexto.

**Lo que ayuda:**

- Reduce la presión de definiciones de herramientas con `ENABLE_TOOL_SEARCH=auto:5` si tu configuración lo admite.
- Reduce `MAX_THINKING_TOKENS` para trabajo rutinario.
- Dirige el trabajo de subagentes a un modelo más barato como `CLAUDE_CODE_SUBAGENT_MODEL=haiku` si tu configuración expone ese control.
- Desactiva los servidores MCP no usados por proyecto.
- Compacta manualmente en puntos naturales de pausa en lugar de esperar a la auto-compactación.

## Documentos relacionados de ECC

- [hook-bug-workarounds.md](../../docs/hook-bug-workarounds.md) para la lista de verificación más breve de recuperación de hooks, compactación y MCP.
- [hooks/README.md](../../hooks/README.md) para el ciclo de vida de hooks documentado en ECC y el comportamiento de los códigos de salida.
- [token-optimization.md](./token-optimization.md) para ajustes de costo y gestión de contexto.
- [issue #644](https://github.com/affaan-m/everything-claude-code/issues/644) para el reporte original y el entorno probado.
