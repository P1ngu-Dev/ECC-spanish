# Centro de comando de lanzamiento de hipercrescimiento de ECC 2.0

Fecha de captura: 2026-05-19.

Este es el mapa de ejecución para convertir ECC 2.0 en un lanzamiento público completo, un embudo para socios, un embudo para patrocinadores, una superficie de consultoría y un lanzamiento de contenido. Está escrito para operadores. Úsalo para decidir qué se publica, qué se anuncia y qué permanece bloqueado hasta que exista evidencia.

## Afirmación del lanzamiento

ECC 2.0 es el sistema de operador nativo del harness para trabajo agéntico.

La prueba pública debe mostrar el sistema real:

- skills, rules, hooks, convenciones MCP y gates de lanzamiento reutilizables;
- Claude Code, Codex, OpenCode, Cursor, Gemini, Zed, GitHub Copilot y flujos de solo terminal como superficies de ejecución compatibles;
- `ecc2/` como la dirección alfa del plano de control/TUI;
- Hermes como la shell opcional de operador para chat, cron, handoffs y enrutamiento del trabajo diario;
- ECC Tools Pro, GitHub Sponsors y consultoría como la superficie de negocio que financia la capa OSS.

Evita un lenguaje que lo presente como un cambio de nombre o un retroceso respecto al proyecto anterior.
La copia del lanzamiento debe mostrar directamente la forma del producto 2.0.

## Línea base de crecimiento actual

| Métrica | Actual | Objetivo | Brecha |
| --- | ---: | ---: | ---: |
| MRR | `$1,728/mo` | `$10,000/mo` | `$8,272/mo` |
| Movimiento de patrocinio | GitHub Sponsors activo más inbound abierto | Ciclo repetible de cierre de patrocinadores | outbound con aprobación previa |
| Movimiento de consultoría | Abierto, no principal | Paquetes listos para socios | prueba pública, charlas e intake |
| Movimiento de contenido | Candidatos a publicación de video de lanzamiento listos | Clips de lanzamiento semanales y prueba del fundador | aprobación del propietario, subida y URLs públicas |
| Movimiento de comunidad | Discord existe | Comunidad útil de codificación/operador | invitación, canales, fijados, moderación |

El crecimiento de MRR debería venir de cuatro vías a la vez:

- GitHub Sponsors y patrocinadores OSS de socios;
- suscripciones a ECC Tools Pro;
- contratos de consultoría e implementación;
- charlas, podcasts, demos en conferencias y webinars de socios que generen inbound.

## Segunda fase de hipercrescimiento

El lanzamiento debe comportarse como un motor de prueba, no como un anuncio de cambio de nombre.
Cada superficie pública debe dejar obvio el producto en la primera pantalla, clip, párrafo o demo:

| Flujo de trabajo | Prueba pública | Ruta de ingresos |
| --- | --- | --- |
| Categoría del producto | ECC como sistema de operador nativo del harness, no como un paquete de configuración solo para Claude | Convierte el tráfico OSS confundido en intención de instalación, Pro y patrocinio |
| Cobertura del harness | Claude Code, Codex, OpenCode, Cursor, Gemini, Zed, GitHub Copilot y flujos de terminal mostrados como superficies de ejecución | Conversaciones con herramientas, IDEs, proveedores de modelos y equipos de plataforma |
| Plano de control | superficie alfa de dashboard/status/session de `ecc2/` y shell de operador Hermes claramente enmarcada como dirección en vivo | consultoría y sprints de implementación para equipos |
| Confianza empresarial | AgentShield, supply-chain, release, observabilidad y gates de CI mostrados como evidencia repetible | proveedores de seguridad, proveedores de code-review, patrocinadores de plataforma y pilotos empresariales |
| Motor de medios | video principal de lanzamiento, cinco clips de prueba, capturas del navegador, transcripciones, EDLs, subtítulos y líneas de tiempo editables | alcance social, reservas para podcasts/charlas, prueba para patrocinadores, demos para socios |
| Embudo de comunidad | GitHub Discussions, Discord una vez aprobado, niveles de patrocinio, Pro y CTAs de consultoría enrutados sin ruido | inbound repetible, no picos puntuales de lanzamiento |

