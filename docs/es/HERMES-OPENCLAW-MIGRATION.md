# Migración de Hermes / OpenClaw -> ECC

Este documento es la guía pública de migración para trasladar una configuración de operador estilo Hermes u OpenClaw al modelo actual de ECC.

El objetivo no es reproducir un espacio de trabajo privado de operador byte por byte.

El objetivo es preservar la superficie de trabajo útil:

- skills reutilizables
- puntos de entrada de automatización estables
- portabilidad entre harnesses
- programadores / recordatorios / despacho
- contexto duradero y memoria del operador

mientras se eliminan las partes que deben permanecer privadas:

- secretos
- conjuntos de datos personales
- tokens de cuenta
- artefactos empresariales locales

## Tesis de migración

Trata Hermes y OpenClaw como sistemas de origen, no como el runtime final.

ECC es el sistema público duradero:

- skills
- agents
- commands
- hooks
- superficies de instalación
- adaptadores de sesión
- trabajo de plano de control de ECC 2.0

Hermes y OpenClaw son entradas útiles porque contienen flujos de trabajo de operador repetidos que pueden destilarse en superficies nativas de ECC.

Eso significa que la ruta segura más corta es:

1. extraer el comportamiento reutilizable
2. traducirlo en skills, hooks, docs o trabajo de adaptador nativos de ECC
3. mantener secretos y datos personales fuera del repositorio

## Modelo actual del workspace

Usa de forma consistente la división actual del workspace:

- el trabajo de código en vivo ocurre en repos clonados bajo `~/GitHub`
- el contexto activo específico del repositorio vive en `WORKING-CONTEXT.md` a nivel del repo
- el contexto más amplio no relacionado con código puede vivir en capas KB/archive
- la verdad duradera entre máquinas debe preferir GitHub, Linear y la base de conocimiento

No reconstruyas un workspace privado en sombra dentro del repositorio público.

## Mapa de traducción

### 1. Capa de programador / cron

Ejemplos de origen:

- `cron/scheduler.py`
- `jobs.py`
- bucles recurrentes de preparación o responsabilidad

Traducir en:

- programación nativa de Claude cuando esté disponible
- automatización de hooks / commands de ECC para repetibilidad local
- trabajo de programador de ECC 2.0 bajo la issue `#1050`

Hoy, el repositorio ya tiene el marco público correcto:

- hooks para automatización local de baja latencia en el repo
- commands para acciones explícitas del operador
- ECC 2.0 como el futuro plano de control / programación de larga vida

### 2. Capa de gateway / despacho

Ejemplos de origen:

- gateway de Hermes
- despacho móvil / avisos remotos
- enrutamiento del operador entre sesiones activas

Traducir en:

- trabajo de adaptador de sesión y plano de control de ECC
- commands de orquestación / inspección de sesiones
- backlog del plano de control de ECC 2.0 bajo:
  - `#1045`
  - `#1046`
  - `#1047`
  - `#1048`

El repositorio público debe describir la frontera del adaptador y el modelo de plano de control, no fingir que el shell remoto del operador ya está completamente GA.

### 3. Capa de memoria

Ejemplos de origen:

- `memory_tool.py`
- memoria local del operador
- almacenes de contexto empresarial / operativo

Traducir en:

- `knowledge-ops`
- `WORKING-CONTEXT.md` del repo
- contexto duradero respaldado por GitHub / Linear / KB
- trabajo futuro de memoria profunda bajo `#1049`

La distinción importante es:

- el contexto de ejecución del repo pertenece cerca del repo
- la memoria más amplia no relacionada con código pertenece en sistemas KB/archive
- el repositorio público debe documentar la frontera, no almacenar volcados de memoria privados

### 4. Capa de skills

Ejemplos de origen:

- skills de Hermes
- skills de OpenClaw
- playbooks de operador generados

Traducir en:

- skills nativos de ECC de nivel superior cuando el flujo de trabajo sea reutilizable
- docs/examples cuando el contenido sea solo una plantilla
- hooks o commands cuando el comportamiento sea procedimental en lugar de conocimiento estructurado

Ejemplos recientes ya recuperados de esta manera:

