# workaround del bug de argv duplicado en install_hook_wrapper.ps1 (2026-04-22)

## Resumen

`docs/fixes/install_hook_wrapper.ps1` es el helper PowerShell que copia
`observe-wrapper.sh` a `~/.claude/skills/continuous-learning/hooks/` y reescribe
`~/.claude/settings.local.json` para que el hook observer apunte allí.

La versión anterior producía un comando de hook de la forma:

```
"C:\Program Files\Git\bin\bash.exe" "C:\Users\...\observe-wrapper.sh"
```

Bajo Claude Code v2.1.116 el primer token argv se duplica. Cuando ese token es
una ruta entre comillas a un ejecutable de Windows, `bash.exe` se vuelve a
invocar a sí mismo como `$0`, lo que falla con `cannot execute binary file`
(exit 126). PR #1524 documenta la causa raíz; este script es un compañero que
mantie(ne) el instalador alineado con la forma corregida de `settings.local.json`.

## Qué hace el fix

- El primer token ahora es `bash` resuelto por PATH (sin ruta `.exe` entre comillas),
  así que el bug de argv duplicado ya no pasa un binario como script.
- La ruta del wrapper se normaliza a forward slashes antes de incrustarse en el
  comando del hook, evitando sorpresas del manejo de backslashes de MSYS.
- `PreToolUse` y `PostToolUse` reciben comandos distintos con argumentos
  posicionales explícitos `pre` / `post`, coincidiendo con la forma que espera el wrapper.
- El archivo de settings se escribe con finales de línea LF para que los parsers
  JSON posteriores nunca vean salida CRLF/LF mezclada de `ConvertTo-Json`.

## Forma del comando resultante

```
bash "C:/Users/<you>/.claude/skills/continuous-learning/hooks/observe-wrapper.sh" pre
bash "C:/Users/<you>/.claude/skills/continuous-learning/hooks/observe-wrapper.sh" post
```

## Uso

```powershell
# Coloca observe-wrapper.sh junto a este script, luego:
pwsh -File docs/fixes/install_hook_wrapper.ps1
```

El script hace una copia de seguridad de `settings.local.json` a
`settings.local.json.bak-<timestamp>` antes de escribir.

## Compatibilidad con PowerShell 5.1

`ConvertFrom-Json -AsHashtable` solo existe en PowerShell 7+. El script prueba
primero `-AsHashtable` y cae de vuelta a una conversión manual de `PSCustomObject`
→ `Hashtable` en Windows PowerShell 5.1. Ambos buckets de hook (`PreToolUse`,
`PostToolUse`) y sus arrays `hooks` internos se materializan como
`System.Collections.ArrayList` antes de serializar, de modo que `ConvertTo-Json`
de PS 5.1 no puede colapsar arrays de un solo elemento en objetos sueltos.
Verificado ejecutando `powershell -NoProfile -File
docs/fixes/install_hook_wrapper.ps1` en una máquina Windows 11 con solo Windows
PowerShell 5.1 instalado (sin `pwsh`).

## Relacionado

- PR #1524 — fix de forma de `settings.local.json` (misma raíz del bug de argv duplicado)
- PR #1511 — omitir `AppInstallerPythonRedirector.exe` en la resolución de python del observer
- PR #1539 — `detect-project.sh` independiente de locale
- PR #1542 — fix compañero de `patch_settings_cl_v2_simple.ps1`