El ritmo operativo después del lanzamiento debería ser semanal:

1. un clip de prueba del producto;
2. un clip de prueba de seguridad o disciplina de lanzamiento;
3. un lote de outreach a socios/patrocinadores/charlas tras aprobación del propietario;
4. una discusión pública o prompt para la comunidad;
5. una devolución de métricas del embudo que cubra tráfico del repo, clics a patrocinio, conversiones a Pro, movimiento de MRR y respuestas inbound.

## Gates de lanzamiento

| Línea | Listo cuando | Acción actual |
| --- | --- | --- |
| Identidad del repo | README, metadatos del paquete, metadatos del plugin, docs de release, workflows y copia de lanzamiento usan `affaan-m/ECC` donde se necesitan URLs públicas | Barrido canónico de URLs |
| Publicación de paquete y plugin | los dry-runs de `ecc-universal@2.0.0-rc.1` salen limpios, npm `next` está aprobado, los dry-runs de tag del plugin de Claude pasan, el smoke de marketplace del repo Codex pasa, el build de OpenCode pasa | refrescar la evidencia de publicación desde el commit final |
| Prueba del producto | Quickstart, arquitectura entre harnesses, demo prompts, boundary alfa de `ecc2/`, prueba de seguridad de AgentShield y enlaces alojados de ECC Tools son consistentes | mantener concretas las superficies de prueba |
| Prueba de ingresos | los niveles de patrocinio, precios de Pro, CTA de consultoría, CTA de socios y lenguaje de readback de facturación están actualizados | no anunciar afirmaciones de facturación antes del readback en vivo |
| Prueba de contenido | el video de lanzamiento, clips cortos, capturas, notas de la versión, GitHub Discussion, X, LinkedIn y la publicación de formato largo están alineados | elegir los cortes finales de video, subir tras aprobación y adjuntar URLs públicas |
| Prueba de comunidad | la invitación a Discord, las reglas, los canales, el onboarding y el enrutamiento sponsor/comunidad están listos | requiere decisión sobre invitación/token antes de enlaces públicos |

## Suite de video

La línea de video debe usar la skill existente de edición de video de ECC más el modelo `browser-use/video-use` cuando sea útil: la transcripción como superficie de edición, aprobación de estrategia antes del render, cortes deterministas, salida de línea de tiempo/proyecto cuando esté disponible y autoevaluación antes de publicar.

Patrón de referencia: <https://github.com/browser-use/video-use>

Las clases de fuente principales ya existen en la biblioteca de medios local de ECC. Mantén fuera de los docs públicos las rutas absolutas; usa basenames o un manifiesto privado de producción al entregar trabajo a un editor o agente.

| Entregable | Duración | Material fuente | Objetivo de prueba |
| --- | ---: | --- | --- |
| Video principal de lanzamiento | 90-150s | `longform-full-wide.mp4`, `sf-longform-full.mp4`, `architecture-2-wide.mp4`, `terminal-scan-2-wide.mp4`, `new_site_raw.mp4` | ECC 2.0 como sistema de operador |
| Clip de prueba de instalación | 30s | instalación README, barrido terminal, quickstart, instalación del plugin | adopción con menos clics |
| Qué es ECC | 45-60s | `sf-thread-2-whatisecc.mp4`, `vertical-2-whatisecc.mp4`, `architecture-2-*` | claridad de categoría del producto |
| Prueba de seguridad | 45-60s | `sf-thread-4-security.mp4`, evidencia de AgentShield, gates de supply-chain | confianza empresarial |
| Clip de dinero/prueba | 30-45s | `thread-2-ghapp-money.mp4`, `metrics-ticker-2-*`, `gh_app_*.png` | credibilidad para patrocinadores, Pro y socios |
| Clip de cobertura/prueba social | 30-45s | `coverage-montage-wide.mp4`, `100k.png`, `star_history.png`, `x_analytics.png`, capturas de cobertura | palanca de distribución |

Pasos de producción:

