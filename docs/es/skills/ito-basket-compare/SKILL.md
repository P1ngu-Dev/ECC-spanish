---
name: ito-basket-compare
description: Compare Itô prediction-market baskets against a user's knowledge base, portfolio notes, financial context, watchlist, or research thesis. Use for read-only basket comparison and gap analysis without investment advice or live trading.
origin: ECC
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Comparación de cestas Itô

Usa este skill para comparar una cesta, tema o conjunto de mercados con la base de conocimiento, notas de portfolio, memo de investigación, contexto de CRM o tesis declarada del usuario.

Este skill es solo de lectura. No recomienda operaciones. Ayuda a inspeccionar encaje, exposición, supuestos y contexto faltante antes de que el usuario decida qué hacer.

## Guardrails

- No des consejos de inversión ni digas al usuario que compre, venda, mantenga, cubra, apalanque o dimensione una operación.
- No ejecutes, prepares ni envíes órdenes.
- No uses documentos privados salvo que el usuario los señale explícitamente.
- Usa `ITO_API_KEY` solo para datos de cestas/mercados Itô de solo lectura tras una petición explícita del usuario.
- Si comparas contra datos financieros, preserva la privacidad y resume solo los campos necesarios para la comparación.

## Modos de comparación

### Cesta vs base de conocimiento

1. Identifica el tema de la cesta y los underliers.
2. Recupera las notas, docs o snippets de memoria relevantes del usuario.
3. Mapea cada underlier a claims, fuentes, incertidumbres y supuestos obsoletos.
4. Devuelve señales alineadas, señales en conflicto y research faltante.

### Cesta vs notas de portfolio

1. Analiza watchlist, resumen de holdings o notas de exposición del usuario.
2. Compara temas, geografías, horizontes temporales y resultados de eventos.
3. Señala concentración, correlación y exposición narrativa duplicada.
4. Evita recomendaciones; formula la salida como inspección y preguntas.

### Cesta vs contexto financiero

1. Acepta solo contexto financiero proporcionado por el usuario o seleccionado explícitamente.
2. Identifica desajustes de liquidez, drawdown, horizonte temporal y restricciones.
3. Pregunta por restricciones faltantes en vez de adivinar.

## Contrato de salida

Usa esta estructura:

1. Resumen de la cesta
2. Objetivo de comparación
3. Coincidencias
4. Conflictos o supuestos obsoletos
5. Contexto faltante
6. Lista de acciones para el usuario

Termina con:

```text
This comparison is informational and not investment or trading advice.
```
