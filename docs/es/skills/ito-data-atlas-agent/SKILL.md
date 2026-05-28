---
name: ito-data-atlas-agent
description: Design background Data Atlas style agents for Itô basket research, market discovery, parameter drafting, and human-in-the-loop editing. Use for architecture and workflow planning, not live order execution.
origin: ECC
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Agente Itô Data Atlas

Usa este skill para diseñar un agente que vigila fuentes de datos, construye cestas candidatas de prediction markets, redacta cambios de parámetros y entrega el resultado a una persona para revisión.

Este skill describe arquitectura y flujo de trabajo. No ejecuta trading en vivo.

## Guardrails

- Mantén toda ejecución detrás de aprobación humana explícita.
- Requiere `ITO_API_KEY` solo para acceso de solo lectura a datos Itô, salvo que una implementación privada aparte añada controles de ejecución.
- No persistas datos privados del usuario salvo que el repo objetivo ya tenga un contrato de almacenamiento y el usuario lo pida.
- No expongas lógica privada de estrategia, credenciales de venues ni rutas locales en docs públicas.

## Patrón de arquitectura

Usa cuatro carriles:

1. Recolector de research: web pública, X, GitHub, docs de venue, metadatos de API y endpoints de lectura Itô cuando exista acceso controlado.
2. Redactor de cesta: convierte fuentes en underliers candidatos, pesos, reglas y preguntas.
3. Revisor de riesgo: comprueba frescura de datos, límites del venue, ambigüedad de resolución, notas de compliance y exposición a prompt injection.
4. Editor humano: abre un chat o estado de UI donde el usuario puede aprobar, rechazar, ajustar o pedir más research.

## Flujo de trabajo

1. Define el objetivo del usuario y las acciones excluidas.
2. Enumera las fuentes de datos y los requisitos de acceso.
3. Redacta una especificación de cesta con procedencia para cada underlier.
4. Produce parámetros editables, no órdenes ejecutables.
5. Guarda una trazabilidad: entradas, salida del modelo, fuentes y decisión humana.

## Cadenas de skills útiles

- `deep-research` para recolección de fuentes.
- `x-api` para señales sociales/eventos actuales.
- `ito-market-intelligence` para contexto de venue y underliers.
- `ito-basket-compare` para matching con la base de conocimiento del usuario.
- `prediction-market-risk-review` antes de cualquier integración capaz de ejecutar.

## Contrato de salida

Devuelve una especificación de workflow lista para implementar con:

- fuentes de datos
- gates de acceso
- roles de agente
- puntos de aprobación humana
- frontera de almacenamiento/auditoría
- no objetivos
