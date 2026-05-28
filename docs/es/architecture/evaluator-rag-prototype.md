# Prototipo de evaluator RAG

ECC 2.0 necesita un bucle de harness auto-mejorable que pueda aprender del
trabajo real sin mutar a ciegas la configuración de Claude, Codex, OpenCode,
dmux, Zed o terminal de un usuario. Este prototipo define el conjunto de
artefactos solo de lectura más pequeño para ese bucle.

El conjunto de fixtures vive en
[`examples/evaluator-rag-prototype/`](../../examples/evaluator-rag-prototype/).
Empezó con la limpieza de PRs obsoletos y la línea de rescate de mayo de 2026
porque esa línea tiene entradas reales, trabajo aceptado real y trabajo
rechazado real. El corpus ahora también incluye un escenario de readiness de
facturación/Marketplace para que el texto de lanzamiento no trate la evidencia
de release en dry-run ni la intención de roadmap como estado de facturación en
vivo. Un escenario de diagnóstico de fallos en CI añade el workflow de logs
primero necesario antes de que un agente proponga fixes para checks en rojo. Un
escenario de calidad de configuración del harness mantiene las recomendaciones
de MCP, plugin, hook, command, agent y adapter ligadas a la matriz de adapters
antes de que muten la guía de setup. Un escenario de excepción de policy de
AgentShield condiciona las excepciones de seguridad a evidencia SARIF/informe,
campos de owner, estado de expiración y decisiones de remediación frente a
excepción. Un escenario de evidencia de calidad de skills requiere evidencia
observada de fallo o feedback, ejemplos, huecos del set de referencia y
comandos de validación antes de que una enmienda de skill pueda promoverse. Un
escenario de evidencia de deep-analyzer requiere casos del corpus del analyzer,
comparaciones de output esperado y prueba de taxonomía de riesgo antes de que
pueda cambiar el comportamiento de análisis de repos o commits.

## Presión de referencia

- Meta-Harness: trata el propio harness como un experimento con specs de
  escenario, resultados de verifier y playbooks promovidos.
- Autocontext: almacena trazas, informes, artefactos y mejoras reutilizables
  antes de cambiar los assets instalados del agente.
- Claude HUD: expón contexto, herramientas, todos, actividad de agentes,
  checks y riesgo para que un evaluator pueda juzgar una ejecución después de
  los hechos.
- Hermes Agent: mantén explícitos los skills, la memoria, los seguimientos
  estilo scheduler y el comportamiento de gateway de terminal en lugar de
  ocultar los comandos locales.
- dmux, Orca, Superset y Ghast: preserva el estado de worktree/sesión para que
  el trabajo paralelo de agentes pueda compararse, reanudarse o cerrarse con
  limpieza.
- ECC Tools: enruta los hallazgos del evaluator a comentarios de PR, check runs
  e ítems de backlog de Linear sin inundar GitHub.

## Contrato de artefactos

Cada ejecución de evaluator/RAG es solo de lectura hasta que un verifier
promueve un playbook.

| Artifact | Propósito | Fixture |
| --- | --- | --- |
| Scenario spec | Declara el objetivo, la evidencia permitida, las acciones prohibidas y los gates de pass/fail. | `scenario.json` |
| Trace | Captura eventos de observación, recuperación, propuesta, verificación y promoción. | `trace.json` |
| Report | Resume puntuaciones, cobertura de evidencia, riesgos y la siguiente acción recomendada. | `report.json` |
| Candidate playbook | Describe el workflow mantenido por el maintainer que podría reutilizarse más adelante. | `candidate-playbook.md` |
| Verifier result | Acepta o rechaza candidatos con razones concretas y notas de rollback. | `verifier-result.json` |

El prototipo separa deliberadamente la recuperación de la acción. Una ejecución
puede recuperar diffs de PRs cerrados, estado de Linear, historial de CI y docs
locales, pero no puede cerrar, mergear, publicar, etiquetar ni reescribir
configuraciones como parte del paso de evaluator.

## Modelo de fases

1. Observa la cola actual, worktrees sucios, estado de ramas, PRs/issues
   abiertos, discusiones, estado de CI y gates de release.
2. Recupera evidencia de referencia relevante: filas del registro de stale
   salvage, PRs previos de maintainers, docs actuales, hallazgos del analyzer,
   fallos de CI y reglas de adapters del harness.
3. Propón uno o más playbooks con atribución de fuente y gates de validación
   esperados.
4. Verifica cada playbook contra reglas explícitas de aceptación y rechazo.
5. Promueve solo el candidato que mejore el escenario sin ampliar el radio de
   impacto.
