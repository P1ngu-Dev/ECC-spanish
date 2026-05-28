# Notas de la versión de ECC v2.0.0-rc.1

## Posicionamiento

ECC v2.0.0-rc.1 es la primera superficie de release candidate para ECC como un sistema operativo entre harnesses para trabajo agéntico.

Claude Code sigue siendo un objetivo central. Codex, OpenCode, Cursor, Gemini y otros harnesses se tratan como superficies de ejecución que pueden compartir las mismas skills, rules, convenciones MCP y flujos de operador. ECC es el sustrato reutilizable; Hermes se documenta como la shell de operador que puede situarse encima de esa capa.

## Qué cambió

- Se añadió la guía saneada de configuración de Hermes a la historia pública del lanzamiento.
- Se añadió material de lanzamiento dentro del repo para que la versión pueda publicarse desde una sola superficie revisada.
- Se aclaró la separación entre ECC como sustrato reutilizable y Hermes como shell de operador.
- Se documentó el modelo de portabilidad entre harnesses para skills, hooks, MCPs, rules e instrucciones.
- Se añadió un playbook de importación de Hermes para convertir patrones locales de operador en skills publicables de ECC.
- Se añadió Zed como objetivo local de planificación/instalación mientras se mantienen los secretos BYOK y OpenRouter fuera de los archivos de proyecto gestionados por ECC.
- Se añadió cobertura de registro de comandos, auditoría de plataforma, auditoría de discusión, dashboard de operador, preparación de progreso Linear y gates de smoke del preview-pack.
- Se añadió un [gate local de preparación de observabilidad](../../architecture/observability-readiness.md) para estado de bucle, trazas de sesión, auditoría del harness y registros de riesgo de herramientas ECC2. <!-- TODO: no existe equivalente traducido; consulta la versión en inglés -->
- Se añadió el [skill pack público de prediction-market de Itô](ito-prediction-market-skill-pack.md)
  para investigación de cestas de solo lectura, comparación, inteligencia de mercado al estilo oracle
  y revisión de riesgos. El acceso vivo a la API de Itô sigue protegido y separado de la
  facturación de ECC Tools.
- Se añadió el skill pack de optimización derivado del rollout: ejecución paralela,
  bucles de benchmark, aceleración de throughput de datos, sistemas críticos de latencia y
  libros de decisiones recursivos.
- Se actualizó la evidencia de preparación del release después del seguimiento de la campaña Mini
  Shai-Hulud/TanStack de mayo de 2026, incluida la cobertura completa de IOC de AgentShield,
  comprobaciones de cola cero/discusión, un gate detallado de roadmap Linear, la instantánea
  del dashboard de operador del 18 de mayo y un ledger de URLs de release en vivo/pendiente
  para el gate de anuncio.

## Desde v1.10.0

La superficie rc.1 ahora incluye la dirección principal de 2.0 en lugar de una sola rama de
funcionalidad aislada:

- trabajo de sustrato entre harnesses para Claude Code, Codex, OpenCode, Cursor,
  Gemini, Zed y flujos de solo terminal;
- superficies más fuertes de publicación de paquete y plugin para npm, plugin de Claude,
  Codex repo-marketplace, OpenCode y metadatos de agentes;
- gates de operador para PRs, issues, discussions, trabajo heredado obsoleto, progreso Linear,
  evidencia de release y repetibilidad del dashboard;
- endurecimiento de supply-chain después de la campaña Mini Shai-Hulud/TanStack,
  incluyendo escaneo IOC, instalaciones CI sin lifecycle, actualización de fuentes de advisories,
  comprobaciones de npm audit/signature y objetivos de persistencia de herramientas de IA a nivel de usuario;
- espejos de roadmap empresarial de AgentShield para endurecimiento del gestor de paquetes,
  procedencia de paquetes de evidencia, exportación de políticas, promoción de políticas,
  enrutamiento de flota y telemetría de salida de GitHub Actions;
- espejos del roadmap de ECC Tools para análisis alojado, consumo de resúmenes de flota,
  hallazgo de rutas de evidencia, vinculación de rutas de política del harness, trazas de auditoría
  de juez de promoción alojada, preflight de anuncio de facturación y estado de readback de
  Marketplace en producción;
- expansión de documentación, localización japonesa, reparación de paridad zh-CN a ja-JP y
  preparación de dependencias mediante TypeScript 6 y actualizaciones de tipos de Node;
- material de lanzamiento para copia de GitHub release, X, LinkedIn, esquema de artículo,
  handoff de Telegram/Hermes, demo prompts, outreach a socios/patrocinadores/charlas y la
  checklist de lanzamiento protegida por aprobación.
- distribución protegida de Itô como teaser público de workflow, no como una afirmación de
  trading en vivo ni una fusión de la propiedad de ECC Tools e Itô.
- un ledger de URLs de release que separa los enlaces que ya resuelven de los enlaces que deben
  esperar al GitHub release, paquete rc de npm, tag/directorio del plugin y readback de
  facturación de ECC Tools.

## Por qué importa

ECC ya no es solo un plugin de Claude Code o un paquete de configuración.

El sistema ahora tiene una forma más clara:

- skills reutilizables en lugar de prompts de una sola vez
- hooks y tests para la disciplina del workflow
- acceso respaldado por MCP a docs, código, automatización del navegador e investigación
- superficies de instalación entre harnesses para Claude Code, Codex, OpenCode, Cursor y herramientas relacionadas
- Hermes como shell de operador opcional para chat, cron, handoffs y enrutamiento del trabajo diario

## Límites de release candidate

Esto es una release candidate, no la afirmación final de GA.

Qué se publica en esta superficie:

- documentación pública de configuración de Hermes
- notas de la versión y material de lanzamiento
- documentación de arquitectura entre harnesses
- guía de importación de Hermes para flujos de operador saneados
- evidencia de preparación de publicación para estado de cola, estado de discussion, cobertura del roadmap Linear, estado del dashboard de operador y seguimiento de supply-chain
- evidencia de smoke del preview-pack que demuestra que el pack público está ensamblado sin el estado privado de Hermes

Qué permanece local:

- secretos, tokens OAuth y claves API
- exportaciones privadas del workspace
- datasets personales
- automatizaciones específicas del operador que aún no se han saneado
- playbooks más profundos de CRM, finanzas y Google Workspace

## Movimiento de actualización

1. Sigue el [quickstart de rc.1](quickstart.md).
2. Lee la [guía de configuración de Hermes](../../HERMES-SETUP.md).
3. Revisa la [arquitectura entre harnesses](../../architecture/cross-harness.md).
4. Ejecuta el [gate de preparación de observabilidad](../../architecture/observability-readiness.md). <!-- TODO: no existe equivalente traducido; consulta la versión en inglés -->
5. Comprueba el [ledger de URLs de release](release-url-ledger-2026-05-19.md) antes de
   usar cualquier enlace de anuncio.
6. Empieza con una sola línea de workflow: ingeniería, investigación, contenido o outreach.
7. Importa solo patrones de operador saneados en skills de ECC.
8. Trata `ecc2/` como un plano de control alfa hasta que el empaquetado del release y el comportamiento del instalador estén finalizados.

## No tratar como publicado todavía

La copia de release candidate está lista para revisión final, pero el lanzamiento público sigue bloqueado por acciones protegidas por aprobación: el prerelease de GitHub, la publicación `next` de npm, el tag/ruta del plugin de Claude, el estado del Directorio de plugins de Codex, las URLs finales en vivo y cualquier anuncio de facturación o pagos nativos.
