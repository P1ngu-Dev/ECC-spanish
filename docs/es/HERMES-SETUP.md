# Configuración de Hermes x ECC

Hermes es el shell del operador. ECC es el sistema reutilizable detrás de él.

Esta guía es la versión pública y sanitizada del stack de Hermes usado para ejecutar contenido, alcance, investigación, operaciones de ventas, revisiones financieras y flujos de trabajo de ingeniería desde una sola superficie nativa de terminal.

## Qué se publica de forma pública

- skills, agents, commands, hooks y configuraciones MCP de ECC desde este repo
- skills de flujo de trabajo generadas por Hermes que son lo bastante estables para reutilizarse
- una topología de operador documentada para chat, crons, memoria de workspace y flujos de distribución
- material de lanzamiento para compartir el stack públicamente

Esta guía no incluye secretos privados, tokens activos, datos personales ni una exportación cruda de `~/.hermes`.

## Arquitectura

Usa Hermes como la puerta de entrada y ECC como el sustrato reutilizable del flujo de trabajo.

```text
Telegram / CLI / TUI
        ↓
      Hermes
        ↓
 ECC skills + hooks + MCPs + generated workflow packs
        ↓
 Google Drive / GitHub / browser automation / research APIs / media tools / finance tools
```

## Mapa del Workspace Público

Usa esto como la superficie mínima para reproducir la configuración sin filtrar el estado privado.

- `~/.hermes/config.yaml`
  - enrutamiento de modelos
  - registro de servidores MCP
  - carga de plugins
- `~/.hermes/skills/ecc-imports/`
  - skills de ECC copiadas para uso nativo de Hermes
- `skills/hermes-generated/`
  - patrones de operador destilados de sesiones repetidas de Hermes
- `~/.hermes/plugins/`
  - plugins puente para hooks, recordatorios y pegamento de herramientas específico del flujo de trabajo
- `~/.hermes/cron/jobs.json`
  - ejecuciones programadas de automatización con prompts y canales explícitos
- `~/.hermes/workspace/`
  - artefactos de negocio, operaciones, salud, contenido y memoria

## Stack de Capacidades Recomendado

### Núcleo

- Hermes para chat, cron, orquestación y estado del workspace
- ECC para skills, rules, prompts y convenciones entre harnesses
- GitHub + Context7 + Exa + Firecrawl + Playwright como capa MCP base

### Contenido

- FFmpeg para edición y ensamblaje local
- Remotion para clips programables
- fal.ai para generación de imagen/video
- ElevenLabs para voz, limpieza y empaquetado de audio
- CapCut o VectCutAPI para el pulido final nativo de redes sociales

### Operaciones de Negocio

- Google Drive como sistema de registro para docs, sheets, decks y volcados de investigación
- Stripe para ingresos y operaciones de pago
- GitHub para ejecución de ingeniería
- Telegram y canales estilo iMessage para avisos urgentes y aprobaciones

## Qué sigue requiriendo autenticación local

Esto permanece local y debe configurarse por operador:

- token OAuth de Google para Drive / Docs / Sheets / Slides
- credenciales de distribución saliente / X / LinkedIn
- claves de Stripe
- credenciales de automatización de navegador y ajustes de stealth/proxy
- cualquier credencial de CRM o sistema de proyectos como Linear o Apollo
- ruta de exportación o ingesta de Apple Health si las automatizaciones de salud están habilitadas

## Orden de puesta en marcha sugerido

0. Ejecuta `ecc migrate audit --source ~/.hermes` primero para inventariar el workspace heredado y ver qué partes ya se mapean a ECC2.
0.5. Planea y genera artefactos de scaffolding antes de importar cualquier cosa:
   - genera planes revisables con `ecc migrate plan` y `ecc migrate scaffold`
   - genera scaffolding de skills heredadas reutilizables con `ecc migrate import-skills --output-dir migration-artifacts/skills`
   - genera plantillas de traducción de herramientas con `ecc migrate import-tools --output-dir migration-artifacts/tools`
   - genera plantillas de plugins puente con `ecc migrate import-plugins --output-dir migration-artifacts/plugins`
   - previsualiza trabajos recurrentes con `ecc migrate import-schedules --dry-run`
   - previsualiza despacho de gateway con `ecc migrate import-remote --dry-run`
   - previsualiza contexto seguro de env/service con `ecc migrate import-env --dry-run`
   - importa memoria sanitizada del workspace con `ecc migrate import-memory`
1. Instala ECC y verifica la configuración base del harness con `node tests/run-all.js`; el resultado esperado es un resumen de pruebas sin fallos.
2. Instala Hermes y apúntalo a las skills importadas de ECC.
3. Registra los servidores MCP que realmente usas a diario.
4. Autentica Google Drive primero, luego GitHub, y después los canales de distribución.
5. Empieza con una superficie pequeña de cron: comprobación de disponibilidad, responsabilidad de contenido, triage de bandeja de entrada, monitor de ingresos.
6. Solo después agrega flujos personales más pesados como salud, graficado de relaciones o secuenciación saliente.

## Documentos relacionados

- [Guía de migración de Hermes/OpenClaw](../../docs/HERMES-OPENCLAW-MIGRATION.md)
- [Arquitectura cross-harness](../../docs/architecture/cross-harness.md)

## Por qué Hermes x ECC

Este stack es útil cuando quieres:

- un solo lugar nativo de terminal para ejecutar operaciones de negocio e ingeniería
- skills reutilizables en lugar de prompts de una sola vez
- automatización que pueda avisar, auditar y escalar
- un repo público que muestre la forma del sistema sin exponer tu estado privado de operador

## Alcance del candidato a release público

ECC v2.0.0-rc.1 documenta la superficie de Hermes y publica el material de lanzamiento ahora.

Las piezas privadas restantes pueden añadirse más adelante:

- plantillas sanitizadas adicionales
- ejemplos públicos más ricos
- más paquetes de flujo de trabajo generados
- integraciones más estrechas con CRM y Google Workspace
