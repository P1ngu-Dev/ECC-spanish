# Preparación de observabilidad de ECC 2.0

ECC 2.0 debe ser observable antes de volverse más autónomo. El valor local por
defecto es un gate de preparación opt-in, propiedad del repo, que comprueba si
las señales núcleo están presentes sin enviar telemetría a ningún sitio.

Ejecuta:

```bash
npm run observability:ready
node scripts/observability-readiness.js --format json
```

El gate es determinista y seguro para ejecutar en CI. Solo comprueba archivos
del repositorio e informa si la superficie de release puede exponer las señales
que necesita un operador.

## Modelo de señales

- Estado en vivo: `scripts/loop-status.js` puede emitir JSON, vigilar loops
  activos y escribir snapshots para dashboards o handoffs.
- Contrato HUD/estado: `docs/architecture/hud-status-session-control.md` y
  `examples/hud-status-contract.json` definen el payload portable para contexto,
  llamadas de herramienta, agentes activos, tareas pendientes, comprobaciones,
  coste, riesgo, colas, controles de sesión y sincronización de tracker.
- Trazas de sesión: `scripts/session-inspect.js` puede inspeccionar sesiones de
  Claude, dmux y respaldadas por adapters, y luego escribir snapshots canónicos.
- Línea base del harness: `scripts/harness-audit.js` proporciona una tarjeta de
  puntuación repetible para cobertura de herramientas, eficiencia de contexto,
  gates de calidad, persistencia de memoria, cobertura de evals, guardrails de
  seguridad y eficiencia de coste.
- Actividad de herramientas: `scripts/hooks/session-activity-tracker.js` registra
  eventos locales `tool-usage.jsonl` que ECC2 puede sincronizar.
- Registro de riesgo: `ecc2/src/observability/mod.rs` puntúa llamadas de
  herramienta y almacena un registro paginado para revisión.
- Sincronización de progreso: `docs/architecture/progress-sync-contract.md`
  define cómo GitHub, Linear, los handoffs locales, la hoja de ruta del repo y
  `scripts/work-items.js` se mantienen alineados durante lotes de merge y
  revisiones de gates de release.
- Seguridad de release: `docs/releases/2.0.0-rc.1/publication-readiness.md`, la
  evidencia de hardening posterior, la respuesta a incidentes de supply chain, la
  validación de seguridad de workflows, las comprobaciones de `npm pack` y las
  pruebas de la superficie de release deben estar presentes antes de cualquier
  tag público, publicación de paquete, envío de plugin o acción de anuncio.

## Presión de referencia

El ecosistema actual de tooling para agentes converge en las mismas necesidades
operativas:

- dmux, Orca y Superset enfatizan worktrees aislados más un único lugar para ver
  el estado del agente y el trabajo de merge/review.
- Claude HUD hace visibles el contexto, la actividad de herramientas, la
  actividad de agentes y el progreso de los todos dentro del bucle de coding.
- Autocontext registra cada ejecución como trazas, informes, artefactos y
  mejoras reutilizables.
- Meta-Harness trata el propio harness como algo que hay que evaluar y mejorar,
  lo que requiere logs limpios del comportamiento del proponente y de los
  resultados.
- Zed y OpenCode enfatizan superficies de control de agentes, cambios
  revisables y configuración específica del harness que aun así debe preservar
  conocimiento portable del proyecto.

La respuesta de ECC no es una dependencia de analítica alojada por defecto. El
primer gate de release-candidate es local y respaldado por archivos. La
telemetría alojada puede llegar después, pero solo cuando el modelo local de
eventos sea lo bastante útil como para confiar en él.

## Flujo de trabajo del operador

1. Ejecuta `npm run observability:ready`.
2. Ejecuta `npm run harness:audit -- --format json` para la tarjeta de
   puntuación más amplia del harness.
3. Ejecuta `node scripts/loop-status.js --json --write-dir .ecc/loop-status`
   durante lotes autónomos más largos.
4. Revisa `examples/hud-status-contract.json` antes de conectar un nuevo HUD o
   panel del operador.
5. Ejecuta `node scripts/session-inspect.js --list-adapters` para confirmar qué
   superficies de sesión están disponibles.
6. Ejecuta `node scripts/work-items.js sync-github --repo <owner/repo>` antes de
   confiar en el estado local de work-items para un repositorio rastreado.
7. Usa los logs de herramientas de ECC2 para operaciones de riesgo, análisis de
   conflictos y revisión de handoff antes de aumentar la autonomía.
8. Vuelve a ejecutar las comprobaciones de evidencia de seguridad de release
   antes de cualquier acción de release pública: publication readiness, respuesta
   a incidentes de supply chain, validación de seguridad de workflows, superficie
   de paquetes y pruebas de la superficie de release.

El estado final es práctico: antes de pedirle a ECC que ejecute bucles
multientidad más grandes, el operador puede demostrar que el sistema tiene
estado en vivo, trazas de sesión duraderas, tarjetas de puntuación base, un
registro local de riesgo y un contrato de sincronización de progreso que evita
que GitHub, Linear, los handoffs y la evidencia de la hoja de ruta se separen.
