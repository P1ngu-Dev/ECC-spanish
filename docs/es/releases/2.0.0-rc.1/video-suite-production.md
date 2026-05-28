# Manifiesto de producción de la suite de video de ECC 2.0

Fecha de captura: 2026-05-19.

Este es el contrato de producción para la suite de video del lanzamiento de ECC 2.0. Mantiene en un solo lugar la historia pública del lanzamiento, el inventario local de fuentes, las salidas de render y el gate de autoevaluación, sin comprometer material bruto, exportaciones privadas de transcripciones ni rutas locales absolutas.

## Afirmación

ECC 2.0 es el sistema de operador nativo del harness para trabajo agéntico.

Los videos deberían probar esa afirmación directamente:

- una sola capa reutilizable para Claude Code, Codex, OpenCode, Cursor, Gemini, Zed, GitHub Copilot y flujos de terminal;
- skills, rules, hooks, agentes, convenciones MCP, gates de release y flujos de operador reutilizables;
- `ecc2/` como la dirección alfa del plano de control/TUI, no como todo el producto;
- AgentShield y los gates de supply-chain como la capa de confianza empresarial;
- OSS sigue siendo gratis, con GitHub Sponsors, ECC Tools Pro y consultoría como la superficie de financiación.

No enmarques el lanzamiento como un cambio de nombre, un pivot, un paquete de configuración o un paquete solo para Claude.

## Entradas privadas

No comprometas material bruto, JSON de transcripciones ni exportaciones de líneas de tiempo.

Los operadores deben apuntar el validador a medios locales usando variables de entorno:

```bash
ECC_VIDEO_SOURCE_ROOT=/path/to/ecc_2_raws \
ECC_VIDEO_RELEASE_SUITE_ROOT=/path/to/ecc_2_release_suite \
npm run release:video-suite -- --format json
```

`ECC_VIDEO_SOURCE_ROOT` debe contener imágenes de prueba y puede incluir un subdirectorio `_edited/` con clips fuente editados. `ECC_VIDEO_RELEASE_SUITE_ROOT` debe contener `edl/`, `segments/`, `renders/`, `timelines/` y `transcripts/`.

## Inventario de fuentes

Estos basenames son las entradas locales requeridas para el validador de la suite de lanzamiento.

| Activo | Línea | Prueba |
| --- | --- | --- |
| `longform-full-wide.mp4` | Video principal de lanzamiento | sistema de operador, dirección del plano de control, prueba de cierre |
| `sf-longform-full.mp4` | Video principal de lanzamiento | apertura de contexto estructurado |
| `sf-thread-2-whatisecc.mp4` | Qué es ECC | claridad de categoría y explicación de GitHub App |
| `sf-thread-4-security.mp4` | Prueba de seguridad | AgentShield, hooks, MCP, riesgo de permisos |
| `thread-2-ghapp-money.mp4` | Clip de dinero/prueba | OSS más hosting y servicios de pago |
| `architecture-2-wide.mp4` | B-roll | arquitectura nativa del harness |
| `terminal-scan-2-wide.mp4` | Prueba de instalación | flujo de terminal y confianza en la instalación |
| `new_site_raw.mp4` | B-roll | sitio y superficie del producto |
| `coverage-montage-wide.mp4` | Cobertura/prueba social | distribución y prueba social |
| `metrics-ticker-2-wide.mp4` | Clip de dinero/prueba | tracción y prueba del embudo |
| `growth-timeline-2-wide.mp4` | Cobertura/prueba social | línea temporal del impulso del lanzamiento |
| `gh_app_1.png` | Clip de dinero/prueba | superficie alojada de GitHub App |
| `star_history.png` | Cobertura/prueba social | gráfico de adopción OSS |
| `x_analytics.png` | Cobertura/prueba social | prueba de distribución social |
| `100k.png` | Cobertura/prueba social | prueba de hito de alcance |

## Entregables

