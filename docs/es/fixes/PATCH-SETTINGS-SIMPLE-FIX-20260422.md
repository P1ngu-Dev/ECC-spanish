# workaround del bug de argv duplicado en patch_settings_cl_v2_simple.ps1 (2026-04-22)

## Resumen

`docs/fixes/patch_settings_cl_v2_simple.ps1` es el helper PowerShell mínimo que
parchea `~/.claude/settings.local.json` para que el hook observer apunte a
`observe-wrapper.sh`. Es la contraparte “simple” de
`docs/fixes/install_hook_wrapper.ps1` (PR #1540): nunca copia el script wrapper,
solo reescribe el archivo de settings.

La versión anterior de este helper registraba la ruta cruda de `observe.sh`
como comando del hook, compartía una sola cadena de comando entre `PreToolUse`
y `PostToolUse`, y dependía de defaults de `ConvertTo-Json` que pueden emitir
finales de línea CRLF. Bajo Claude Code v2.1.116 el primer token argv se duplica,
así que el wrapper necesita invocarse con una forma específica y las dos fases
del hook necesitan entradas distintas.

## Qué hace el fix

- El primer token es `bash` resuelto por PATH (sin ruta `.exe` entre comillas),
  así que el bug de argv duplicado ya no pasa un binario como script. Coincide
  con PR #1524 y PR #1540.
- La ruta del wrapper se normaliza a forward slashes antes de incrustarse en el
  comando del hook, evitando sorpresas del manejo de backslashes de MSYS.
- `PreToolUse` y `PostToolUse` reciben comandos distintos con argumentos
  posicionales explícitos `pre` / `post`.
- El archivo de settings se escribe en UTF-8 (sin BOM) con CRLF normalizado a
  LF para que los parsers JSON posteriores nunca vean finales de línea mezclados.
- Los hooks existentes (incluidas entradas legacy de `observe.sh` y hooks de
  terceros no relacionados) se preservan — el script solo añade las nuevas
  entradas del wrapper cuando todavía no están registradas.
- Idempotente en reejecuciones: una segunda invocación reconoce las cadenas de
  comando canónicas y registra `[SKIP]` en lugar de duplicar entradas.

## Forma del comando resultante

```
bash "C:/Users/<you>/.claude/skills/continuous-learning/hooks/observe-wrapper.sh" pre
bash "C:/Users/<you>/.claude/skills/continuous-learning/hooks/observe-wrapper.sh" post
```

## Uso

```powershell
pwsh -File docs/fixes/patch_settings_cl_v2_simple.ps1
# Windows PowerShell 5.1 también es compatible:
powershell -NoProfile -ExecutionPolicy Bypass -File docs/fixes/patch_settings_cl_v2_simple.ps1
```

El script hace una copia de seguridad del archivo de settings existente en
`settings.local.json.bak-<timestamp>` antes de escribir.

## Compatibilidad con PowerShell 5.1

`ConvertFrom-Json -AsHashtable` solo existe en PowerShell 7+. El script prueba
primero `-AsHashtable` y cae de vuelta a una conversión manual de `PSCustomObject`
→ `Hashtable` en Windows PowerShell 5.1. Ambos buckets de hook (`PreToolUse`,
`PostToolUse`) y sus arrays `hooks` internos se materializan como
`System.Collections.ArrayList` antes de serializar, de modo que `ConvertTo-Json`
de PS 5.1 no puede colapsar arrays de un solo elemento en objetos sueltos.

## Casos verificados (dry-run)

1. Instalación limpia — sin settings existentes → crea el archivo canónico.
2. Reejecución idempotente — archivo canónico existente → `[SKIP]` en ambas fases,
   el contenido del archivo no cambia aparte del backup previo a la escritura.
3. Presencia de `observe.sh` legacy → preserva las entradas legacy y añade las
   nuevas entradas de `observe-wrapper.sh` junto a ellas.

Los tres casos producen salida solo LF y coinciden con la forma registrada por
el fix manual de PR #1524 en `settings.local.json`.

## Relacionado

- PR #1524 — fix de forma de `settings.local.json` (misma raíz del bug de argv duplicado)
- PR #1539 — `detect-project.sh` independiente de locale
- PR #1540 — fix de argv duplicado de `install_hook_wrapper.ps1` (script compañero)
