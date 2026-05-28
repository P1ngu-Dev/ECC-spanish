# Borrador de anuncio de release de ECC 1.10.1

ECC 1.10.1 es el release de estabilización de seguimiento a 1.10.0.

Este release se centra en la corrección de instalación, claridad en naming entre
superficies, recuperación en Windows/PowerShell, corrección de instalación de
Cursor en proyecto y compatibilidad de hooks de Claude Code. No es un release
cargado de features.

## Qué llegó en el pase de estabilización
- las superficies de npm/package/release están alineadas y `ecc-universal@1.10.0` está en vivo en npm
- corregidas las regresiones de locale/ruta de Windows y de la ruta de instalación de PowerShell
- corregida la regresión de storm de procesos del hook Bash
- corregida la compatibilidad del esquema de hook de Claude Code 2.1.x
- reparada la ruta nativa de instalación de proyecto en Cursor:
  - `.cursor/hooks.json` ahora incluye la superficie de schema/version requerida
  - `.cursor/mcp.json` se escribe en la ubicación nativa de proyecto de Cursor
- `continuous-learning-v2` ahora acepta `claude-desktop` como entrypoint válido
- la ruta de observe en Windows ahora omite `AppInstallerPythonRedirector.exe`
- los docs ahora distinguen con más claridad entre instalaciones de plugin e instalaciones manuales completas

## Para qué sirve 1.10.1
- hacer predecibles las superficies de instalación actuales
- reducir guías obsoletas de naming/instalación
- cerrar las regresiones de seguimiento de 1.10.0
- dar a los usuarios un punto de actualización estable en lugar de juntar fixes entre issues y discusiones

## Fixes incluidos en el release
- `#1543` reparación de la instalación nativa de hook + MCP de proyecto en Cursor
- `#1524` mitigación de argv duplicado en `settings.local.json` para Claude Code v2.1.116
- `#1522` `continuous-learning-v2` acepta `claude-desktop` como entrypoint válido
- `#1511` la ruta observe de Windows omite `AppInstallerPythonRedirector.exe`
- `#1546` corrección de quick start del plugin `continuous-learning-v2`
- `#1535` seguimiento de hero overflow

## Aclaración importante de naming
- identificador del marketplace/plugin de Claude: `everything-claude-code@everything-claude-code`
- paquete npm: `ecc-universal`
- repo de GitHub: `affaan-m/everything-claude-code`

Esas superficies son intencionalmente diferentes. El identificador del plugin sigue las reglas del marketplace de Anthropic; el paquete npm sigue siendo `ecc-universal`.

## Seguimiento activo
Esto debe anunciarse como un release de estabilización, no como “todos los edge cases están resueltos”.

Seguimos vigilando:
- edge cases específicos de OS en macOS, Windows y Linux
- diferencias de comportamiento específicas de shell
- desajustes de ruta de instalación entre Cursor y el plugin de Claude que solo aparecen en instalaciones antiguas o mixtas
- reportes de compatibilidad de provider/herramienta de terceros que todavía necesitan repro en current-main

Ejemplos actuales de la watch-list:
- `#1520` probablemente obsoleto salvo que el repro vuelva en el instalador actual
- `#1516` no bloquea salvo que se reproduzca en `main` actual
- `#1484` sigue siendo un issue paraguas/watch-list de Windows y no un gate activo de release

## Guía de actualización recomendada
Si encuentras problemas de instalación/runtime de 1.10.0:
1. actualiza a la superficie más reciente de paquete/plugin
2. evita mezclar instalación de plugin más copia manual completa del repo salvo que los docs lo indiquen explícitamente
3. si los problemas persisten, reporta:
   - OS + shell
   - versión de Claude Code/Cursor
   - método de instalación usado
   - stderr/output exacto
   - si el problema es instalación de plugin, instalación npm, sync del repo o instalación de proyecto de Cursor