| Entregable | Duración | Aspecto | Salida |
| --- | ---: | --- | --- |
| Video principal de lanzamiento | 90-150s | 16:9 | `ecc-2-primary-launch.mp4` |
| Clip de prueba de instalación | 25-35s | 16:9 y 9:16 | `ecc-2-install-proof-*` |
| Clip de qué es ECC | 45-60s | 16:9 y 9:16 | `ecc-2-what-is-ecc-*` |
| Clip de prueba de seguridad | 45-60s | 16:9 y 9:16 | `ecc-2-security-proof-*` |
| Clip de dinero/prueba | 30-45s | 16:9 y 9:16 | `ecc-2-money-proof-*` |
| Clip de cobertura/prueba social | 30-45s | 16:9 y 9:16 | `ecc-2-social-proof-*` |

## Video principal de lanzamiento

El ensamblaje bruto v1 del video principal es la columna vertebral actual. Debe seguir liderado por la voz, con prueba de producto cubriendo los cortes bruscos y el lenguaje anterior.

| Orden | Fuente | Entrada | Salida | Uso |
| --- | --- | ---: | ---: | --- |
| 01 | `sf-longform-full.mp4` | 161.12 | 177.68 | Apertura más limpia: ECC como contexto estructurado con skills, comandos, agentes, hooks y configuración de proyecto. |
| 02 | `thread-2-ghapp-money.mp4` | 21.84 | 30.40 | Tesis directa del producto: optimización del harness agéntico. |
| 03 | `thread-2-ghapp-money.mp4` | 41.00 | 59.72 | No otro harness; ECC es la capa y las herramientas encima de los harnesses. |
| 04 | `longform-full-wide.mp4` | 254.60 | 271.20 | IDE agéntico, observabilidad, tracing y dirección de plano de control multiagente. |
| 05 | `sf-thread-2-whatisecc.mp4` | 40.08 | 60.60 | GitHub App analiza repos e inyecta skills, prompts y hooks específicos del proyecto. |
| 06 | `sf-thread-4-security.mp4` | 17.60 | 32.72 | Configuración del riesgo de seguridad: hooks, servidores MCP, permisos. |
| 07 | `sf-thread-4-security.mp4` | 37.28 | 51.32 | Prueba de AgentShield: rules, categorías, evaluaciones, secretos, inyección, exfiltración. |
| 08 | `thread-2-ghapp-money.mp4` | 59.72 | 75.96 | Modelo de negocio OSS-first más superficie gestionada de GitHub App. |
| 09 | `longform-full-wide.mp4` | 507.34 | 525.62 | Cierre sobre workflows, shipping probado y trabajo diario seguro de agentes. |

Artefactos brutos v1 requeridos localmente:

- `edl/primary-launch.edl.md`
- `timelines/primary-launch-v1.timeline.json`
- `renders/ecc-2-primary-launch-rough-v1.mp4`
- `renders/ecc-2-primary-launch-rough-v1.captions.srt`
- `segments/primary-launch-v1/01-structured-context.mp4`
- `segments/primary-launch-v1/02-agentic-harness-optimization.mp4`
- `segments/primary-launch-v1/03-not-another-harness.mp4`
- `segments/primary-launch-v1/04-agentic-ide-surface.mp4`
- `segments/primary-launch-v1/05-github-app-proof.mp4`
- `segments/primary-launch-v1/06-security-risk.mp4`
- `segments/primary-launch-v1/07-agentshield-proof.mp4`
- `segments/primary-launch-v1/08-oss-paid-model.mp4`
- `segments/primary-launch-v1/09-close-shipping-system.mp4`

## Salidas candidatas para publicación

El validador del lanzamiento también espera el conjunto actual de candidatas en `renders/publish-candidates/`. Siguen siendo archivos locales de revisión, no cargas públicas ni medios comprometidos.

| Salida | Objetivo |
| --- | --- |
| `ecc-2-primary-launch.mp4` | 90-150s, 1920x1080, audio |
| `ecc-2-primary-launch.captions.srt` | subtítulos principales |
| `ecc-2-install-proof-wide.mp4` | 25-35s, 1920x1080, audio |
| `ecc-2-install-proof-vertical.mp4` | 25-35s, 1080x1920, audio |
| `ecc-2-what-is-ecc-wide.mp4` | 45-60s, 1920x1080, audio |
| `ecc-2-what-is-ecc-vertical.mp4` | 45-60s, 1080x1920, audio |
| `ecc-2-security-proof-wide.mp4` | 45-60s, 1920x1080, audio |
| `ecc-2-security-proof-vertical.mp4` | 45-60s, 1080x1920, audio |
| `ecc-2-money-proof-wide.mp4` | 30-45s, 1920x1080, audio |
| `ecc-2-money-proof-vertical.mp4` | 30-45s, 1080x1920, audio |
| `ecc-2-social-proof-wide.mp4` | 30-45s, 1920x1080, audio |
| `ecc-2-social-proof-vertical.mp4` | 30-45s, 1080x1920, audio |