1. Genera transcripciones para los clips largos y cortos.
2. Construye una lista de decisiones de edición con hook, prueba, demo, CTA de negocio y segmentos de CTA final.
3. Corta de forma determinista con FFmpeg.
4. Añade superposiciones y movimiento de datos en Remotion o Manim.
5. Añade subtítulos, corrección de color ligera, normalización de audio y reencuadres por plataforma.
6. Ejecuta una pasada de autoevaluación para marcos en blanco, subtítulos defectuosos, cortes bruscos, hook débil, prueba de producto faltante y URLs obsoletas.
7. Exporta los MP4 finales más el estado editable de la línea de tiempo/proyecto.

## Plan de distribución

| Canal | Activo | CTA |
| --- | --- | --- |
| GitHub Release | notas de la versión, quickstart, video de lanzamiento, enlace de patrocinio | star, instalar, patrocinar |
| GitHub Discussion | anuncio breve y bullets de prueba | preguntas, feedback, patrocinadores |
| X | hilo de lanzamiento, clip de instalación de 30s, clips de prueba | repo, patrocinio, Pro |
| LinkedIn | prueba de producto amigable para socios, CTA de consultoría | patrocinadores, consultoría, charlas |
| YouTube/Shorts/Reels/TikTok | video principal de lanzamiento y clips | repo, sitio, boletín/comunidad |
| Podcasts/charlas | pitch de una página, esquema de demo, prueba del fundador | reservas, socios |
| Outreach a patrocinadores | nota directa al patrocinador y tabla de niveles | GitHub Sponsors o Pro |

La fuente de verdad para la copia de patrocinadores, socios, consultoría, conferencias, podcasts y GitHub Discussion es
`docs/releases/2.0.0-rc.1/partner-sponsor-talks-pack.md`.
La fuente de verdad para la aprobación del propietario en acciones de release, paquete, plugin, video, facturación, social y outbound es
`docs/releases/2.0.0-rc.1/owner-approval-packet-2026-05-19.md`.

## Reglas de copia

Usa lenguaje directo de producto:

- `ECC 2.0 es el sistema de operador nativo del harness para trabajo agéntico.`
- `Una sola capa reutilizable para Claude Code, Codex, OpenCode, Cursor, Gemini, Zed, GitHub Copilot y flujos de terminal.`
- `OSS sigue siendo gratis. Los patrocinadores y Pro financian el trabajo.`
- `Usa ECC para skills, hooks, rules, convenciones MCP, gates de lanzamiento y flujos de trabajo de operador.`

Evita:

- `cambiamos el nombre del repo`;
- `pivot`;
- el encuadre de paquete de configuración heredado;
- `solo Claude`;
- lenguaje genérico de trayectoria del fundador;
- afirmaciones sobre facturación, pagos del marketplace o listados oficiales del directorio antes de que exista evidencia en vivo.

## Primer orden de construcción

1. Implementa las correcciones públicas de identidad del repo.
2. Actualiza las URLs de paquete, plugin, workflow, release y copia de lanzamiento.
3. Registra la evidencia final de publicación desde el commit exacto de release.
4. Mantén actualizados el manifiesto de la suite de video, las transcripciones, los candidatos a publicación y la QA visual con `npm run release:video-suite -- --format json`.
5. Captura desde navegador el README, la app ECC Tools, el flujo de instalación y las superficies de prueba relevantes para b-roll.
6. Elige el video principal de lanzamiento aprobado por el propietario y cinco clips cortos, luego súbelos y adjunta las URLs públicas finales.
7. Finaliza el GitHub release, el hilo de X, el post de LinkedIn, el anuncio de Discussion, la copia del email a patrocinadores, la intro de consultoría, el DM a socios y el pitch para podcasts/charlas.
8. Publica solo después de que los gates de npm, plugin, URL de release y readback de facturación estén en vivo o marcados explícitamente como bloqueados.

## Aprobaciones del propietario

Estas acciones necesitan aprobación humana o credencial antes de avanzar:

- enviar emails de actualización anual o patrocinio;
- actualizar el texto del perfil de LinkedIn;
- conectar Discord con un token de bot y un guild ID;
- publicar en npm o crear tags de plugin;
- anunciar facturación/pagos nativos;
- enviar outreach a socios, consultoría, conferencias, podcasts o patrocinadores;
- publicar la copia final de redes desde cuentas personales.
