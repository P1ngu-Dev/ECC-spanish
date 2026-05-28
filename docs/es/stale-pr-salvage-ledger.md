# Registro de rescate de PR obsoletos

Este registro documenta trabajo útil recuperado de PR obsoletos, en conflicto o cerrados.
La regla es simple: la limpieza de colas cierra los PR obsoletos, pero no descarta
trabajo útil. Los mantenedores deben inspeccionar el diff cerrado, portar las partes compatibles
en ramas nuevas y acreditar el PR de origen.

## Estados de clasificación

| Estado | Significado |
| --- | --- |
| Rescatado | El trabajo útil se portó al `main` actual mediante un PR de mantenedor. |
| Ya presente | El `main` actual ya contenía el trabajo útil antes del rescate. |
| Reemplazado | El `main` actual resolvió el mismo problema de otra manera. |
| Omitido | El PR era accidental, demasiado amplio, inseguro o tenía muy poca señal como para portarlo. |
| Revisión de traductor/manual | El contenido puede ser útil, pero necesita revisión humana de idioma/dominio antes de importarlo. |

## Rescatado al `main` actual

| PR de origen | Contribución original | Resultado del rescate |
| --- | --- | --- |
| #1232 | Flujo de trabajo `search-before-creating` de `skill-scout` | Rescatado en el pase de mantenedor de costo/skill-scout del 12 de mayo, con redacción actual del repositorio, validación de fuentes externas y sin ediciones obsoletas del conteo del catálogo. |
| #1304 | Skill de seguimiento de costos y comando `/cost-report` | Rescatado en el pase de mantenedor de costo/skill-scout del 12 de mayo, con convenciones actuales de comando/skill y sin precios de modelo fijados de forma obsoleta. |
| #1309 | Material del proyecto de trading/comunidad | Rescatado en #1761 como un README neutral de proyecto comunitario. |
| #1310 | Reviewer de Django, resolver de compilación y guía de tareas asíncronas con Celery | Rescatado en el pase de mantenedor de Django/Celery del 12 de mayo, con conteos actuales del catálogo y limpieza menor de ejemplos. |
| #1322 | Traducción al vietnamita del README | Rescatado en #1764 como `docs/vi-VN/README.md` más actualizaciones de selectores. |
| #1325 | Guía del framework Quarkus, agentes Java y material de localización | Rescatado en #1771 y #1803; no se copiaron las ediciones obsoletas de docs amplios/conteos. |
| #1326 | Skill de Angular developer y reglas | Rescatado en #1763 con el skill actual, reglas, wiring de instalación y actualizaciones del catálogo. |
| #1328 | Corrección de stdout UTF-8 en Windows para continuous-learning | Rescatado en #1761. |
| #1329 | Endurecimiento de detección de instalación de plugins | Rescatado en #1761 mediante soporte actual de detección de auditoría del harness. |
| #1334 | Skill de E2E para escritorio Windows | Rescatado en #1762 con wiring de instalación, paquete y catálogo. |
| #1352 | Target de instalación de Qwen | Rescatado en #1738 a través del target de instalación actual de Qwen. |
| #1413 | Skills/agentes de red y homelab | Rescatado mediante #1729, #1731, #1745 y #1778. |
| #1414 | Reglas de F#, agente reviewer y skill de testing | Rescatado en #1770 con manifests de instalación actuales, tests de detección y wiring del catálogo. |
| #1429 | Target de instalación de JoyCode | Rescatado en #1737 a través del target de instalación actual de JoyCode. |
| #1467 | Skills científicos y trabajo de descubrimiento de OpenCode | Partes útiles de USPTO y gget rescatadas en #1740; no se copiaron las afirmaciones generadas obsoletas. |
| #1478 | Reglas de HarmonyOS/ArkTS, agente resolver y ejemplo de CLAUDE | Rescatado en #1769 con wiring de instalación actual; no se conservaron las ediciones obsoletas de sesión/TUI `ecc2`. |
| #1493 | Scope de contexto en SessionStart | Rescatado en #1774 con semántica y tests actuales de hooks. |
| #1498 | Flujo de planificación PRD | Rescatado en #1777. |
| #1504 | Hooks de statusline/monitor de contexto | Rescatado en #1776 con estructura actual del manifest de hooks y tests. |
| #1528/#1529/#1547 | Soporte de proveedor Astraflow y UModelVerse | Rescatado en #1775 con wiring actual de proveedores y análisis defensivo de tool-call. |
| #1558 | Skill `agentic-os` | Rescatado en #1772. |
| #1559 | Skill `error-handling` | Rescatado en #1772. |
| #1566 | Skill de auditoría de arquitectura de agentes | Rescatado en #1772. |
| #1578 | Endurecimiento de file-probe de OpenCode | Rescatado en #1773. |
| #1603 | Skill `plan-orchestrate` | Rescatado en #1766 con wiring actual de manifest/catalog. |
| #1658 | Supresión de falsos positivos del code-reviewer | Rescatado en el pase de mantenedor de code-reviewer del 12 de mayo, con redacción actual del agente revisor, una compuerta de prueba para hallazgos HIGH/CRITICAL, exclusiones comunes de falsos positivos y una prueba de regresión. |
| #1659 | Dirección de diseño frontend y skills de pulido de interfaces | Rescatado en el pase de mantenedor de frontend-design del 12 de mayo, con layout canónico de `skills/` y guía frontend actual de ECC, preservando la regla del repositorio de que el skill oficial `frontend-design` debe instalarse desde `anthropics/skills`. |
| #1674 | Skill de auditoría de producción | Rescatado en #1732 después de revisión de cadena de suministro/privacidad y reescritura. |
| #1687 | Sincronización de localización zh-CN | Grandes subconjuntos seguros rescatados en #1746-#1752; las piezas restantes requieren revisión de traductor/manual. |
| #1694 | Curación de portfolio | Actualizaciones útiles de curación enfocada rescatadas en #1723 y #1724. |
| #1695 | Traducción al ruso del README | Portado en #1722. |
| #1697 | Configuración guardada del selector LLM | Rescatado como parte del trabajo de config de proveedores/esquema de herramientas en #1720. |
| #1699 | Protección de ruta post-edit-format en Windows | Portado en #1719. |
| #1700 | Serialización de herramientas del proveedor | Portado en #1720. |
| #1705/#1780 | Sistema de motion para UI de producción | Rescatado en #1772, #1781 y #1782, con ejemplos corregidos antes de la fusión. |
| #1713 | Soporte para el lenguaje Swift | Portado en #1721. |
| #1715 | Endurecimiento del validador de rutas personales en CI | Portado mediante el endurecimiento del validador de CI en #1717. |
| #1727 | Skill de patrones de MySQL | Rescatado en #1733. |
| #1757 | Flujo de trabajo de ingeniería de aprendizaje automático | Rescatado en #1758 y ajustado en #1759. |

