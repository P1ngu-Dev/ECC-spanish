# Hoja de ruta de investigación enterprise de AgentShield

Generado: 2026-05-12; actualizado con la fleet-ticket de AgentShield del 18 de
mayo y evidencia IOC de Mini Shai-Hulud.

Este es un artefacto de planificación para la siguiente iteración enterprise de
AgentShield. No modifica el código de AgentShield. El objetivo es convertir el
scanner actual, el gate de policy, el corpus y la superficie de reporting en un
control plane de seguridad para equipos que ejecutan agentes de coding AI en
múltiples harnesses.

## Evidencia revisada

Estado actual del repositorio AgentShield:

- Checkout de AgentShield en `main` limpio.
- Estructura actual de módulos en `README.md`, `API.md`, `package.json`,
  `.github/workflows/*` y `src/`/`tests/`.
- Superficies de usuario actualmente soportadas: `agentshield scan`,
  `agentshield init`, `agentshield miniclaw start`, JSON del scanner, MiniClaw
  API, GitHub Action, HTML, SARIF, markdown, terminal y reports JSON.
- Superficies tipo enterprise actuales: policy packs, enforcement de policy en
  GitHub Action, violaciones de policy en SARIF, procedencia de supply chain,
  benchmark de corpus, reports ejecutivos HTML y auditoría del ciclo de vida de
  excepciones.

Referencias externas revisadas desde repos oficiales de GitHub o fuentes README:

