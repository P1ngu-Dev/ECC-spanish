# Plantilla de capability de producto

Usa esto cuando la intención del producto existe pero las restricciones de implementación todavía están implícitas.

El propósito es crear un contrato de capability duradero, no otro doc de planificación vago.

## Capability

- **Nombre de la capability:**
- **Source:** PRD / issue / discusión / roadmap / nota del founder
- **Actor primario:**
- **Resultado después del ship:**
- **Señal de éxito:**

## Intención de producto

Describe la promesa visible para el usuario en un párrafo corto.

## Restricciones

Lista las reglas que deben ser verdad antes de empezar la implementación:

- reglas de negocio
- límites de alcance
- invariantes
- restricciones de rollout
- restricciones de migración
- restricciones de compatibilidad hacia atrás
- restricciones de billing / auth / compliance

## Actores y superficies

- actor(es)
- superficies UI
- superficies API
- superficies de automatización / operador
- superficies de reporting / dashboard

## Estados y transiciones

Describe el ciclo de vida en términos de estados explícitos y transiciones permitidas.

Ejemplo:

- `draft -> active -> paused -> completed`
- `pending -> approved -> provisioned -> revoked`

## Contrato de interfaz

- entradas
- salidas
- side effects requeridos
- estados de fallo
- reintentos / recuperación
- expectativas de idempotencia

## Implicaciones de datos

- fuente de verdad
- nuevas entidades o campos
- límites de ownership
- expectativas de retención / borrado

## Seguridad y policy

- fronteras de confianza
- requisitos de permisos
- rutas de abuso
- requisitos de policy / governance

## No objetivos

Lista lo que esta capability explícitamente no posee.

## Preguntas abiertas

Captura las decisiones sin resolver que bloquean la implementación.

## Handoff

- **¿Listo para implementación?**
- **¿Necesita revisión de arquitectura?**
- **¿Necesita aclaración de producto?**
- **Siguiente línea de ECC:** `project-flow-ops` / `tdd-workflow` / `verification-loop` / otra