## Paso de brechas 2026-05-12

El registro inicial de cierres obsoletos cubría el cohorte de limpieza P0 y las ramas de
rescate más grandes. Un pase posterior de brechas sobre los PR cerrados el 2026-05-11 encontró
elementos adicionales útiles que ya estaban presentes en `main` o que todavía valía la pena
portar.

| PR de origen | Disposición |
| --- | --- |
| #1310 | Portado mediante la rama mantenedora de Django/Celery después de confirmar que `agents/django-reviewer.md`, `agents/django-build-resolver.md` y `skills/django-celery/SKILL.md` seguían faltando. |
| #1325 | El material útil del framework Quarkus ya se había preservado en #1771 y #1803; el `main` actual contiene las reglas/skills de Quarkus más las superficies de Java reviewer/build-resolver. |
| #1360 | Ya presente como `skills/security-bounty-hunter/`. |
| #1414 | El soporte útil de F# ya se había preservado en #1770; el `main` actual contiene las reglas de F#, el agente reviewer, el skill de testing, el wiring de instalación y las pruebas de detección. |
| #1415 | Ya presente como `skills/vite-patterns/`. |
| #1478 | El soporte útil de HarmonyOS/ArkTS ya se había preservado en #1769; el `main` actual contiene las reglas de ArkTS, el agente resolver, el ejemplo de CLAUDE y el wiring de instalación. |
| #1438 | Ya presente como `skills/ui-to-vue/`. |
| #1504 | Ya mapeado a #1776 en la tabla de rescate duradera. |
| #1508 | Ya presente como `skills/fastapi-patterns/` y `agents/fastapi-reviewer.md`. |
| #1563/#1564/#1565 | Revisión de traductor/manual: las sincronizaciones del README en zh-TW, tr y pt-BR pueden contener actualizaciones útiles de localización, pero el texto obsoleto de README/version/conteos debe revisarse con los responsables del idioma antes de importarlo. |
| #1567 | Ya presente como el bypass de file-gate del subagente GateGuard actual en `scripts/hooks/gateguard-fact-force.js`, con compuertas Bash preservadas y pruebas de regresión en `tests/hooks/gateguard-fact-force.test.js`. |
| #1570 | Ya presente como imports públicos de `llm.prompt`, construcción de `PromptBuilder` basada en palabras clave y helpers de registro de plantillas; los tests actuales registran el marcador `unit` a través de `tests/conftest.py`. |
| #1584 | Ya presente como ruta rápida de notificación nativa de escritorio de iTerm2 en `scripts/hooks/desktop-notify.js`, con fallback a `osascript` para multiplexores. |
| #1589 | Ya presente como detección con comillas de `actions/checkout` en `scripts/ci/validate-workflow-security.js` más pruebas de regresión con comillas dobles/simples. |
| #1594 | Ya presente como manejo de reachability de MCP HTTP que trata las respuestas de prueba HTTP 400, 401 y 403 como reachable/con control de auth, con pruebas de hooks. |
| #1597 | Ya presente como validación de conteos de catálogo para README, AGENTS, docs zh-CN, `.claude-plugin/plugin.json` y `.claude-plugin/marketplace.json`. |
| #1602 | Ya presente como la deprecación v1 de `continuous-learning` que redirige el uso nuevo a `continuous-learning-v2` mientras preserva la superficie archivada de v1. |
| #1603 | El trabajo útil de `/plan-orchestrate` ya se había preservado en #1766 con metadatos actuales de paquete/catálogo. |
| #1604 | Omitido: el instalador local de Windows para arrastrar y soltar copia archivos directamente y ejecuta `git pull`; el flujo actual de instalador administrado/perfil es más seguro y lo reemplaza. |
| #1609 | Revisión de traductor/manual: la traducción al persa del README puede ser útil, pero necesita revisión de idioma y actualización actual de catálogo/versiones antes de importarse. |
| #1613 | Ya presente en `rules/web/hooks.md` como el ejemplo de PostToolUse con `tsc --incremental` más timeout acotado. |
| #1631 | Ya presente en `scripts/hooks/suggest-compact.js` y `tests/hooks/hooks.test.js`; el código actual lee `session_id` del JSON de stdin antes de caer a `CLAUDE_SESSION_ID`. |
| #1648 | Ya presente en `src/llm/providers/claude.py`; el proveedor Claude actual recoge todos los bloques de contenido de texto y tool-use y cubre el comportamiento en `tests/test_claude_provider.py`. |
| #1658 | Portado mediante la rama mantenedora de code-reviewer después de confirmar que la compuerta de prueba de falsos positivos y la lista común de omisión de falsos positivos seguían faltando. |
| #1693 | Ya presente como `skills/redis-patterns/`. |

