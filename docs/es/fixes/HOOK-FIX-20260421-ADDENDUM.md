# Addendum de HOOK-FIX-20260421 — bug de argv duplicado v2.1.116

Ya quedó corregido en la sesión de la mañana como commit 527c18b. En la sesión
de la noche se hicieron verificaciones adicionales y se identificó un bug
específico de Claude Code que la fix de la mañana no cubría del todo, así que se
registra este apéndice.

## Forma del fix de la mañana

```json
"command": "C:/Users/sugig/.claude/skills/continuous-learning/hooks/observe-wrapper.sh pre"
```

Forma que usa el archivo `.sh` directamente como command. Se asume que Git Bash
lo ejecuta mediante shebang.

## Lo que reveló la verificación adicional de la noche

Si ejecutas directamente un archivo `.sh` con `child_process.spawn` de Node.js,
en Windows falla con **EFTYPE**:

```js
spawn('C:/Users/sugig/.claude/skills/continuous-learning/hooks/observe-wrapper.sh', 
      ['post'], {stdio:['pipe','pipe','pipe']});
// → Error: spawn EFTYPE (errno -4028)
```

Con `shell:true` puede ejecutarse vía `cmd.exe`, pero queda el riesgo de una
dependencia de implementación interna de Claude Code.

## Fix adicional aplicado por la noche

Se actualizó a una invocación explícita con `bash` como primer token (resuelto
por PATH):

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "*",
      "hooks": [{
        "type": "command",
        "command": "bash \"C:/Users/sugig/.claude/skills/continuous-learning/hooks/observe-wrapper.sh\" pre"
      }]
    }],
    "PostToolUse": [{
      "matcher": "*",
      "hooks": [{
        "type": "command",
        "command": "bash \"C:/Users/sugig/.claude/skills/continuous-learning/hooks/observe-wrapper.sh\" post"
      }]
    }]
  }
}
```

Esta forma es la misma que la registrada en `~/.claude/hooks/hooks.json` como
observer canónico de ECC, y hay evidencia real de que funciona sin errores.

### Verificación con Node spawn

```js
spawn('bash "C:/Users/sugig/.claude/skills/continuous-learning/hooks/observe-wrapper.sh" post',
      [], {shell:true});
// exit=0 → observaciones.jsonl se añade correctamente
```

## Bug de argv duplicado de Claude Code v2.1.116 (detalle)

En el doc del fix de la mañana se registró como “Defecto 2” el error
`bash.exe: bash.exe: cannot execute binary file`, pero ya se pudo identificar el
mecanismo raíz.

### Reproducción

```bash
"C:\Program Files\Git\bin\bash.exe" "C:\Program Files\Git\bin\bash.exe"
# stderr: "C:\Program Files\Git\bin\bash.exe: C:\Program Files\Git\bin\bash.exe: cannot execute binary file"
# exit: 126
```

bash interpreta `argv[1]` como script e intenta cargarlo. Si `argv[1]` es
`bash.exe` en sí mismo, falla en la detección ELF/PE → exit 126. El mensaje de
error coincide exactamente.

### Comportamiento de Claude Code

Cuando el hook command es `"C:\Program Files\Git\bin\bash.exe"
"C:\Users\...\wrapper.sh"`, v2.1.116 probablemente pasa el **primer token
(= ruta completa a bash.exe) tanto a `argv[0]` como a `argv[1]`**. Como
resultado bash intenta leer `argv[1] = bash.exe` como script y cae con 126.

### Workarounds

Evita que el primer token sea la ruta completa a bash.exe con espacios:
1. `OK:` `bash` (un único token resuelto por PATH) — patrón del fix de la noche / hooks.json
2. `OK:` ruta `.sh` directa (depende del manejo de `.sh` por Claude Code) — fix de la mañana
3. `BAD:` `"C:\Program Files\Git\bin\bash.exe" "<path>"` — primer token quoted con espacios

## Conclusión

El fix de la mañana (indicar `.sh` directamente) y el fix de la noche (prefijo
explícito `bash`) evitan el bug de argv duplicado, pero **el fix de la noche es
preferible** porque depende menos de la implementación de Claude Code.

Sin embargo, el commit de la fix de la mañana 527c18b ya está en docs/fixes/,
así que este Addendum deja constancia de ambas posturas. En el próximo restart
del CLI, el fix de la noche es el que debería quedarse en operación.

## Relacionado

- commit de fix de la mañana: 527c18b
- doc de fix de la mañana: docs/fixes/HOOK-FIX-20260421.md
- script de aplicación de la mañana: docs/fixes/apply-hook-fix.sh
- registro local del fix nocturno: C:\Users\sugig\Documents\Claude\Projects\ECC作成\hook-fix-report-20260421.md
- archivo aplicado en la noche: C:\Users\sugig\.claude\settings.local.json
- backup nocturno: C:\Users\sugig\.claude\settings.local.json.bak-hook-fix-20260421