## Flujo compatible con video-use

Usa la misma forma de producción que Video Use, manteniendo intacta la pila de medios específica de ECC:

1. Trata los datos de transcripción y línea de tiempo como la superficie de edición.
2. Mantén la inspección visual bajo demanda: filmstrips, composites de waveform/timeline o muestras de fotogramas solo en puntos de corte ambiguos.
3. Propón la estrategia de edición y la EDL antes de renderizar.
4. Corta de forma determinista con FFmpeg.
5. Añade superposiciones de prueba con Remotion o Manim cuando las afirmaciones del producto necesiten evidencia visual.
6. Exporta el MP4 más la línea de tiempo editable y el estado de subtítulos.
7. Ejecuta una autoevaluación de límites de corte, audio, subtítulos, fotogramas negros y afirmaciones del producto antes de cualquier subida o publicación social.

No vuelques fotogramas al repo. Las muestras de fotogramas usadas para la autoevaluación pertenecen al workspace local de la suite de lanzamiento.

## Plan de captura del navegador

Usa Browser o una captura de escritorio equivalente solo para material de prueba que deba estar actualizado el día del lanzamiento:

| Superficie | Captura |
| --- | --- |
| Repo de GitHub | hero del README, bloque de instalación, enlaces de patrocinio, notas de la versión |
| Plugin de Codex | ruta de instalación del marketplace del repo y README local del plugin |
| Paquete de OpenCode | instalación del paquete y banner del plugin |
| ECC Tools Pro | página de facturación/producto solo después de que el readback en vivo confirme las afirmaciones |
| AgentShield | salida de CLI, vista de categoría de política, gate de supply-chain |
| `ecc2/` | superficie alfa del plano de control/TUI con enmarcado alfa |

Si una superficie no está en vivo, usa una captura local del navegador y etiquétala como prueba local o de release candidate. No afirmes disponibilidad de marketplace, facturación o directorio oficial antes de que exista evidencia.

## Gate de autoevaluación

Ejecuta el validador:

```bash
ECC_VIDEO_SOURCE_ROOT=/path/to/ecc_2_raws \
ECC_VIDEO_RELEASE_SUITE_ROOT=/path/to/ecc_2_release_suite \
npm run release:video-suite -- --format json
```

Luego revisa manualmente el render final para verificar:

- que la autoevaluación del validador pase para el render principal: 90-150 segundos, al menos 1280x720, stream de video presente, stream de audio presente y salida no vacía;
- que la autoevaluación del validador pase para el conjunto de candidatas: MP4 principal más subtítulos y cinco clips cortos en formatos ancho y vertical;
- que la QA visual del validador reporte cero segmentos de fotogramas negros detectados para cada MP4 candidata;
- que no haya fotogramas en blanco ni exposición accidental del escritorio;
- que no haya nombre viejo del repo, pivot, rename ni framing solo-para-Claude en los subtítulos;
- que no haya subtítulos que reescriban el discurso en una afirmación falsa;
- que no haya URLs obsoletas, comandos de instalación antiguos o enlaces al repositorio anteriores al cambio de nombre;
- que no haya números internos de MRR salvo que la publicación los necesite explícitamente;
- continuidad de audio a través de cada corte;
- que los primeros 10 segundos digan claramente qué es ECC;
- que el CTA final dirija al repo, patrocinio, Pro o consultoría sin ruido.

## No publicar si

- `npm run release:video-suite` no está listo para las rutas de origen locales.
- El render principal está fuera del objetivo de 90-150 segundos.
- Los subtítulos mencionan el antiguo nombre del repositorio.
- La prueba del producto depende de pantallas privadas, secretos, datos de clientes o rutas locales en bruto.
- La URL de release, npm, plugin, facturación o marketplace supera la evidencia en `publication-readiness.md`.