- [stablyai/orca](https://github.com/stablyai/orca): IDE multiagente,
  aislamiento de worktree, estado en vivo de agentes, integración con GitHub,
  review de diffs y notificaciones.
- [superset-sh/superset](https://github.com/superset-sh/superset): editor de
  agentes AI con orquestación de worktree, review de diffs integrado, presets de
  workspace y compatibilidad universal con agentes CLI.
- [standardagents/dmux](https://github.com/standardagents/dmux): multiplexor
  tmux/worktree con hooks de ciclo de vida, lanzamientos multiagente, visibilidad
  de panes y workflows de merge/PR.
- [jarrodwatts/claude-hud](https://github.com/jarrodwatts/claude-hud):
  statusline de Claude Code, salud del contexto, actividad de herramientas,
  tracking de agentes, progreso de todos, parsing de transcript y telemetría de
  uso.
- [stanford-iris-lab/meta-harness](https://github.com/stanford-iris-lab/meta-harness):
  optimización de harness mediante tareas repetibles, interacciones de proponentes
  registradas y cambios de scaffold evaluados.
- [greyhaven-ai/autocontext](https://github.com/greyhaven-ai/autocontext):
  bucle de mejora recursiva con trazas, generaciones puntuadas, playbooks,
  conocimiento persistido, evaluación de escenarios y trazas opcionales de
  producción.
- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent):
  skills auto-mejorables, memoria, búsqueda de sesiones, gateway multiplataforma,
  automatización programada, backends de terminal y generación de trayectorias.
- [anthropics/claude-code](https://github.com/anthropics/claude-code):
  superficies de terminal, IDE, GitHub, plugin, permisos, MCP y retención de
  datos.
- [anomalyco/opencode](https://github.com/anomalyco/opencode): agente de coding
  open-source agnóstico al provider con agentes build/plan, beta de escritorio,
  arquitectura cliente/servidor y soporte LSP.
- [opencode-ai/opencode](https://github.com/opencode-ai/opencode): agente de
  terminal en Go archivado antes, con sesiones, providers, LSP, tracking de
  cambios de archivo, comandos personalizados y auto-compact.
- [zed-industries/zed](https://github.com/zed-industries/zed): editor
  multiusuario de alto rendimiento con expectativas estrictas de licencia y CI de
  cumplimiento.
- [aidenybai/ghast](https://github.com/aidenybai/ghast): multiplexor nativo de
  terminal basado en Ghostty, agrupación de workspaces, paneles divididos,
  drag/drop, notificaciones y búsqueda en terminal.

Inspección local del código fuente de Claude Code:

- Se revisó solo la forma de archivos/módulos locales no secretos desde una
  instantánea privada del source de Claude Code.
- Superficies relevantes observadas: `tools/`, `utils/permissions/`,
  `utils/mcp/`, `utils/hooks/`, `utils/plugins/`, `types/permissions.ts`,
  `types/plugin.ts`, `remote/`, `tasks/`, `assistant/sessionHistory.ts` y
  utilidades de sesión/historial.
- No se copió código. La conclusión es que AgentShield debe seguir permisos,
  plugins, MCP, hooks, sesiones remotas, actividad de tasks/subagents e historial
  como dominios de auditoría de primera clase, en lugar de tratar un árbol
  `.claude/` como única fuente de verdad.

## Posición actual de AgentShield

AgentShield ya es más que una herramienta estática de lint:

- La cobertura de reglas abarca secretos, permisos, hooks, servidores MCP,
  configs de agentes, prompt injection, supply chain, análisis de taint,
  ejecución en sandbox, evaluación de policy, reparación/estado runtime, validación
  de corpus, MiniClaw y análisis Opus.
- Los reports sirven para humanos y máquinas: terminal, JSON, markdown, HTML,
  SARIF, logs de scan y outputs de GitHub Action.
- Existen hooks enterprise: policy packs, metadatos de excepción, reporting de
  excepciones expiradas/expiradas, code scanning en SARIF y salida de resumen de
  jobs.
- El trabajo de precisión está activo: `runtimeConfidence`, ponderación de
  templates/ejemplos, rebajas para ejemplos docs, confianza de plugin cache
  instalado de Claude, resolución de hook-manifest, guía de auditoría de falsos
  positivos y readiness del corpus.
- El consumo de evidence-pack ya es lo bastante primero en clase para tools
  downstream: `agentshield evidence-pack inspect` verifica un bundle y emite
  resúmenes compactos JSON/texto para puntuación de report, conteos de hallazgos,
  confianza runtime, policy, baseline, supply chain, contexto CI, remediación y
  errores de artefactos malformados.
- El consumo de evidence-pack a nivel de fleet ahora tiene un primitive de
  routing local: `agentshield evidence-pack fleet <dirs...> [--json]` agrega
  múltiples bundles inspeccionados en rutas ready, security-blocker,
  policy-review, baseline-regression, supply-chain-review e invalid.
- ECC-Tools ya consume ese primitive de fleet en la revisión de seguridad alojada:
  `agentshield-evidence/fleet-summary.json` enruta packs inválidos,
  bloqueadores de seguridad, revisiones de policy, regresiones de baseline y
  revisiones de supply chain en hallazgos alojados.

Actualización del 16 de mayo: PR #87 de AgentShield se fusionó como
`26bb44650663816d07180e0d20c1895e431a326c`. Clasifica el contenido cacheado del
plugin instalado de Claude como `runtimeConfidence: plugin-cache`, mantiene el
impacto de puntuación del plugin-cache sin secretos en `0.5x`, evita degradar
rutas locales de repo no-Claude en `plugins/cache` y hace que la clasificación
de plugin-cache gane antes de que las implementaciones de hooks cacheadas se
presenten como `hook-code` activa. PR #88 se fusionó como
`65ed6e2a87545dc99d962b58413f49096a4d70ec`. Añade
`agentshield evidence-pack inspect <dir> [--json]`, valida el bundle antes de la
lectura, resume cada artefacto de evidencia visible para el consumidor y evita
que los artefactos JSON malformados pero válidos hagan fallar la inspección.
PR #89 se fusionó como `521ada9091bb6d818511ab8589ae675b920c106a`. Añade
`agentshield evidence-pack fleet <dirs...> [--json]`, verifica cada pack a
través de la ruta inspect, agrega totales de hallazgos, policy, baseline,
supply chain y remediación, y asigna a cada pack una ruta de fleet determinista.
El commit `840952a7a07f820f24081c43df656d7f7295f23b` de AgentShield añade
payloads de ticket de fleet listos para Linear/operator con prioridad, labels,
títulos y cuerpos Markdown. El mismo commit amplía la cobertura actual de IOC de
Mini Shai-Hulud/TanStack para el endpoint Vault en clúster y el breadcrumb
temporal del lockfile, con typecheck local, lint, tests completos, `git diff
--check` y evidencia de GitHub CI/Self-Scan/Action-test.

La siguiente iteración después del routing de fleet no debería ser “añadir más
reglas regex” por defecto. El follow-up routing de ECC-Tools ahora consume
resúmenes de fleet y expone rutas de evidencia fuente en hallazgos alojados, y el
primer slice de policy cross-harness ya vincula rutas de destino de fleet de
AgentShield a revisión por owner del harness. El output de fleet de AgentShield
ahora también emite `reviewItems` con rutas de evidencia fuente y
recomendaciones listas para el owner, además de payloads de ticket listos para
copiar para packs enrutados. El movimiento de mayor valor es la aprobación/readback
duraderos del operador y la automatización de workflows para hallazgos de fleet
enrutados.

## Brechas enterprise

### 1. Baselines de organización y drift

Los compradores enterprise necesitan saber si un repo, equipo o fleet de agentes
está volviéndose más seguro o más riesgoso con el tiempo. AgentShield ya tiene
logs de scan y módulos de comparación con baseline, y PR #63 ahora expone ese
drift mediante inputs, outputs, anotaciones y evidencia de job-summary de GitHub
Action. PR #64 añade creación de snapshots de baseline como primera clase a
través de `agentshield baseline write`. La superficie de producto restante debe
hacer explícitos los resúmenes de drift CLI, los evidence packs y los deltas
listos para owner.

Capacidad objetivo:

- `agentshield baseline write --path .claude --output agentshield-baseline.json`
- `agentshield scan --baseline agentshield-baseline.json`
- Secciones de report para hallazgos nuevos, corregidos, sin cambios,
  suprimidos y exceptuados por policy.
- Output de GitHub Action que publique “security posture changed” en lugar de
  solo una calificación puntual.

### 2. Adaptadores de seguridad multi-harness

El mercado se está moviendo hacia muchos harnesses de agente en paralelo, no
hacia una sola herramienta. Orca, Superset, dmux, OpenCode, Claude Code, Codex,
Gemini, Zed y multiplexores de terminal crean superficies de seguridad
diferentes.

Capacidad objetivo:

- Un registry pequeño de adapters para `claude-code`, `opencode`, `codex`,
  `gemini`, `zed`, `dmux`, `orca`, `superset` y `generic-terminal`.
- Cada adapter declara rutas de config, conceptos de permisos, superficies de
  plugin, convenciones de MCP/herramientas, superficies de historial/sesión y
  evidencia de CI.
- El output de report agrupa hallazgos por harness y confianza, para que los
  hallazgos de templates/docs no parezcan exposiciones runtime activas.

### 3. Conciencia de sesión y worktree

Los orquestadores nativos de worktree cambian el modelo de riesgo. Un equipo
puede ejecutar muchos agentes en paralelo, cada uno con su propia rama, shell,
config MCP y estado local.

Capacidad objetivo:

- Metadatos de scan opcionales para rama, ruta de worktree, nombre de agente, id
  de sesión, provider y orchestrator.
- Una tabla de historial de scan que responda: qué worktree introdujo un nuevo
  permiso, qué ejecución de agente añadió un MCP riesgoso, qué rama relajó la
  policy y si la rama final mergeada lo corrigió.
- Un resumen compacto tipo “security HUD” utilizable por statuslines, checks de
  GitHub y dashboards locales.

### 4. Evidence packs para compradores y auditores

Los reports HTML son el artefacto orientado al comprador correcto hoy; el PDF
nativo queda diferido. La necesidad más profunda es un bundle de evidencia
portable que pueda adjuntarse a auditorías, revisiones de seguridad y
cuestionarios de clientes.

Capacidad objetivo:

- `agentshield scan --evidence-pack out/agentshield-evidence`
- El bundle incluye report JSON, report HTML, SARIF, evaluación de policy,
  auditoría de excepciones, diff de baseline, resumen de dependencias/procedencia
  y un README corto que explique cómo interpretar los artefactos.
- Modo de redacción opcional para secretos, rutas locales, usernames y nombres de
  proyecto.

### 5. Corpus de regresión y conjuntos de referencia

Meta-Harness y Autocontext apuntan a la misma lección: las mejoras necesitan
escenarios puntuados, trazas y playbooks. AgentShield ya tiene un benchmark de
corpus, pero la confianza enterprise necesita un set de referencia curado para
falsos positivos, falsos negativos y regresiones de policy.

Capacidad objetivo:

- Fixtures de escenarios versionados para reglas críticas, supresiones de falsos
  positivos, excepciones de policy, ejemplos de templates/docs, manifests de
  plugin y resolución de hook-code.
- Reporting de precisión/cobertura por categoría, no solo readiness agregado.
- Un gate de “no accuracy regression” que debe pasar antes de los releases.
- Notas de playbook sobre por qué existe una supresión y cuándo debe expirar.

### 6. Workflow de remediación

Las herramientas de seguridad se vuelven enterprise cuando convierten hallazgos
en trabajo responsable sin inundar a los maintainers.

Capacidad objetivo:

- Rama de remediación generada por CLI o con un clic para transformaciones
  seguras.
- Comentarios de policy que agrupen hallazgos por owner y riesgo en lugar del
  orden de archivos.
- Soporte de GitHub App para anotaciones de check-run, caps de issues, sync con
  Linear y exportación de backlog diferido.
- Fingerprints de hallazgo que eviten issues duplicados entre scans repetidos.

### 7. Inteligencia de amenazas y reputación de paquetes

La seguridad de agentes depende de paquetes MCP, repos de plugins, bundles de
actions y ecosistemas CLI que cambian rápido. Los checks estáticos necesitan una
capa externa de reputación mantenida.

Capacidad objetivo:

- Un caché local-first de threat intel para riesgos conocidos de MCP/paquetes,
  CVEs, nombres de paquetes malware, scripts de instalación sospechosos,
  dependencias git mutables y paquetes conocidos como buenos.
- El modo offline determinista sigue disponible.
- El enriquecimiento online es opt-in y produce procedencia clara para cada
  afirmación externa.

### 8. Controles comerciales y de equipo

AgentShield ya está conectado conceptualmente a ECC Tools GitHub App. Los pagos
nativos de GitHub hacen más concreto el camino del producto: scans locales
gratuitos, gates de policy org pagados, evidence packs pagados y drift/historial
pagados.

Capacidad objetivo:

- Checks de GitHub sensibles al tier: scan estático gratuito, enforcement de
  policy org pagado, evidence packs pagados, drift histórico pagado y análisis
  profundo pagado.
- Mapeo seat/equipo para owners de policy y aprobadores de excepciones.
- Comprobaciones de readiness de billing compartidas con ECC-Tools para que el
  estado de pago nunca cambie silenciosamente el comportamiento de enforcement.

## Orden de build recomendado

### Slice 1: MVP de drift de baseline

Implementa el primitive de control plane enterprise más pequeño: compara este
scan con el último baseline aceptado.

Artefactos:

- Schema JSON de baseline.
- Escritor y comparador de baseline.
- Secciones de report terminal y JSON para hallazgos nuevos/corregidos/sin
  cambios.
- Tests que cubran fingerprints estables, hallazgos corregidos, hallazgos nuevos
  y carry-forward de excepciones de policy.

Por qué primero:

- Reutiliza el output de scan existente.
- Mejora a la vez el CLI, GitHub Action y el valor del GitHub App.
- No requiere un servicio alojado.

### Slice 2: Bundle de evidence pack

Agrupa los reports máquina y humano existentes en un artefacto de auditoría
portable.

Artefactos:

- Flag CLI `--evidence-pack <dir>`.
- README redacted del bundle.
- Archivos HTML, JSON, SARIF, policy, excepción y diff de baseline.
- Tests para layout de archivos, redacción y nombres de output deterministas.

Por qué segundo:

- Convierte el trabajo de reporting existente en prueba lista para compradores.
- Mantiene el PDF nativo diferido y aun así cubre las necesidades de handoff de
auditoría.

### Slice 3: Registry de adapters del harness

Haz explícito el soporte del harness en lugar de implícito.

Artefactos:

- Metadatos de adapter para Claude Code, OpenCode, Codex, Gemini, dmux,
  terminal genérico y templates locales del proyecto.
- Output de descubrimiento que reporte qué adapters coincidieron y por qué.
- Agrupación de report por adapter.
- Tests usando directorios fixture para cada adapter.

Por qué tercero:

- Alinea AgentShield con el posicionamiento agnóstico al harness de ECC.
- Crea una superficie estable para futuras integraciones con Zed, Orca,
  Superset y Hermes sin fingir que todos los harnesses comparten el modelo de
  config de Claude.

### Slice 4: Gate de precisión del corpus

Promueve el corpus de benchmark a gate de release.

Artefactos:

- Report de corpus por categoría.
- Umbrales requeridos por categoría.
- Snapshots de regresión para supresiones conocidas de falsos positivos.
- Entrada de checklist de release que requiera readiness del corpus antes de
  publicar.

Por qué cuarto:

- Evita que la credibilidad enterprise se degrade a medida que crecen las reglas.
- Crea una ruta duradera para bucles de mejora estilo Meta-Harness/Autocontext
  más adelante.

### Slice 5: Wiring de GitHub App y sync con Linear

Conecta los hallazgos de AgentShield al routing de follow-up de ECC-Tools.

Artefactos:

- Fingerprints de hallazgo compatibles con los caps de issues de ECC-Tools.
- Exportación de backlog lista para Linear para drift de baseline y violaciones
  de policy.
- Anotaciones de check-run agrupadas por owner/riesgo.
- Tests que aseguren que los scans repetidos no generen spam de issues duplicados.

Por qué quinto:

- Necesita el trabajo de baseline/fingerprint de Slice 1.
- Es el puente desde el CLI local al workflow pagado del equipo.

## No objetivos de esta iteración

- Generación nativa de PDF, salvo que los workflows de comprador/compliance
  requieran explícitamente PDF generado en lugar de HTML más print-to-PDF.
- Dashboards alojados antes de que los contratos locales de baseline/evidence/
  fingerprint estén estables.
- Fine-tuning o entrenamiento de modelos antes de que existan gates de corpus
  deterministas y trazas de referencia.
- Reescrituras automáticas amplias de código para hallazgos riesgosos sin
  transformaciones y tests explícitos y revisables.

## Gates de aceptación

La iteración enterprise de AgentShield no está completa hasta que esto sea
verdad:

- `npm run typecheck`, `npm run lint`, `npm test` y `npm run build` locales
  pasan desde la raíz del repositorio de AgentShield.
- Los smoke tests del CLI construido cubren los nuevos flags o modos de report.
- La auto-prueba de GitHub Action cubre el nuevo output visible en CI.
- La documentación nombra por separado la ruta free/local y la ruta pagada/de
  equipo.
- Los cambios de runtime-confidence incluyen evidencia de scan en vivo que
  demuestre que las superficies de plugin/paquete de menor confianza siguen
  visibles en lugar de suprimidas.
- La evidencia producida por la feature es lo bastante determinista para
  diffs en CI.
- ECC-Tools puede consumir los fingerprints de hallazgo o la exportación de
  backlog sin exceder los caps de objetos de GitHub/Linear.
- La hoja de ruta de GA y el estado del proyecto en Linear enlazan a los PRs de
  AgentShield fusionados.
