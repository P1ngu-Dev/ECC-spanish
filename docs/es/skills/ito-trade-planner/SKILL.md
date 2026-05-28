---
name: ito-trade-planner
description: Build a non-advisory prediction-market trade planning worksheet for Itô or venue workflows. Use to inspect venues, underliers, constraints, order prerequisites, and manual execution steps without placing trades or recommending positions.
origin: ECC
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Planificador de operaciones Itô

Usa este skill cuando un usuario quiera una hoja de trabajo estructurada para una idea de prediction market, un ajuste de cesta, una comparación de venues o un plan de ejecución manual.

El skill es intencionalmente no ejecutor. Produce checklists y tablas de parámetros que el usuario puede revisar manualmente.

## Guardrails

- No digas que una operación es buena, mala, óptima ni recomendada.
- No des consejos de inversión ni de tamaño de posición.
- No coloques, canceles, envíes ni firmes órdenes.
- No pidas claves privadas, seed phrases, contraseñas de exchange o credenciales de wallet.
- Exige aprobación explícita del usuario antes de que cualquier flujo pase de research a herramientas capaces de ejecutar.

## Flujo de planificación

1. Reexpresa la idea del usuario como una hipótesis neutral.
2. Identifica mercados, venues, underliers, reglas de resolución, comisiones y restricciones de frescura de datos.
3. Si `ITO_API_KEY` está configurado y se solicita, lee metadatos de cesta Itô.
4. Construye una hoja de trabajo manual:
   - market/underlier
   - venue
   - fuente de datos
   - precio o estado observable actual
   - regla de resolución
   - advertencia de liquidez
   - preguntas abiertas
   - enlace a acción manual o siguiente revisión
5. Ejecuta `prediction-market-risk-review` antes de hablar de automatización, claves, auth del venue o restricciones de capital.

## Lenguaje permitido

Usa:

- "manual planning worksheet"
- "questions to answer before acting"
- "observable venue data"
- "risk and constraint review"

Evita:

- "you should buy/sell"
- "best trade"
- "guaranteed"
- "risk-free"
- "optimal size"

## Contrato de salida

Termina cada plan con:

```text
This is a planning worksheet, not investment or trading advice. Review venue
rules and make any trading decisions yourself.
```