## Ya presente o reemplazado

| PR de origen | Disposición |
| --- | --- |
| #1306 | Los workarounds de bugs de hooks ya existen en `main` como `docs/hook-bug-workarounds.md`. |
| #1318 | La utilidad de adaptación del agente Gemini ya estaba presente en el `main` actual. |
| #1323 | La actualización de la configuración de hooks ya estaba presente en el `main` actual. |
| #1337 | La actualización del conteo del catálogo fue reemplazada por la sincronización actual de conteos del catálogo. |
| #1631 | El aislamiento de `session_id` de stdin en `suggest-compact` ya estaba presente en el `main` actual con pruebas de hooks. |
| #1608 | El manejo inseguro de apertura de documentos/terminal del dashboard ya estaba presente en el `main` actual mediante helpers seguros en tiempo de ejecución y apertura de documentos limitada al proyecto. |
| #1678 | El comportamiento fallback de `.cmd`/`.bat` para MCP en Windows ya estaba presente en el `main` actual con las pruebas de health-check vigentes. |
| #1682/#1701 | Las correcciones de ruta del hook strategic compact se fusionaron directamente o fueron reemplazadas por correcciones actuales de docs. |
| JARVIS #4/#5/#6 | PR obsoletos con fallos solo por dependencias; el estado futuro de dependencias debería regenerarse con Dependabot. |

## Limpieza de cola amplia del propietario 2026-05-18