- `knowledge-ops`
- `github-ops`
- `hookify-rules`
- `automation-audit-ops`
- `email-ops`
- `finance-billing-ops`
- `messages-ops`
- `research-ops`
- `terminal-ops`
- `ecc-tools-cost-audit`

### 5. Capa de herramientas / servicios

Ejemplos de origen:

- envoltorios de servicio personalizados
- herramientas locales respaldadas por API keys
- pegamento de automatización de navegador

Traducir en:

- superficies respaldadas por MCP cuando exista un conector
- skills de operador nativos de ECC cuando la lógica del flujo de trabajo sea el activo real
- trabajo de adaptador / plano de control cuando la pieza que falta sea la coordinación de sesión/runtime

No importes runtimes opacos de terceros a ECC solo porque un flujo de trabajo privado dependía de ellos.

Si un flujo de trabajo es valioso:

1. entiende el comportamiento
2. reconstruye la versión mínima nativa de ECC
3. documenta localmente la autenticación / los conectores requeridos

## Lo que ya existe públicamente

El repositorio actual ya cubre partes significativas de la migración:

- documentos de descubrimiento de adaptador / plano de control de ECC 2.0
- sustrato de orquestación / inspección de sesiones
- skills de flujos de trabajo del operador
- skills de auditoría de costos / facturación / flujos de trabajo
- superficies de instalación entre harnesses
- AgentShield para configuración y escaneo de superficies de agentes

Esto significa que el problema de migración ya no es "empezar desde cero".

Más bien es:

- destilar flujos de trabajo privados faltantes
- aclarar la documentación pública
- continuar la construcción del plano de control / operador de ECC 2.0

ECC 2.0 ahora incluye un punto de entrada acotado para auditoría de migración:

- `ecc migrate audit --source ~/.hermes`
- `ecc migrate plan --source ~/.hermes --output migration-plan.md`
- `ecc migrate scaffold --source ~/.hermes --output-dir migration-artifacts`
- `ecc migrate import-skills --source ~/.hermes --output-dir migration-artifacts/skills`
- `ecc migrate import-tools --source ~/.hermes --output-dir migration-artifacts/tools`
- `ecc migrate import-plugins --source ~/.hermes --output-dir migration-artifacts/plugins`
- `ecc migrate import-schedules --source ~/.hermes --dry-run`
- `ecc migrate import-remote --source ~/.hermes --dry-run`
- `ecc migrate import-env --source ~/.hermes --dry-run`
- `ecc migrate import-memory --source ~/.hermes`

Úsalo primero para inventariar el workspace heredado y mapear las superficies detectadas hacia el programador, el despacho remoto, el grafo de memoria, las plantillas y las rutas de traducción manual de ECC2.

## Lo que todavía pertenece al backlog

Los temas grandes restantes de migración ya están registrados:

- `#1051` migración de Hermes/OpenClaw
- `#1049` capa de memoria profunda
- `#1050` programación autónoma
- `#1048` capa universal de compatibilidad entre harnesses
- `#1046` orquestador de agentes
- `#1045` gestor TUI multisesión
- `#1047` gestor visual de worktrees

Ese es el lugar correcto para el trabajo pendiente del plano de control.

No finjas que la migración está "terminada" solo porque existen documentos públicos.

## Orden recomendado de puesta en marcha

1. Mantén el repositorio público de ECC como la capa reutilizable canónica.
2. Porta los flujos de trabajo reutilizables de Hermes/OpenClaw a skills nativos de ECC una línea a la vez.
3. Mantén la autenticación privada y el contexto personal fuera del repositorio.
4. Usa los sistemas GitHub / Linear / KB como verdad duradera.
5. Trata ECC 2.0 como el camino hacia un shell nativo de operador, no como un producto terminado.

## Regla de decisión

Al revisar un artefacto de Hermes o OpenClaw, pregunta:

1. ¿Es reutilizable entre operadores o solo personal?
2. ¿El activo es principalmente conocimiento, procedimiento o comportamiento de runtime?
3. ¿Debe convertirse en:
   - un skill
   - un command
   - un hook
   - un doc/example
   - una issue de plano de control
4. ¿Publicarlo filtraría secretos, conjuntos de datos privados o estado operativo personal?

Publica solo la superficie reutilizable.
