# Notas de la versión de ECC v1.10.0

## Posicionamiento

ECC v1.10.0 es una versión de sincronización de superficies y de la línea de operador.

El objetivo fue volver a hacer que el repo público, los metadatos del plugin, las rutas de instalación y la historia del ecosistema reflejen otra vez el estado real del proyecto, al tiempo que se siguen lanzando los flujos de trabajo de operador y las herramientas de medios que crecieron alrededor de la capa central del harness.

## Qué cambió

- Se sincronizó la superficie OSS en vivo a **38 agentes, 156 skills y 72 comandos**.
- Se actualizaron el plugin de Claude, el plugin de Codex, los metadatos del paquete de OpenCode y la documentación orientada a la versión a **1.10.0**.
- Se renovaron las métricas principales del repositorio para que coincidieran con el repo público en vivo (**140K+ stars**, **21K+ forks**, **170+ contributors**).
- Se amplió la línea de operador/flujo de trabajo con:
  - `brand-voice`
  - `social-graph-ranker`
  - `connections-optimizer`
  - `customer-billing-ops`
  - `google-workspace-ops`
  - `project-flow-ops`
  - `workspace-surface-audit`
- Se amplió la línea de medios con:
  - `manim-video`
  - `remotion-video-creation`
- Se añadió y estabilizó más cobertura de framework/dominio, incluyendo `nestjs-patterns`.

## Estado de ECC 2.0

ECC 2.0 es **real y utilizable como alfa**, pero **no está completo para disponibilidad general**.

Qué existe hoy:

- base de código del plano de control `ecc2/` en Rust en el repo principal
- `cargo build --manifest-path ecc2/Cargo.toml` pasa
- comandos `ecc-tui` actualmente disponibles:
  - `dashboard`
  - `start`
  - `sessions`
  - `status`
  - `stop`
  - `resume`
  - `daemon`

Qué significa esto:

- Puedes experimentar ahora con la superficie del plano de control.
- No deberías describir como terminado el roadmap completo de ECC 2.0.
- La formulación correcta hoy es **ECC 2.0 alfa / vista previa del plano de control**, no GA.

## Guía de instalación

Superficies de instalación actuales:

- plugin de Claude Code
- `ecc-universal` en npm
- manifiesto del plugin de Codex
- superficie de paquete/plugin de OpenCode
- CLI de AgentShield + npm + acción de GitHub Marketplace

Matiz importante:

- El plugin de Claude sigue limitado por las restricciones de distribución de `rules` a nivel de plataforma.
- La ruta de instalación selectiva / OSS sigue siendo la más fiable como instalación completa para equipos que quieren toda la superficie de ECC.

## Ruta de actualización recomendada

1. Actualiza a los metadatos más recientes del plugin/instalación.
2. Prefiere la ruta de instalación selectiva / OSS cuando necesites cobertura completa de reglas.
3. Usa AgentShield para guardrails y análisis del repositorio.
4. Trata ECC 2.0 como una superficie alfa del plano de control hasta que se reduzca de forma material el roadmap abierto P0/P1.
