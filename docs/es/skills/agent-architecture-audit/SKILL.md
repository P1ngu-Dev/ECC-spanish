---
name: agent-architecture-audit
description: Full-stack diagnostic for agent and LLM applications. Audits the 12-layer agent stack for wrapper regression, memory pollution, tool discipline failures, hidden repair loops, and rendering corruption. Produces severity-ranked findings with code-first fixes. Essential for developers building agent applications, autonomous loops, or any LLM-powered feature.
origin: oh-my-agent-check
tools: Read, Write, Edit, Bash, Grep, Glob
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->
# Auditoría de arquitectura del agente

Un flujo de trabajo de diagnóstico para sistemas de agentes que ocultan fallas detrás de capas envolventes, memoria obsoleta, bucles de reintento o mutaciones de transporte/renderización.

## Cuándo activar

**OBLIGATORIO para:**
- Lanzar cualquier agente o aplicación impulsada por LLM a producción.
- Funciones de envío con llamada de herramientas, memoria o flujos de trabajo de varios pasos
- El comportamiento del agente se degrada después de agregar capas envolventes.
- El usuario informa "el agente está empeorando" o "las herramientas no funcionan correctamente"
- El mismo modelo funciona en el patio de recreo pero se rompe dentro de su envoltorio.
- Depuración del comportamiento del agente durante más de 15 minutos sin encontrar la causa raíz

**Especialmente crítico cuando:**
- Ha agregado nuevas capas de mensajes, definiciones de herramientas o sistemas de memoria.
- Diferentes agentes en su sistema se comportan de manera inconsistente
- La modelo estaba bien ayer pero hoy está alucinando.
- Sospechas que los bucles ocultos de reparación/reintento mutan silenciosamente las respuestas.

**No utilizar para:**
- Depuración de código general: use `agent-introspection-debugging`
- Revisión de código: utilice agentes revisores específicos del idioma
- Escaneo de seguridad: use `security-review` o `security-review/scan`
- Evaluación comparativa del rendimiento del agente: use `agent-eval`
- Redacción de nuevas funciones: utilice la habilidad de flujo de trabajo adecuada

## La pila de 12 capas

Todo sistema de agentes tiene estas capas. Cualquiera de ellos puede corromper la respuesta:

| # | Capa | ¿Qué sale mal?
|---|-------|----------------|
| 1 | Aviso del sistema | Instrucciones contradictorias, instrucciones hinchadas |
| 2 | Historial de sesiones | Inyección de contexto obsoleto de turnos anteriores |
| 3 | Memoria a largo plazo | Contaminación en las sesiones, viejos temas en nuevas conversaciones |
| 4 | Destilación | Artefactos comprimidos que vuelven a entrar como pseudohechos |
| 5 | Recuerdo activo | Capas de resumen redundantes que desperdician contexto |
| 6 | Selección de herramientas | Ruta de herramientas incorrecta, el modelo omite las herramientas necesarias |
| 7 | Ejecución de herramientas | Ejecución alucinada: dice llamar pero no lo hace |
| 8 | Interpretación de herramientas | Salida de herramienta mal interpretada o ignorada |
| 9 | Dar forma a la respuesta | Corrupción de formato en la respuesta final |
| 10 | Representación de plataforma | Mutación de la capa de transporte (UI, API, CLI mutan respuestas válidas) |
| 11 | Bucles de reparación ocultos | Agentes silenciosos de respaldo/reintento que ejecutan un segundo pase de LLM |
| 12 | Persistencia | Estado caducado o artefactos almacenados en caché reutilizados como evidencia viva |

## Patrones de falla comunes

### 1. Regresión envolvente

El modelo base produce respuestas correctas, pero las capas envolventes lo empeoran.

**Síntomas:**
- El modelo funciona bien en el patio de juegos o en una llamada API directa, se interrumpe su agente
- Se agregó una nueva capa de mensajes, el comportamiento existente se degradó.
- El agente parece confiado pero está totalmente equivocado.
- "Estaba funcionando antes de la última actualización"

### 2. Contaminación de la memoria

Los viejos temas se filtran en nuevas conversaciones a través de la historia, la recuperación de la memoria o la destilación.

