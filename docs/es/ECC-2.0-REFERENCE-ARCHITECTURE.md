# Arquitectura de referencia de ECC 2.0

Espejo de ejecución actual:
[`ECC-2.0-GA-ROADMAP.md`](ECC-2.0-GA-ROADMAP.md).

Este documento convierte el barrido de referencia de mayo de 2026 en una forma
concreta de backlog para ECC. No es un segundo memorando de estrategia: toda la
presión de referencia que aparece abajo debe convertirse en un adaptador,
verificación, señal observable, política de seguridad, superficie de revisión de
PR o puerta de preparación para liberación.

## Línea base de referencia

Fecha de captura: 2026-05-12.

| Referencia | Presión principal sobre ECC 2.0 | Diferencia concreta en ECC |
| --- | --- | --- |
| [`stablyai/orca`](https://github.com/stablyai/orca) | IDE multiagente nativo de worktree con terminales, control de código fuente, integración con GitHub, SSH, notificaciones, modo diseño/navegador y cambio de cuenta, además de contexto por worktree. | Tratar el ciclo de vida del worktree, el estado de revisión, el estado de notificaciones y la identidad de cuenta/proveedor como señales de adaptador de primera clase. |
| [`superset-sh/superset`](https://github.com/superset-sh/superset) | Espacio de trabajo de agente de IA para escritorio con ejecución paralela, aislamiento de worktree, revisión de diffs, presets de espacio de trabajo y amplia compatibilidad con agentes CLI. | Añadir una taxonomía de presets de espacio de trabajo y hacer que el estado de sesión/worktree de ECC2 sea exportable en la medida necesaria para que editores externos lo consuman. |
| [`standardagents/dmux`](https://github.com/standardagents/dmux) | Orquestación de tmux/worktree, hooks de ciclo de vida, control multi-selección de agentes, fusión inteligente, explorador de archivos, notificaciones y limpieza. | Añadir cobertura de hooks de ciclo de vida a la matriz de harness y definir eventos de cola de fusión/conflicto. |
| [`aidenybai/ghast`](https://github.com/aidenybai/ghast) | Multiplexor de terminal nativo para macOS con espacios de trabajo agrupados por cwd, paneles, pestañas, arrastrar y soltar, búsqueda y notificaciones. | Conservar la ergonomía nativa de terminal mientras se añade agrupación por cwd/sesión y registros de handoff/sesión con búsqueda. |
| [`jarrodwatts/claude-hud`](https://github.com/jarrodwatts/claude-hud) | Línea de estado siempre visible de Claude Code para contexto, herramientas, agentes, tareas pendientes y actividad respaldada por transcripciones. | Formalizar la carga útil HUD/estado de ECC para contexto, costo, llamadas a herramientas, agentes activos, tareas pendientes, estado de cola, verificaciones y riesgo. |
| [`stanford-iris-lab/meta-harness`](https://github.com/stanford-iris-lab/meta-harness) | Búsqueda automatizada sobre el diseño del harness específico de la tarea: qué almacenar, recuperar y mostrar. | Dividir los bucles de mejora de ECC en especificación de escenario, traza del proponente, resultado del verificador y playbook promovido. |
| [`greyhaven-ai/autocontext`](https://github.com/greyhaven-ai/autocontext) | Mejora recursiva del harness usando trazas, informes, artefactos, datasets, playbooks y evaluadores separados por rol. | Almacenar trazas y playbooks reutilizables antes de mutar los activos del harness instalados. |
| [`NousResearch/hermes-agent`](https://github.com/NousResearch/hermes-agent) | Shell operador auto-mejorable con memorias, skills, scheduler, gateways, subagentes, backends de terminal y herramientas de migración. | Mantener ECC portátil entre backends de terminal locales, SSH, contenedores y alojados sin ocultar los comandos subyacentes. |
| [`anthropics/claude-code`](https://github.com/anthropics/claude-code), [`sst/opencode`](https://github.com/sst/opencode), Zed, Codex, Cursor, Gemini | Distintos harnesses de agentes exponen diferentes hooks, superficies de plugins, almacenes de sesión, archivos de configuración y bucles de revisión. | Mantener una matriz pública de conformidad de adaptadores en lugar de tratar a un solo harness como la UX canónica. |
| Revisión local del código fuente de Claude Code | Las superficies de sesión, herramientas, permisos, hooks, remotos, analíticas, tareas y sugerencias de contexto están más estructuradas de lo que sugiere la UX pública del CLI. | Modelar los eventos de estado y riesgo alrededor de mensajes de sesión, solicitudes de permisos, progreso de herramientas, presión de contexto y estado resumido. |

## Forma de la arquitectura

ECC 2.0 debería ser un sistema operativo de harness, no solo un catálogo de
comandos, agentes y skills.

```text
┌──────────────────────────────────────────────────────────────┐
│ Superficie del operador                                      │
│ CLI, plugin, TUI, HUD/statusline, puertas de liberación,     │
│ verificaciones de PR                                         │
├──────────────────────────────────────────────────────────────┤
│ Capa de adaptadores de harness                               │
│ Claude Code, Codex, OpenCode, Cursor, Gemini, Zed, dmux,     │
│ Orca, Superset, Ghast, solo terminal                         │
├──────────────────────────────────────────────────────────────┤
│ Runtime de worktree, sesión y colas                          │
│ worktrees, paneles, sesiones, tareas pendientes, verificac-  │
│ iones, colas de merge/conflicto, estado de notificaciones,    │
│ propiedad, exportaciones de handoff                          │
├──────────────────────────────────────────────────────────────┤
│ Bucle de observabilidad y evaluación                         │
│ trazas JSONL, snapshots de estado, registro de riesgo,       │
│ auditoría del harness, especificaciones de escenario,         │
│ verificadores, playbooks promovidos, conjuntos RAG           │
├──────────────────────────────────────────────────────────────┤
│ Seguridad y plataforma comercial                             │
│ políticas/SARIF de AgentShield, verificaciones de ECC Tools, │
│ facturación, sincronización con Linear/GitHub, informes      │
│ empresariales                                                │
└──────────────────────────────────────────────────────────────┘
```

## Mapa de referencia a backlog

### Orquestación de worktree y sesión

Adoptar de Orca, Superset, dmux y Ghast:

- Eventos del ciclo de vida del worktree: crear, reanudar, pausar, detener,
  diff, revisar, PR, listo para merge, conflicto, obsoleto, cerrar, rescatar.
- Agrupación de sesiones por repo, rama, cwd, tarea, propietario y harness.
- Presets de espacio de trabajo para la línea de liberación, la línea de triage
  de PR, la línea de documentación, la línea de seguridad y la línea de
  escritura de tests.
- Notificaciones para CI bloqueada, worktrees sucios, conflictos de merge,
  revisión obsoleta y ejecuciones autónomas finalizadas.
- Bucles de revisión que puedan anotar diffs y PRs sin quitar la propiedad a los
  mantenedores.

Trabajo en el repo:

- `everything-claude-code`: ampliar la matriz de conformidad de adaptadores y
  la vía de acceso pública al scorecard.
- `ecc2`: exponer el estado de sesión/worktree mediante una carga útil local
  estable antes de añadir telemetría alojada.
- `ECC-Tools`: consumir los mismos eventos de ciclo de vida para verificaciones
  de PR, enrutamiento de issues y sincronización con Linear.

Verificación:

- `npm run harness:audit -- --format json`
- `npm run observability:ready`
- pruebas dirigidas de la matriz de adaptadores una vez que la matriz pase de
  la documentación a los datos

### HUD, estado y observabilidad

Adoptar de Claude HUD y la revisión del código fuente de Claude Code:

- Presión de contexto: uso, riesgo de compactación, advertencias de resultados
  grandes y estado del resumen.
- Actividad de herramientas: herramienta activa, herramientas recientes,
  duración, operaciones de riesgo y solicitudes de permiso.
- Actividad de agentes: subagentes activos, tarea delegada, rama/worktree y
  estado de espera.
- Actividad de cola: PRs/issues abiertos, estado de CI, lotes obsoletos/
  en conflicto, estado de revisión y backlog de rescate de obsoletos cerrados.
- Costo/riesgo: estimación de costo de tokens, riesgo de operaciones
  destructivas, riesgo de hooks/MCP y estado del escaneo de seguridad.

Trabajo en el repo:

- Mantener `docs/architecture/observability-readiness.md` como la puerta de
  preparación orientada al operador.
- Definir un contrato JSON versionado de HUD/estado que tanto ECC2 como ECC
  Tools puedan consumir.
- Añadir exportaciones de ejemplo de `loop-status`, `session-inspect`, auditoría
  del harness y registro de riesgo en un directorio de fixtures antes de crear
  la UI visual.

Verificación:

- `npm run observability:ready`
- validación de fixtures para cada carga útil de estado
- prueba de humo multiplataforma para comandos que leen el historial de sesión

### Bucle de harness auto-mejorable

Adoptar de Meta-Harness, Autocontext y Hermes Agent:

- Separar el bucle en observación, propuesta, verificación, promoción y
  reversión.
- Almacenar cada mejora propuesta como traza más artefacto, no solo como un
  archivo final modificado.
- Promover playbooks solo después de que un verificador demuestre que mejoran
  un escenario sin ampliar el radio de explosión.
- Usar conjuntos RAG/de referencia para patrones ECC validados, historial del
  equipo, fallos de CI, resultados de revisión, calidad de la configuración del
  harness y decisiones de seguridad.

Trabajo en el repo:

- `everything-claude-code`: documentar especificaciones de escenario, contratos
  de verificador y reglas de promoción de playbooks.
- `ECC-Tools`: mapear hallazgos del analizador a comentarios de PR, check runs y
  tareas de Linear sin inundar el espacio de trabajo.
- `agentshield`: alimentar los hallazgos de prompt injection y riesgo de
  configuración en suites de regresión.

Prototipo actual:

- `docs/architecture/evaluator-rag-prototype.md` define el contrato de artefacto
  de evaluador/RAG de solo lectura.
- `examples/evaluator-rag-prototype/` registra la primera especificación de
  escenario, traza, informe, playbook candidato y resultado del verificador para
  el rescate de PR obsoletos.

Verificación:

- prototipo de solo lectura que emite una traza, informe, playbook candidato y
  resultado del verificador
- fixture de regresión que demuestra que se rechaza una propuesta mala

### Plataforma de seguridad empresarial AgentShield

AgentShield debería pasar de un escáner útil a una plataforma de seguridad
empresarial.

Forma del backlog:

- Esquema de políticas para baseline de organización, severidad de regla,
  propietario, excepción, expiración, evidencia y rastro de auditoría.
- Salida SARIF para GitHub code scanning.
- Paquetes de políticas para OSS, equipo, empresa, regulado, hooks/MCP de alto
  riesgo y aplicación de CI.
- Inteligencia de cadena de suministro para paquetes MCP, procedencia de npm/
  pip, CVEs, typosquats y reputación de dependencias.
- Corpus de prompt injection y benchmark de regresión.
- Salida de informes JSON y HTML/PDF ejecutivo.

Verificación:

- pruebas unitarias de esquema
- pruebas de fixtures SARIF
- pruebas doradas de paquetes de políticas
- pruebas de regresión de falsos positivos a partir del historial público de
  issues

### Plataforma comercial y de revisión ECC Tools

ECC Tools debería convertirse en la capa nativa de GitHub para facturación,
análisis profundo, verificaciones de PR y seguimiento de progreso en Linear.

Forma del backlog:

- Auditoría nativa de facturación de GitHub Marketplace antes de cualquier
  anuncio de pagos: planes, asientos, mapeo org/account, estado de suscripción,
  comportamiento de sobreuso, comportamiento de downgrade/cancelación y modos
  de fallo.
- Analizador profundo comparable en alcance a las partes útiles de GitGuardian,
  Dependabot, CodeRabbit y Greptile: evidencia de seguridad, riesgo de
  dependencias, recomendaciones de CI/CD, comportamiento de revisión de PR,
  calidad de configuración, riesgo de tokens/costo y deriva del harness.
- Conjunto RAG/de referencia sobre patrones ECC validados, resultados históricos
  de PR, avisos de dependencias, fallos de CI, decisiones de revisión y
  convenciones específicas del equipo.
- Sincronización con Linear que asigne hallazgos al estado del proyecto,
  evidencia de hitos y issues listos para el responsable sin agotar los límites
  de issues.

Verificación:

- pruebas de fixtures de check-run
- pruebas de reproducción de webhooks de facturación
- fixtures dorados de PR para el analizador
- fixture de dry-run de sincronización con Linear

### Línea de rescate de PR obsoletos cerrados

Cerrar PRs obsoletos mantiene útil la cola pública, pero el trabajo valioso no
debería perderse porque una persona colaboradora ya no tenga tiempo para rebase.

Regla de ejecución:

1. Cerrar PRs obsoletos, con conflicto u obsoletos con un comentario cortés y
   claro.
2. Registrarlos en un ledger de rescate con PR de origen, autor, motivo del
   cierre, archivos/conceptos útiles, riesgo y acción recomendada al mantenedor.
3. Después del lote de limpieza, inspeccionar manualmente el diff de cada PR
   cerrado.
4. Hacer cherry-pick solo cuando el parche siga aplicándose limpiamente y
   preserve la arquitectura actual. Si no, reimplementar la idea útil en una
   rama nueva del mantenedor.
5. Preservar la atribución en el cuerpo del commit o en el cuerpo del PR.
6. Responder al PR de origen cuando el trabajo útil se incorpore, enlazando el
   PR del mantenedor o el commit fusionado.
7. Marcar el elemento del ledger como incorporado, sustituido, rastreado en
   Linear o sin acción.

Salvaguardas requeridas:

- Nunca hacer cherry-pick ciego de churn generado, localización masiva o cambios
  de versión mayor de dependencias.
- Preferir PRs pequeños del mantenedor antes que una mega-rama de rescate.
- Ejecutar las mismas puertas de validación que en cambios normales de código,
  documentación o catálogo.
- Mantener el crédito del colaborador incluso cuando la implementación final se
  reescribe.

## Orden de implementación a corto plazo

1. Ampliar la matriz de adaptadores del harness y la vía de acceso pública al
   scorecard.
2. Mantener actualizada la lista de verificación de publicación de release/
   nombre/plugin con evidencia fresca del commit final antes de la publicación
   de rc.1.
3. Definir el contrato JSON de HUD/estado y el directorio de fixtures.
4. Iniciar el esquema de políticas de AgentShield más las fixtures SARIF.
5. Auditar las superficies de facturación y check-run de ECC Tools.
6. Inventariar carpetas heredadas y PRs obsoletos cerrados en el ledger de
   rescate.
7. Portar trabajo útil obsoleto en pequeños PRs atribuidos del mantenedor.

## No objetivos

- Telemetría alojada antes de que el modelo local de eventos sea útil y
  testeable.
- Mutación automática de las configuraciones de harness del usuario sin evidencia
  de verificador.
- Tratar cualquier único harness de agentes como la interfaz canónica.
- Anuncios de release o pagos antes de que la evidencia de comandos, paquetes,
  marketplace y facturación sea reciente.