6. Registra la guía de rollback y las colas de revisión manual sin resolver.

## Primer escenario

El primer escenario es `stale-pr-salvage-maintainer-branch`.

Modela la regla que Affaan estableció durante la limpieza de mayo de 2026: el
cierre obsoleto es higiene de cola, no pérdida de trabajo útil. El trabajo útil
de PRs cerrados debe portarse a PRs propiedad del maintainer con atribución y
backlinks, mientras que el churn generado, la localización masiva y el trabajo
ambiguo de traductor quedan fuera de los cherry-picks a ciegas.

El verifier acepta una rama de rescate de maintainer que:

- acredita los PRs fuente;
- evita contexto privado en bruto y rutas personales;
- no importa localización masiva obsoleta sin revisión de traductor;
- registra una actualización duradera del ledger;
- ejecuta los mismos gates de validación que un cambio normal de código, docs o catálogo;
- deja las acciones de publicación de release sujetas a aprobación.

El verifier rechaza una propuesta de cherry-pick a ciegas que:

- importe churn obsoleto de traducción/doc en bloque;
- omita la arquitectura actual de catálogo/instalación;
- carezca de atribución;
- carezca de tests o actualizaciones de ledger;
- mutile el estado de release o publicación de plugin.

## Fixtures del corpus

Los archivos raíz de fixture conservan el prototipo original
`stale-pr-salvage-maintainer-branch`. Los escenarios adicionales pueden vivir en
subdirectorios cuando reutilicen el mismo contrato de cinco artefactos.

Corpus actual:

- `stale-pr-salvage-maintainer-branch`: recupera trabajo útil de PRs cerrados a
  través de ramas propiedad del maintainer con atribución y validación.
- `billing-marketplace-readiness`: verifica claims de lanzamiento de billing,
  App y Marketplace antes de que el texto público diga que están activos.
- `ci-failure-diagnosis`: requiere logs de jobs fallidos, alcance de archivos
  cambiados y un comando de regresión nombrado antes de que pueda promoverse un
  playbook de fix de CI.
- `harness-config-quality`: requiere estado del adapter, ruta de instalación/
  onramp, comandos de verificación, notas de riesgo y comportamiento de
  preservación de config antes de que pueda promoverse una recomendación de
  setup del harness.
- `agentshield-policy-exception`: requiere evidencia SARIF o de informe de
  AgentShield, fuente del policy pack, campos de owner/ticket/ámbito/expiración
  y aplicación de excepciones expiradas antes de que pueda promoverse una
  excepción de policy.
- `skill-quality-evidence`: requiere alcance acotado del skill, evidencia
  observada de fallo o feedback de usuarios, cobertura de ejemplos/reference
  set, comandos de validación y seguridad de publicación antes de que pueda
  promoverse una enmienda de skill.
- `deep-analyzer-evidence`: requiere casos mantenidos del corpus del analyzer,
  comparaciones de output esperado, historiales representativos de
  repos/commits y comandos de regresión antes de que pueda promoverse el
  comportamiento de deep analysis.

## Mapeo de ECC Tools

ECC Tools ya marca la falta de evidencia RAG/evaluator para cambios de
retrieval, embedding, ranking y evaluator. Este prototipo les da una forma de
destino:

- `scenario.json` se mapea a entradas del corpus del analyzer.
- `trace.json` se mapea a golden traces y telemetría de ejecución.
- `report.json` se mapea a resúmenes para comentarios de PR y de backlog de
  Linear.
- `candidate-playbook.md` se mapea al cuerpo del PR de seguimiento sugerido.
- `verifier-result.json` se mapea a evidencia de check-run de pass/fail.

El trabajo futuro de ECC Tools debería consumir estos artefactos como forma de
fixture antes de añadir retrieval alojado o juzgado respaldado por modelos. El
prototipo local es suficiente para demostrar el contrato antes de introducir
una API de pago o un vector store.

## Reglas de promoción

Un candidato solo puede promoverse cuando:

- el resultado del verifier es `accepted`;
- al menos un candidato rechazado demuestra que el verifier puede decir que no;
- cada PR fuente o artefacto de referencia tiene atribución;
- la acción propuesta es propiedad del maintainer y reversible;
- los comandos de validación están nombrados;
- los ítems sin resolver de traductor, release, billing o publicación siguen
  bloqueados hasta aprobarse por separado.

## Próxima expansión

El corpus local de evaluator/RAG ya cubre los buckets de evidencia actuales. El
trabajo futuro debería consumir estos fixtures desde ECC Tools antes de añadir
retrieval alojado, almacenamiento vectorial, juzgado respaldado por modelos o
promoción automática de check-runs.
