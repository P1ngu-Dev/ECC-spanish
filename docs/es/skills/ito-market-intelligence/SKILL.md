---
name: ito-market-intelligence
description: Research prediction-market events, venues, underliers, liquidity, and news context for Itô basket workflows. Use for read-only market intelligence, API-gated Itô exploration, and source-grounded prediction-market briefings without investment advice or live trading.
origin: ECC
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Inteligencia de mercado Itô

Usa este skill cuando un usuario quiera contexto de prediction markets, descubrimiento de eventos, comparación de venues, exploración de temas de cestas o un brief de mercado respaldado por la API de Itô.

Este es un skill teaser público. Puede trabajar con fuentes públicas por defecto. Cualquier llamada de datos respaldada por Itô requiere acceso explícito mediante `ITO_API_KEY`.

## Guardrails

- No des consejos de inversión, legales, fiscales ni de trading.
- No coloques, canceles, envíes ni simules órdenes en vivo.
- No infieras la situación financiera del usuario salvo que la proporcione.
- Trata Polymarket, Kalshi, Itô, X, Exa, GitHub y la web como entradas de fuente, no como verdad por sí mismas.
- Separa hechos, señales implícitas del mercado e interpretación.

## Flujo de trabajo

1. Aclara el tema del mercado, el venue, la geografía y el horizonte temporal.
2. Reúne datos públicos de mercado desde docs/APIs del venue o research con base en fuentes.
3. Si `ITO_API_KEY` está presente y el usuario pide explícitamente datos de Itô, llama solo endpoints de lectura e indica que el acceso está restringido.
4. Normaliza diferencias entre venues de evento, underlier, liquidez, comisiones, resolución y latencia de datos.
5. Produce un brief de decisión:
   - resumen del mercado/evento
   - venues y underliers disponibles
   - advertencias sobre liquidez y calidad de datos
   - noticias/contexto de fuentes relevantes
   - preguntas abiertas antes de cualquier acción del usuario

## Cadenas de skills útiles

- Usa `deep-research` o `exa-search` para descubrir fuentes.
- Usa `x-api` para descubrir señales sociales públicas cuando X esté configurado.
- Usa `market-research` para tamaño de mercado, competidores o casos de negocio.
- Usa `prediction-market-risk-review` antes de cualquier flujo que toque capital del usuario, datos de portfolio o credenciales capaces de ejecutar.

## Contrato de salida

Por defecto, entrega un brief compacto con enlaces a fuentes y una advertencia clara:

```text
This is market intelligence, not investment or trading advice.
```

Si falta acceso, di:

```text
Itô live basket/API data requires gated access. Request an ITO_API_KEY before
using Itô-backed reads.
```
