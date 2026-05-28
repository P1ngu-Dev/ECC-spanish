---
name: hipaa-compliance
description: HIPAA-specific entrypoint for healthcare privacy and security work. Use when a task is explicitly framed around HIPAA, PHI handling, covered entities, BAAs, breach posture, or US healthcare compliance requirements.
origin: ECC direct-port adaptation
version: "1.0.0"
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Cumplimiento HIPAA

Usa esto como punto de entrada específico de HIPAA cuando una tarea trate claramente sobre cumplimiento sanitario de EE. UU. Este skill se mantiene intencionalmente delgado y canónico:

- `healthcare-phi-compliance` sigue siendo el skill principal de implementación para manejo de PHI/PII, clasificación de datos, registro de auditoría, cifrado y prevención de fugas.
- `healthcare-reviewer` sigue siendo el revisor especializado cuando el código, la arquitectura o el comportamiento del producto necesitan una segunda pasada con criterio sanitario.
- `security-review` sigue aplicando para auth general, manejo de input, secretos, APIs y hardening de despliegue.

## Cuándo usarlo

- La solicitud menciona explícitamente HIPAA, PHI, covered entities, business associates o BAAs
- Construyes o revisas software sanitario de EE. UU. que almacena, procesa, exporta o transmite PHI
- Evalúas si logs, analytics, prompts de LLM, almacenamiento o flujos de soporte crean exposición HIPAA
- Diseñas sistemas para pacientes o clínicos donde importan el mínimo necesario y la auditabilidad

## Cómo funciona

Trata HIPAA como una capa sobre el skill más amplio de privacidad sanitaria:

1. Empieza con `healthcare-phi-compliance` para las reglas concretas de implementación.
2. Aplica gates de decisión específicos de HIPAA:
   - ¿Estos datos son PHI?
   - ¿Este actor es una covered entity o un business associate?
   - ¿Un proveedor o modelo necesita un BAA antes de tocar los datos?
   - ¿El acceso está limitado al mínimo necesario?
   - ¿Los eventos de lectura/escritura/exportación son auditables?
3. Escala a `healthcare-reviewer` si la tarea afecta seguridad del paciente, flujos clínicos o arquitectura regulada en producción.

## Guardrails específicos de HIPAA

- Nunca pongas PHI en logs, eventos de analytics, crash reports, prompts ni strings de error visibles para el cliente.
- Nunca expongas PHI en URLs, almacenamiento del navegador, capturas de pantalla o payloads de ejemplo copiados.
- Exige acceso autenticado, autorización acotada y trazas de auditoría para lecturas y escrituras de PHI.
- Trata SaaS de terceros, observabilidad, tooling de soporte y proveedores de LLM como bloqueados por defecto hasta que el estado BAA y los límites de datos estén claros.
- Aplica el mínimo necesario: el usuario correcto solo debe ver el fragmento más pequeño de PHI necesario para la tarea.
- Prefiere IDs internos opacos sobre nombres, MRN, teléfonos, direcciones u otros identificadores.

## Ejemplos

### Ejemplo 1: solicitud de producto planteada como HIPAA

Solicitud del usuario:

> Add AI-generated visit summaries to our clinician dashboard. We serve US clinics and need to stay HIPAA compliant.

Patrón de respuesta:

- Activa `hipaa-compliance`
- Usa `healthcare-phi-compliance` para revisar movimiento de PHI, logs, almacenamiento y límites de prompts
- Verifica si el proveedor de resúmenes está cubierto por un BAA antes de enviar PHI
- Escala a `healthcare-reviewer` si los resúmenes influyen en decisiones clínicas

### Ejemplo 2: decisión de proveedor/tooling

Solicitud del usuario:

> Can we send support transcripts and patient messages into our analytics stack?

Patrón de respuesta:

- Asume que esos mensajes pueden contener PHI
- Bloquea el diseño salvo que el proveedor de analytics esté aprobado para cargas sujetas a HIPAA y la ruta de datos esté minimizada
- Exige redacción o un modelo de eventos sin PHI cuando sea posible

## Skills relacionadas

- `healthcare-phi-compliance`
- `healthcare-reviewer`
- `healthcare-emr-patterns`
- `healthcare-eval-harness`
- `security-review`
