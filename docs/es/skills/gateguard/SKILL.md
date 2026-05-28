---
name: gateguard
description: Fact-forcing gate that blocks Edit/Write/Bash (including MultiEdit) and demands concrete investigation (importers, data schemas, user instruction) before allowing the action. Measurably improves output quality by +2.25 points vs ungated agents.
origin: community
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# GateGuard — Puerta previa a la acción que fuerza hechos

Un hook PreToolUse que obliga a Claude a investigar antes de editar. En vez de autoevaluación (“¿estás seguro?”), exige hechos concretos. El acto de investigar crea una conciencia que la autoevaluación nunca genera.

## Cuándo usarlo

- Trabajas en cualquier codebase donde las ediciones de archivo afectan a varios módulos
- Proyectos con archivos de datos que tienen esquemas o formatos de fecha específicos
- Equipos donde el código generado por IA debe coincidir con patrones existentes
- Cualquier flujo donde Claude tienda a adivinar en vez de investigar

## Concepto central

La autoevaluación de un LLM no funciona. Pregunta “¿violaste alguna política?” y la respuesta siempre es “no”. Esto está verificado experimentalmente.

Pero preguntar “enumera todos los archivos que importan este módulo” obliga al LLM a ejecutar Grep y Read. La investigación en sí crea contexto y cambia el resultado.

**Puerta de tres etapas:**

```
1. DENY  — bloquea el primer intento de Edit/Write/Bash
2. FORCE — indica al modelo exactamente qué hechos reunir
3. ALLOW — permite reintentar después de presentar los hechos
```

Nadie más hace las tres cosas. La mayoría se queda en deny.

## Evidencia

Dos pruebas A/B independientes, agentes idénticos, misma tarea:

| Tarea | Con puerta | Sin puerta | Diferencia |
| --- | --- | --- | --- |
| Módulo de analítica | 8.0/10 | 6.5/10 | +1.5 |
| Validador de webhooks | 10.0/10 | 7.0/10 | +3.0 |
| **Promedio** | **9.0** | **6.75** | **+2.25** |

Ambos agentes producen código que funciona y pasa las pruebas. La diferencia está en la profundidad del diseño.

## Tipos de puerta

### Puerta Edit / MultiEdit (primer cambio por archivo)

MultiEdit se maneja igual — cada archivo del lote se protege individualmente.

```
Antes de editar {file_path}, presenta estos hechos:

1. Enumera TODOS los archivos que importan/requieren este archivo (usa Grep)
2. Enumera las funciones/clases públicas afectadas por este cambio
3. Si este archivo lee/escribe archivos de datos, muestra nombres de campos, estructura
   y formato de fecha (usa valores anonimizados o sintéticos, no datos reales)
4. Cita textualmente la instrucción actual del usuario
```

### Puerta Write (primera creación de archivo nuevo)

```
Antes de crear {file_path}, presenta estos hechos:

1. Nombra el/los archivo(s) y línea(s) que llamarán a este nuevo archivo
2. Confirma que ningún archivo existente cumple el mismo propósito (usa Glob)
3. Si este archivo lee/escribe archivos de datos, muestra nombres de campos, estructura
   y formato de fecha (usa valores anonimizados o sintéticos, no datos reales)
4. Cita textualmente la instrucción actual del usuario
```

### Puerta Bash destructiva (cada comando destructivo)

Se activa en: `rm -rf`, `git reset --hard`, `git push --force`, `drop table`, etc.

```
1. Enumera todos los archivos/datos que este comando modificará o eliminará
2. Escribe un procedimiento de rollback de una línea
3. Cita textualmente la instrucción actual del usuario
```

### Puerta Bash rutinaria (una vez por sesión)

```
1. La petición actual del usuario en una frase
2. Qué verifica o produce este comando específico
```

## Inicio rápido

### Opción A: usar el hook ECC (sin instalación)

El hook en `scripts/hooks/gateguard-fact-force.js` viene incluido en este plugin. Actívalo mediante hooks.json.

Si GateGuard bloquea tareas de configuración o reparación, inicia la sesión con
`ECC_GATEGUARD=off`. Para control a nivel de hook, sigue usando
`ECC_DISABLED_HOOKS` con el ID del hook de GateGuard.

### Opción B: paquete completo con configuración

```bash
pip install gateguard-ai
gateguard init
```

Esto añade `.gateguard.yml` para configuración por proyecto (mensajes personalizados, rutas ignoradas, alternadores de puertas).

## Antipatrónes

- **No uses autoevaluación en su lugar.** “¿Estás seguro?” siempre recibe “sí”. Esto está verificado experimentalmente.
- **No omitas la comprobación del esquema de datos.** Ambos agentes de la prueba A/B asumieron fechas ISO-8601 cuando los datos reales usaban `%Y/%m/%d %H:%M`. Revisar la estructura de datos (con valores anonimizados) evita toda esta clase de errores.
- **No protejas cada comando Bash.** Las puertas rutinarias de Bash se ejecutan una vez por sesión. Las destructivas, siempre. Este equilibrio evita lentitud sin perder riesgos reales.

## Buenas prácticas

- Deja que la puerta se dispare de forma natural. No intentes responder de antemano: la investigación es lo que mejora la calidad.
- Personaliza los mensajes para tu dominio. Si tu proyecto tiene convenciones específicas, agrégalas a los prompts de la puerta.
- Usa `.gateguard.yml` para ignorar rutas como `.venv/`, `node_modules/`, `.git/`.

## Skills relacionadas

- `safety-guard` — comprobaciones de seguridad en tiempo de ejecución (complementario, no solapado)
- `code-reviewer` — revisión posterior a la edición (GateGuard investiga antes de editar)
