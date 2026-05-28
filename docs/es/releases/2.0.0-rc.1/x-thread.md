# Borrador de hilo de X - ECC v2.0.0-rc.1

1/ ECC v2.0.0-rc.1 es el primer pase de release candidate para la dirección 2.0.

El repo está pasando de un paquete de configuración de Claude Code a un meta-harness para trabajo agéntico.

2/ La separación importante:

ECC es el sustrato reutilizable.
Hermes es la shell de operador que puede ejecutarse encima.

Skills, hooks, configuraciones MCP, rules y paquetes de workflow viven en ECC.

3/ Un meta-harness importa porque la capa de agentes se está fragmentando.

Claude Code, Codex, OpenCode, Cursor, Gemini, Zed, Copilot y los flujos de terminal
necesitan primitivas operativas similares:

- contexto
- herramientas
- memoria
- gates
- evaluación
- evidencia de release
- comprobaciones de seguridad

4/ ECC da a esas primitivas una forma compartida en lugar de dejar que cada flujo
se quede atascado dentro de un solo cliente.

Usa el harness que prefieras. Mantén portátil la capa de workflow.

5/ Desde v1.10.0, el trabajo también incorporó la capa de operador:

auditorías de PR/issue/discussion, sincronización de progreso de Linear, evidencia de release, comprobaciones de observabilidad y un dashboard de readiness generado.

6/ La postura de seguridad también cambió.

La campaña Mini Shai-Hulud/TanStack forzó un bucle real de supply-chain:

- escaneo IOC
- instalaciones CI sin lifecycle
- actualización de fuentes de advisories
- comprobaciones de npm audit/signature
- objetivos de persistencia de herramientas de IA

7/ La superficie rc.1 publica las piezas públicas:

- guía de configuración de Hermes
- notas de la versión
- checklist de lanzamiento
- documento de arquitectura entre harnesses
- guía de importación de Hermes
- gate de smoke del preview pack
- borradores de X, LinkedIn y artículo

8/ También añade la superficie pública teaser para el skill pack de prediction-market de Itô.

Eso es independiente de la facturación de ECC Tools e Itô sigue siendo un negocio separado.

Las skills públicas son investigación, comparación, planificación y revisión de riesgos.

9/ Límite importante:

Sin asesoramiento de inversión.
Sin trading en vivo por defecto.
Sin claves privadas.
Sin llamada respaldada por Itô sin acceso API explícitamente protegido.

Primero una forma de workflow útil, después acceso a datos protegido.

10/ No publica el estado privado del workspace.

No secretos.
No tokens OAuth.
No exportaciones locales en bruto.
No datasets personales.

El objetivo es publicar la forma reutilizable del sistema.

11/ Por qué importa Hermes:

La mayoría de los sistemas de agentes fallan en el bucle operativo diario.

Pueden programar, pero no mantienen investigación, contenido, handoffs, recordatorios y ejecución en una sola superficie medible.

12/ ECC da la capa reutilizable.

Hermes da la shell de operador.

Juntos hacen que el trabajo se sienta menos como ventanas de chat dispersas y más como un sistema que puedes ejecutar.

13/ Esto sigue siendo una release candidate.

Los docs públicos y las superficies reutilizables están listos para revisión.

Las integraciones locales más profundas permanecen locales hasta que se saneen, y la publicación sigue esperando los gates finales de GitHub release, npm, plugin y URL.

14/ Empieza aquí:

Repo:
<https://github.com/affaan-m/ECC>

Configuración Hermes x ECC:
<https://github.com/affaan-m/ECC/blob/main/docs/HERMES-SETUP.md>

15/ Notas de la versión:
<https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/release-notes.md>

Boundary del skill pack de Itô:
<https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/ito-prediction-market-skill-pack.md>

Ledger de URLs:
<https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/release-url-ledger-2026-05-19.md>
