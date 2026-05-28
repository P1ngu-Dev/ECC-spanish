# Mapa comando → agente / skill

Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

Este documento enumera cada comando slash y el/los agente(s) o skills principales que invoca, además de los agentes de invocación directa más relevantes. Úsalo para descubrir qué comandos usan qué agentes y para mantener consistentes los refactors.

| Comando | Agente(s) principal(es) | Notas |
|---------|------------------------|-------|
| `/plan` | planner | Planificación de implementación antes del código |
| `/tdd` | tdd-guide | Desarrollo guiado por tests |
| `/code-review` | code-reviewer | Revisión de calidad y seguridad |
| `/build-fix` | build-error-resolver | Corregir errores de build/tipos |
| `/e2e` | e2e-runner | Tests E2E con Playwright |
| `/refactor-clean` | refactor-cleaner | Eliminación de código muerto |
| `/update-docs` | doc-updater | Sincronización de documentación |
| `/update-codemaps` | doc-updater | Codemaps / docs de arquitectura |
| `/go-review` | go-reviewer | Revisión de código Go |
| `/go-test` | tdd-guide | Flujo TDD para Go |
| `/go-build` | go-build-resolver | Corregir errores de build en Go |
| `/python-review` | python-reviewer | Revisión de código Python |
| `/harness-audit` | — | Scorecard del harness (sin agente único) |
| `/loop-start` | loop-operator | Iniciar bucle autónomo |
| `/loop-status` | loop-operator | Inspeccionar estado del bucle |
| `/quality-gate` | — | Pipeline de calidad (tipo hook) |
| `/model-route` | — | Recomendación de modelo (sin agente) |
| `/orchestrate` | planner, tdd-guide, code-reviewer, security-reviewer, architect | Handoff multiagente |
| `/multi-plan` | architect (prompts de Codex/Gemini) | Planificación multimodelo |
| `/multi-execute` | architect / prompts frontend | Ejecución multimodelo |
| `/multi-backend` | architect | Backend multi-servicio |
| `/multi-frontend` | architect | Frontend multi-servicio |
| `/multi-workflow` | architect | Flujo general multi-servicio |
| `/learn` | — | continuous-learning skill, instincts |
| `/learn-eval` | — | continuous-learning-v2, evaluar y guardar |
| `/instinct-status` | — | continuous-learning-v2 |
| `/instinct-import` | — | continuous-learning-v2 |
| `/instinct-export` | — | continuous-learning-v2 |
| `/evolve` | — | continuous-learning-v2, agrupar instincts |
| `/promote` | — | continuous-learning-v2 |
| `/projects` | — | continuous-learning-v2 |
| `/skill-create` | — | script skill-create-output, historial git |
| `/checkpoint` | — | skill verification-loop |
| `/verify` | — | skill verification-loop |
| `/eval` | — | skill eval-harness |
| `/test-coverage` | — | análisis de cobertura |
| `/sessions` | — | historial de sesiones |
| `/setup-pm` | — | script de configuración de package manager |
| `/claw` | — | NanoClaw CLI (scripts/claw.js) |
| `/pm2` | — | ciclo de vida de servicios PM2 |
| `/security-scan` | security-reviewer (skill) | AgentShield vía skill security-scan |

## Agentes de uso directo

| Agente directo | Propósito | Alcance | Notas |
|----------------|-----------|---------|-------|
| `typescript-reviewer` | Revisión de código TypeScript/JavaScript | Proyectos TypeScript/JavaScript | Invoca el agente directamente cuando una revisión necesita hallazgos específicos de TS/JS y todavía no existe un comando slash dedicado. |

## Skills referenciadas por comandos

- **continuous-learning**, **continuous-learning-v2**: `/learn`, `/learn-eval`, `/instinct-*`, `/evolve`, `/promote`, `/projects`
- **verification-loop**: `/checkpoint`, `/verify`
- **eval-harness**: `/eval`
- **security-scan**: `/security-scan` (ejecuta AgentShield)
- **strategic-compact**: sugerido en puntos de compactación (hooks)

## Cómo usar este mapa

- **Descubribilidad:** Encuentra qué comando activa qué agente (por ejemplo, “usa `/code-review` para code-reviewer”).
- **Refactorización:** Al renombrar o eliminar un agente, busca en este documento y en los archivos de comando las referencias.
- **CI/docs:** El script de catálogo (`node scripts/ci/catalog.js`) produce conteos de agentes/comandos/skills; este mapa lo complementa con relaciones comando–agente.
