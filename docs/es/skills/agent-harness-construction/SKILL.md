---
name: agent-harness-construction
description: Design and optimize AI agent action spaces, tool definitions, and observation formatting for higher completion rates.
origin: ECC
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->
# Construcción del arnés del agente

Utilice esta habilidad cuando esté mejorando la forma en que un agente planifica, llama a las herramientas, se recupera de errores y converge al finalizar.

## Modelo central

La calidad de la salida del agente está limitada por:
1. Calidad del espacio de acción
2. Calidad de la observación
3. Calidad de la recuperación
4. Calidad del presupuesto contextual

## Diseño del espacio de acción

1. Utilice nombres de herramientas explícitos y estables.
2. Mantenga las entradas primero en el esquema y limitadas.
3. Devolver formas de salida deterministas.
4. Evite las herramientas generales a menos que el aislamiento sea imposible.

## Reglas de granularidad

- Utilizar microherramientas para operaciones de alto riesgo (implementación, migración, permisos).
- Utilice herramientas medianas para bucles comunes de edición/lectura/búsqueda.
- Utilice macroherramientas sólo cuando los gastos generales de ida y vuelta sean el costo dominante.

## Diseño de observación

Cada respuesta de herramienta debe incluir:
- `status`: éxito|advertencia|error
- `summary`: resultado de una línea
- `next_actions`: seguimientos procesables
- `artifacts`: rutas de archivo/ID

## Contrato de recuperación de errores

Para cada ruta de error, incluya:
- pista de causa raíz
- instrucción de reintento seguro
- condición de parada explícita

## Presupuesto contextual

1. Mantenga los avisos del sistema mínimos e invariantes.
2. Trasladar una gran orientación a habilidades cargadas bajo demanda.
3. Prefiera las referencias a archivos en lugar de insertar documentos largos.
4. Compacto en los límites de fase, no en umbrales de token arbitrarios.

## Guía de patrones de arquitectura

- ReAct: mejor para tareas exploratorias con camino incierto.
- Llamada de funciones: mejor para flujos deterministas estructurados.
- Híbrido (recomendado): planificación de ReAct + ejecución de herramientas escritas.

## Evaluación comparativa

Pista:
- tasa de finalización
- reintentos por tarea
- pase@1 y pase@3
- costo por tarea exitosa

## Antipatrones

- Demasiadas herramientas con semánticas superpuestas.
- Salida de herramienta opaca sin sugerencias de recuperación.
- Salida de solo error sin siguientes pasos.
- Sobrecarga del contexto con referencias irrelevantes.