Los repos de la release de ECC ya estaban limpios, pero un barrido de `gh search` a nivel de propietario
encontró colas obsoletas en proyectos públicos/privados antiguos. La limpieza cerró 24
PR obsoletos de dependabot y 72 issues obsoletos de roadmap heredado de pagos/0EM,
luego cerró los 9 PR finales obsoletos/generados/en conflicto/de prueba y 5
issues heredados/de outreach/de marcador. El namespace de propietario `affaan-m` ahora está en 0
PRs abiertos y 0 issues abiertos según `gh search` en vivo. La evidencia detallada antes/después
y la disposición final de la cola están registradas en
`docs/releases/2.0.0-rc.1/owner-queue-cleanup-2026-05-18.md`.

| Alcance | Disposición |
| --- | --- |
| PRs de Dependabot en `stoictradingAI`, `Behavioral_RL`, `dprc-autotrader-v2`, `x-algorithm-score`, `polycule-secure` y `pragmAItism_defAInce` | Omitidos como bumps de dependencias generadas obsoletos; regenéralos desde la base actual si aún se necesitan. |
| Issues heredados en `payments0-api`, `payments0-sdk`, `agent-payments-gateway`, `0EM_Frontend`, `0em-payments-dashboard` y `yield-optimizer` | Reemplazados por las líneas nativas de pagos de ECC Tools, análisis alojado, billing-readback y roadmap de Linear/proyecto. |
| Repos archivados tocados para cierre de PR | `stoictradingAI`, `dprc-autotrader-v2`, `polycule-secure` y `pragmAItism_defAInce` volvieron a su estado archivado después del cierre del PR obsoleto. |
| Barrido final de PR/issues | Cerró los bundles ECC generados restantes, PRs obsoletos de renombre de Cloudflare, PR obsoleto de README-card, PR de prueba/ruido, issues públicos de outreach e issue vacío de marcador. Se preservaron los hallazgos de `dexploy#25` en Linear `ITO-62` antes del cierre. |

## Omitidos

| PR de origen | Motivo |
| --- | --- |
| #1308 | La sincronización obsoleta de zh-CN revertiría o borraría demasiado estado actual del árbol; el arreglo concreto de enlace de selector ya estaba presente. |
| #1320 | La eliminación del gestor de paquetes entra en conflicto con la política actual de CI para npm/pnpm/yarn/bun. |
| #1341 | Cambio generado muy grande y de baja señal, sin una unidad de rescate enfocada y segura. |
| #1416/#1465 | PRs accidentales de fork-sync sin contribución enfocada. |
| #1475 | La idea de puente con Gemini CLI de una sola línea estaba demasiado obsoleta y poco especificada para portarse de forma segura. |
| #1604 | El instalador de Windows por arrastrar y soltar elude el instalador administrado actual, realiza copias amplias directas y ejecuta `git pull` desde un script de instalación local. |

## Pendientes restantes de revisión manual

La cola restante, potencialmente útil, es trabajo de traducción/localización que es
inseguro portar automáticamente sin revisión del responsable del idioma. Este remanente está adjunto a
Linear ITO-55 y no es una tarea de rescate que bloquee la release; el trabajo de release debería
solo verificar que la cola sigue registrada y excluida de importaciones ciegas:

- #1687 cola residual de localización zh-CN
- #1609 traducción al persa del README
- #1563 sincronización del README zh-TW
- #1564 sincronización del README turco
- #1565 sincronización del README pt-BR

Regla de manejo:

1. Mantén estos PRs en revisión de traductor/manual.
2. Divide cualquier trabajo futuro por superficie: agentes, comandos, docs de nivel superior, superficies de release y conteo, y luego skills.
3. No importes docs de nivel superior obsoletos que arrastren datos viejos de versión o conteo de catálogo.
4. No reabras PRs antiguos salvo que el autor original vuelva con un rebase actual; el rescate del lado del mantenedor debe hacerse en ramas nuevas con atribución.

## Regla de limpieza futura

Para cada lote de limpieza de PRs obsoletos/en conflicto:

1. Cierra o comenta en el PR según la política de cola.
2. Agrega el PR de origen a este registro o a un registro sucesor con fecha.
3. Clasifícalo como rescatado, ya presente, reemplazado, omitido o
   revisión de traductor/manual.
4. Si es útil, porta un pequeño fragmento compatible en una rama nueva del mantenedor.
5. Acredita el PR y autor de origen en el cuerpo del PR del mantenedor.
