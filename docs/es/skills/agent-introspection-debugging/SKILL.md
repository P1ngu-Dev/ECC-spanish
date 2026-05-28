---
name: agent-introspection-debugging
description: Structured self-debugging workflow for AI agent failures using capture, diagnosis, contained recovery, and introspection reports.
origin: ECC
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->
# Depuración de introspección del agente

Utilice esta habilidad cuando la ejecución de un agente falla repetidamente, consume tokens sin progreso, utiliza las mismas herramientas o se desvía de la tarea prevista.

Esta es una habilidad de flujo de trabajo, no un tiempo de ejecución oculto. Enseña al agente a depurarse sistemáticamente antes de pasar a ser humano.

## Cuándo activar

- Llamada máxima de herramienta/fallos de límite de bucle
- Reintentos repetidos sin avance
- Crecimiento del contexto o deriva inmediata que comienza a degradar la calidad de la producción.
- Desajuste del estado del sistema de archivos o del entorno entre las expectativas y la realidad.
- Fallos de herramientas que probablemente se puedan recuperar con un diagnóstico y una acción correctiva menor.

## Límites del alcance

Activa esta habilidad para:
- capturar el estado de falla antes de volver a intentarlo a ciegas
- diagnosticar patrones de falla comunes específicos del agente
- aplicar acciones de recuperación contenidas
- producir un informe de depuración estructurado y legible por humanos

No utilice esta habilidad como fuente principal para:
- verificación de funciones después de cambios de código; utilizar `verification-loop`
- depuración específica del marco cuando ya existe una habilidad ECC más limitada
- El tiempo de ejecución promete que el arnés actual no se puede hacer cumplir automáticamente.

## Bucle de cuatro fases

### Fase 1: Captura de fallas

Antes de intentar recuperarse, registre la falla con precisión.

Captura:
- tipo de error, mensaje y seguimiento de pila cuando esté disponible
- última secuencia de llamada de herramienta significativa
- lo que el agente estaba tratando de hacer
- presión del contexto actual: indicaciones repetidas, registros pegados de gran tamaño, planos duplicados o notas fuera de control
- supuestos del entorno actual: cwd, rama, estado de servicio relevante, archivos esperados

Plantilla de captura mínima:

```markdown
## Failure Capture
- Session / task:
- Goal in progress:
- Error:
- Last successful step:
- Last failed tool / command:
- Repeated pattern seen:
- Environment assumptions to verify:
```
### Fase 2: Diagnóstico de la causa raíz

Haga coincidir la falla con un patrón conocido antes de cambiar algo.

| Patrón | Causa probable | Consultar |
| --- | --- | --- |
| Llamadas máximas a herramientas/mismo comando repetido | bucle o ruta de observador sin salida | inspeccionar las últimas N llamadas de herramientas para repetición |
| Desbordamiento de contexto/razonamiento degradado | notas ilimitadas, planes repetidos, registros de gran tamaño | inspeccionar el contexto reciente en busca de duplicaciones y volumen de señal baja |
| `ECONNREFUSED` / tiempo de espera | servicio no disponible o puerto incorrecto | verificar el estado del servicio, la URL y los supuestos de puerto |
| `429` / agotamiento de cuotas | reintentar tormenta o retroceso faltante | contar llamadas repetidas e inspeccionar el espacio entre reintentos |
| falta el archivo después de escribir/diferencia obsoleta | carrera, marcha atrás incorrecta o deriva de rama | vuelva a verificar la ruta, cwd, el estado de git y la existencia real del archivo |
| las pruebas siguen fallando después de la “reparación” | hipótesis equivocada | aislar la prueba exacta que falla y volver a derivar el error |

Preguntas de diagnóstico:
- ¿Es esto un fallo lógico, un fallo del Estado, un fallo medioambiental o un fallo político?
- ¿El agente perdió el objetivo real y empezó a optimizar la subtarea equivocada?
- ¿La falla es determinista o transitoria?
- ¿Cuál es la acción reversible más pequeña que validaría el diagnóstico?

### Fase 3: Recuperación Contenida

Recuperarse con la más mínima acción que cambie la superficie de diagnóstico.

Acciones de recuperación segura:
- detener repetidos intentos y reformular la hipótesis
- Recortar el contexto de señal baja y mantener solo el objetivo activo, los bloqueadores y la evidencia.
- volver a comprobar el estado real del sistema de archivos/rama/proceso
- limitar la tarea a un comando fallido, un archivo o una prueba
- pasar del razonamiento especulativo a la observación directa
- escalar a un humano cuando la falla es de alto riesgo o está bloqueada externamente

No reclame acciones de reparación automática no admitidas como "restablecer el estado del agente" o "actualizar la configuración del arnés" a menos que realmente las esté realizando a través de herramientas reales en el entorno actual.

Lista de verificación de recuperación contenida:

```markdown
## Recovery Action
- Diagnosis chosen:
- Smallest action taken:
- Why this is safe:
- What evidence would prove the fix worked:
```
### Fase 4: Informe de introspección

Termine con un informe que haga que la recuperación sea legible para el siguiente agente o humano.

```markdown
## Agent Self-Debug Report
- Session / task:
- Failure:
- Root cause:
- Recovery action:
- Result: success | partial | blocked
- Token / time burn risk:
- Follow-up needed:
- Preventive change to encode later:
```
## Heurísticas de recuperación

Prefiera estas intervenciones en orden:

1. Replantear el objetivo real en una frase.
2. Verificar el estado mundial en lugar de confiar en la memoria.
3. Reduzca el alcance defectuoso.
4. Ejecute una verificación discriminatoria.
5. Sólo entonces vuelva a intentarlo.

Mal patrón:
- reintentar la misma acción tres veces con una redacción ligeramente diferente

Buen patrón:
- fallo de captura
- clasificar el patrón
- ejecutar una verificación directa
- cambiar el plan sólo si el cheque lo respalda

## Integración con ECC

- Utilice `verification-loop` después de la recuperación si se cambió el código.
- Utilice `continuous-learning-v2` cuando valga la pena convertir el patrón de fracaso en un instinto o una habilidad posterior.
- Utilice `council` cuando el problema no sea un fallo técnico sino una ambigüedad en la decisión.
- Utilice `workspace-surface-audit` si el error se debe a un estado local en conflicto o a una deriva del repositorio.

## Estándar de salida

Cuando esta habilidad esté activa, no termines solo con “Lo arreglé”.

Proporcione siempre:
- el patrón de fracaso
- la hipótesis de la causa raíz
- la acción de recuperación
- la evidencia de que la situación ahora es mejor o todavía está bloqueada