**Síntomas:**
- El agente menciona temas pasados no relacionados.
- Las correcciones del usuario no se mantienen (la memoria antigua sobrescribe la nueva)
- Los artefactos de la misma sesión vuelven a ingresar como pseudohechos.
- La memoria crece sin límites, degradando la calidad de la respuesta con el tiempo.

### 3. Fallo en la disciplina de herramientas

Las herramientas se declaran en el mensaje pero no se aplican en el código. El modelo se los salta o alucina su ejecución.

**Síntomas:**
- "Debe usar la herramienta X" en el mensaje, pero el modelo responde sin llamarla
- Los resultados de la herramienta parecen correctos pero nunca se ejecutaron.
- Diferentes herramientas luchan por la misma responsabilidad.
- El modelo usa la herramienta cuando no debería, o la omite cuando debe hacerlo.

### 4. Representación/corrupción en el transporte

La respuesta interna del agente es correcta, pero la capa de plataforma la muta durante la entrega.

**Síntomas:**
- Los registros muestran la respuesta correcta, el usuario ve una salida rota
- La representación de Markdown, el análisis JSON o la transmisión de fragmentos corrompen respuestas válidas
- El agente de respaldo oculto reemplaza silenciosamente la respuesta antes de la entrega.
- La salida difiere entre terminal y UI

### 5. Capas de agentes ocultos

Los agentes silenciosos de reparación, reintento, resumen o recuperación se ejecutan sin contratos explícitos.

**Síntomas:**
- Cambios de salida entre la generación interna y la entrega al usuario.
- Los bucles de "reparación automática" ejecutan un segundo pase LLM que el usuario no conoce
- Múltiples agentes modifican la misma salida sin coordinación
- Las respuestas se "suavizan" o "corrigen" mediante capas invisibles.

## Flujo de trabajo de auditoría

### Fase 1: Alcance

Defina lo que está auditando:

- **Sistema de destino**: ¿qué aplicación de agente?
- **Puntos de entrada**: ¿cómo interactúan los usuarios con ellos?
- **Pila de modelos**: ¿qué LLM y proveedores?
- **Síntomas**: ¿qué informa el usuario?
- **Ventana de tiempo**: ¿cuándo empezó?
- **Capas para auditar**: ¿cuál de las 12 capas se aplica?

### Fase 2: Recopilación de pruebas

Reúna evidencia del código base:

- **Código fuente**: bucle de agente, enrutador de herramientas, admisión de memoria, ensamblaje rápido
- **Registros**: seguimientos históricos de sesiones, registros de llamadas de herramientas
- **Config**: plantillas de mensajes, esquemas de herramientas, configuración del proveedor
- **Archivos de memoria**: SOP, bases de conocimientos, archivos de sesiones

Utilice `rg` para buscar antipatrones:

```bash
# Tool requirements expressed only in prompt text (not code)
rg "must.*tool|必须.*工具|required.*call" --type md

# Tool execution without validation
rg "tool_call|toolCall|tool_use" --type py --type ts

# Hidden LLM calls outside main agent loop
rg "completion|chat\.create|messages\.create|llm\.invoke"

# Memory admission without user-correction priority
rg "memory.*admit|long.*term.*update|persist.*memory" --type py --type ts

# Fallback loops that run additional LLM calls
rg "fallback|retry.*llm|repair.*prompt|re-?prompt" --type py --type ts

# Silent output mutation
rg "mutate|rewrite.*response|transform.*output|shap" --type py --type ts
```
### Fase 3: Mapeo de fallas

Para cada hallazgo, documente:

- **Síntoma**: lo que ve el usuario
- **Mecanismo** — cómo lo provoca el envoltorio
- **Capa de origen**: cuál de las 12 capas
- **Causa raíz**: la causa más profunda
- **Evidencia** — archivo:línea o registro:referencia de fila
- **Confianza**: 0,0 a 1,0

### Fase 4: Estrategia de reparación

Orden de corrección predeterminado (primero el código, no el mensaje primero):

