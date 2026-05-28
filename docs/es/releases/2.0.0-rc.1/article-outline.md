# Esquema del artículo - ECC v2.0.0-rc.1

## Título de trabajo

Convertir ECC en un sistema operativo multiplataforma

## Argumento central

La mayor parte del trabajo agentivo se descompone porque las herramientas permanecen aisladas.

La ventaja surge de tratar el harness, la capa de flujo de trabajo reutilizable y el shell del operador como un solo sistema:

- skills para trabajo repetible
- hooks y tests para enforcement
- MCPs para acceso a herramientas
- memoria y handoffs para continuidad
- un shell de operador que pueda enrutar la ejecución diaria

## Estructura

### 1. El problema

- demasiadas ventanas de chat
- demasiados flujos de trabajo específicos de cada herramienta
- demasiado contexto viviendo en hábito personal en lugar de una forma de sistema reutilizable

### 2. Lo que ECC ya resolvió

- formato de skill reutilizable
- superficies de instalación multiplataforma
- disciplina de hooks y verificación
- patrones de seguridad y revisión
- skills de flujo de trabajo del operador alrededor de contenido, investigación y operaciones de negocio
- comprobaciones de cola, discusión, Linear, legado y evidencia de lanzamiento que hacen que el estado operativo sea inspeccionable
- escaneo de IOC de la cadena de suministro y endurecimiento de instalación sin ciclo de vida después de la campaña Mini Shai-Hulud/TanStack

### 3. Por qué Hermes es la capa del operador

- chat, CLI, TUI, cron y handoffs pueden situarse encima de la capa reutilizable de ECC
- el trabajo de negocio y contenido puede ejecutarse junto con el trabajo de ingeniería
- el ciclo diario se vuelve más fácil de inspeccionar y mejorar

### 4. Qué se publica en rc.1

- guía de configuración saneada de Hermes
- material de lanzamiento y distribución
- documento de arquitectura multiplataforma
- guía de importación de Hermes
- posicionamiento más claro de 2.0 en el repositorio
- puerta de verificación smoke del preview-pack
- borradores de lanzamiento para copia de GitHub release, X, LinkedIn, artículo, handoff de Telegram/Hermes y prompts de demostración

### 5. Qué cambió desde v1.10.0

- Claude Code sigue siendo el objetivo principal, pero ECC ahora trata Codex, OpenCode, Cursor, Gemini, Zed y los flujos de trabajo solo-terminal como superficies de ejecución compartidas.
- El proceso de lanzamiento ahora tiene comprobaciones repetibles de plataforma, discusión, observabilidad, cadena de suministro, progreso en Linear y preview-pack.
- El trabajo de AgentShield y ECC Tools se refleja en la hoja de ruta para que las líneas de seguridad empresarial, revisión hospedada, promoción de políticas y preparación para facturación no se desvíen del lanzamiento principal.

### 6. Qué se mantiene local

- secretos y auth
- exportaciones sin procesar del workspace
- datasets personales
- automatizaciones específicas del operador que aún no han sido saneadas
- playbooks más profundos de CRM, finanzas y Google Workspace

### 7. Punto final

El objetivo no es copiar una pila exacta.

El objetivo es construir un sistema operativo alrededor del agente que convierta el trabajo repetido en superficies reutilizables y medibles.