1. **Requisitos de la herramienta Code-gate**: aplicar en el código, no solo en el texto solicitado
2. **Eliminar o limitar los agentes de reparación ocultos**: haga explícito el recurso alternativo en los contratos
3. **Reducir la duplicación de contexto**: la misma información a través del mensaje + historial + memoria + destilación
4. **Reforzar la admisión de memoria**: correcciones del usuario > afirmaciones del agente
5. **Apriete los gatillos de destilación**: no comprima lo que no debería comprimirse
6. **Reducir la mutación de renderizado**: transferencia, no transformación
7. **Convertir a sobres JSON escritos**: flujo interno estructurado, no prosa de forma libre

## Modelo de gravedad

| Nivel | Significado | Acción |
|-------|---------|--------|
| `critical` | El agente puede producir con confianza un comportamiento operativo incorrecto | Arreglar antes del próximo lanzamiento |
| `high` | El agente frecuentemente degrada la corrección o la estabilidad | Arregla este sprint |
| `medium` | La corrección suele sobrevivir, pero el resultado es frágil o despilfarrador | Plan para el próximo ciclo |
| `low` | Principalmente problemas cosméticos o de mantenimiento | Trabajo pendiente |

## Formato de salida

Presente los hallazgos al usuario en este orden:

1. **Hallazgos clasificados por gravedad** (los más críticos primero)
2. **Diagnóstico de arquitectura** (qué capa corrompió qué y por qué)
3. **Plan de reparación ordenado** (primero el código, no el aviso primero)

No empieces con elogios o resúmenes. Si el sistema no funciona, dígalo directamente.

## Preguntas de diagnóstico rápido

Al auditar un sistema de agente, responda estas:

| # | Pregunta | En caso afirmativo → |
|---|----------|----------|
| 1 | ¿Puede el modelo omitir una herramienta requerida y aun así responder? | Herramienta no controlada por código |
| 2 | ¿El contenido de conversaciones antiguas aparece en turnos nuevos? | Contaminación de la memoria |
| 3 | ¿Está la misma información en el indicador del sistema, en la memoria y en el historial? | Duplicación de contexto |
| 4 | ¿La plataforma ejecuta un segundo pase LLM antes de la entrega? | Bucle de reparación oculto |
| 5 | ¿El resultado difiere entre la generación interna y la entrega al usuario? | Representando la corrupción |
| 6 | ¿Las reglas de "debe usar la herramienta X" están solo en el texto del mensaje? | Fallo en la disciplina de herramientas |
| 7 | ¿Puede el monólogo del propio agente convertirse en memoria persistente? | Envenenamiento de la memoria |

## Antipatrones a evitar

- Evite culpar al modelo antes de falsificar las regresiones de capa envolvente.
- Evitar culpar a la memoria sin mostrar el camino de la contaminación.
- No dejes que un estado actual limpio borre un incidente histórico sucio.
- No trate la prosa rebajada como un protocolo interno confiable.
- No acepte "herramienta de uso obligatorio" en el texto del mensaje cuando el código nunca lo aplica.
- Mantenga los hallazgos directos, respaldados por evidencia y clasificados por gravedad.

## Esquema de informe

Las auditorías deben producir informes estructurados siguiendo esta forma:

```json
{
  "schema_version": "ecc.agent-architecture-audit.report.v1",
  "executive_verdict": {
    "overall_health": "high_risk",
    "primary_failure_mode": "string",
    "most_urgent_fix": "string"
  },
  "scope": {
    "target_name": "string",
    "model_stack": ["string"],
    "layers_to_audit": ["string"]
  },
  "findings": [
    {
      "severity": "critical|high|medium|low",
      "title": "string",
      "mechanism": "string",
      "source_layer": "string",
      "root_cause": "string",
      "evidence_refs": ["file:line"],
      "confidence": 0.0,
      "recommended_fix": "string"
    }
  ],
  "ordered_fix_plan": [
    { "order": 1, "goal": "string", "why_now": "string", "expected_effect": "string" }
  ]
}
```
## Habilidades relacionadas

- `agent-introspection-debugging`: fallos en tiempo de ejecución del agente de depuración (bucles, tiempos de espera, errores de estado)
- `agent-eval` — Comparar el desempeño del agente cara a cara
- `security-review` — Auditoría de seguridad para código y configuración
- `autonomous-agent-harness` — Configurar operaciones de agente autónomo
- `agent-harness-construction` — Crear arneses de agentes desde cero