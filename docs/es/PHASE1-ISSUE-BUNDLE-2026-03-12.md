# Paquete de incidencias de la Fase 1 — 12 de marzo de 2026

## Estado

Estos borradores de incidencias se prepararon a partir del mega plan del 11 de marzo más la entrega del 12 de marzo. Intenté abrirlos directamente en GitHub, pero la creación de incidencias se bloqueó por falta de autenticación de GitHub en la sesión MCP.

## Estado de GitHub

Estos borradores se publicaron después mediante `gh`:

- `#423` Implementar perfiles de instalación selectiva impulsados por manifiesto para ECC
- `#421` Agregar estado de instalación de ECC más el ciclo de vida uninstall / doctor / repair
- `#424` Definir el contrato canónico del adaptador de sesión para el plano de control de ECC 2.0
- `#422` Definir la ubicación y la política de procedencia de skills generadas
- `#425` Definir la gobernanza y la visibilidad más allá de la llamada a la herramienta

Los cuerpos siguientes se conservan como el paquete fuente local usado para crear las incidencias.

## Incidencia 1

### Título

Implementar perfiles de instalación selectiva impulsados por manifiesto para ECC

### Etiquetas

- `enhancement`

### Cuerpo

```md
## Problem

ECC todavía se instala principalmente por destino e idioma. El repositorio ya cuenta con manifiestos de instalación de primera pasada y un resolvedor de plan no mutante, pero el instalador en sí todavía no consume esos perfiles.

La base ya incorporada en el repositorio es:

- `manifests/install-modules.json`
- `manifests/install-profiles.json`
- `scripts/ci/validate-install-manifests.js`
- `scripts/lib/install-manifests.js`
- `scripts/install-plan.js`

Eso significa que el siguiente paso pendiente ya no es descubrir el diseño. El paso pendiente es la ejecución: integrar la resolución de perfiles/módulos en el flujo real de instalación sin romper la compatibilidad hacia atrás.

## Scope

Implementar la ejecución de instalación impulsada por manifiesto para los destinos actuales de ECC:

- `claude`
- `cursor`
- `antigravity`

Agregar soporte inicial para:

- `ecc-install --profile <name>`
- `ecc-install --modules <id,id,...>`
- filtrado según compatibilidad del módulo con el destino
- modo heredado de instalación por idioma compatible hacia atrás durante la transición

## Non-Goals

- Ciclo de vida completo uninstall/doctor/repair en la misma incidencia
- Destinos de instalación Codex/OpenCode en la primera pasada si eso bloquea el despliegue
- Reorganizar el repositorio en paquetes publicados separados

## Acceptance Criteria

- `install.sh` puede resolver e instalar un perfil con nombre
- `install.sh` puede resolver IDs de módulos explícitos
- Los módulos no compatibles con un destino se omiten o se rechazan de forma determinista
- El modo de instalación heredado basado en idioma sigue funcionando
- Las pruebas cubren la resolución de perfiles y el comportamiento del instalador
- La documentación explica la nueva ruta preferida de instalación por perfil/módulo
```

## Incidencia 2

### Título

Agregar estado de instalación de ECC más el ciclo de vida uninstall / doctor / repair

### Etiquetas

- `enhancement`

### Cuerpo

```md
## Problem

ECC no tiene un registro canónico del estado instalado. Eso hace que uninstall, repair e la inspección posterior a la instalación sean no deterministas.

Hoy el repositorio puede clasificar contenido instalable, pero todavía no puede responder de forma confiable:

- qué perfil/módulos se instalaron
- en qué destino se instalaron
- qué rutas posee ECC
- cómo eliminar o reparar solo los archivos administrados por ECC

Sin install-state, los comandos de ciclo de vida son una adivinanza.

## Scope

Introducir un contrato durable de install-state y los primeros comandos de ciclo de vida:

- `ecc list-installed`
- `ecc uninstall`
- `ecc doctor`
- `ecc repair`

Ubicaciones sugeridas para el estado:

- Claude: `~/.claude/ecc/install-state.json`
- Cursor: `./.cursor/ecc-install-state.json`
- Antigravity: `./.agent/ecc-install-state.json`

El archivo de estado debe registrar como mínimo:

- versión instalada
- marca de tiempo
- destino
- perfil
- módulos resueltos
- rutas copiadas/administradas
- versión del repositorio fuente o versión del paquete

## Non-Goals

- Reconstruir la arquitectura del instalador desde cero
- Funcionalidad completa de plano de control remoto/en la nube
- Expansión de soporte de destino más allá de los instaladores locales actuales, salvo que ocurra de forma natural

## Acceptance Criteria

- Las instalaciones exitosas escriben install-state de forma determinista
- `list-installed` informa claramente destino/perfil/módulos/versión
- `doctor` informa rutas administradas faltantes o desviadas
- `repair` restaura archivos administrados faltantes a partir del install-state registrado
- `uninstall` elimina solo los archivos administrados por ECC y deja intactos los archivos locales no relacionados
- Las pruebas cubren la creación de install-state y el comportamiento del ciclo de vida
```

## Incidencia 3

### Título

Definir el contrato canónico del adaptador de sesión para el plano de control de ECC 2.0

### Etiquetas

- `enhancement`

### Cuerpo

```md
## Problem

ECC ahora tiene un sustrato real de orquestación/sesiones, pero todavía es específico de la implementación.

Estado actual:

- existe orquestación con tmux/worktree
- existen snapshots de sesión legibles por máquina
- existen comandos locales de historial de sesión de Claude

Lo que aún no existe es una frontera de adaptador neutral al harness que pueda normalizar el estado de sesión/tarea entre:

- workers orquestados por tmux
- sesiones simples de Claude
- worktrees de Codex
- sesiones de OpenCode
- futuras superficies de operador remotas o integradas con GitHub

Sin ese contrato de adaptador, cualquier futuro shell de operador de ECC 2.0 tendrá que leer directamente detalles específicos de tmux y de coordinación en Markdown.

## Scope

Definir e implementar la primera pasada de la capa canónica de adaptador de sesión.

Entregables sugeridos:

- registro de adaptadores
- esquema canónico de snapshot de sesión
- adaptador `dmux-tmux` respaldado por el código de orquestación actual
- adaptador `claude-history` respaldado por las utilidades actuales de historial de sesión
- CLI de inspección de solo lectura para snapshots canónicos de sesión

## Non-Goals

- UI completa de ECC 2.0 en la misma incidencia
- implementación de monetización/GitHub App
- plano de control remoto multiusuario

## Acceptance Criteria

- Existe un contrato documentado de snapshot canónico
- El código actual de snapshot de orquestación tmux se envuelve como un adaptador en lugar del contrato superior del producto
- Existe un segundo adaptador que no sea tmux para demostrar que la abstracción es real
- Las pruebas cubren la selección de adaptadores y la salida normalizada del snapshot
- El diseño separa claramente las responsabilidades del adaptador de las de orquestación y UI
```

## Incidencia 4

### Título

Definir la ubicación y la política de procedencia de skills generadas

### Etiquetas

- `enhancement`

### Cuerpo

```md
## Problem

ECC ahora tiene una superficie de skills grande y en crecimiento, pero las skills generadas/importadas/aprendidas todavía no tienen una política clara de ubicación y procedencia a largo plazo.

Esto crea varios problemas:

- separación poco clara entre skills curadas y skills generadas/aprendidas
- ruido en el validador respecto de directorios que pueden o no existir localmente
- procedencia débil para contenido de skills importado o generado por máquina
- incertidumbre sobre dónde deben vivir las futuras salidas de aprendizaje automático

A medida que ECC crece, el repositorio necesita reglas explícitas sobre dónde pertenecen los artefactos de skills generados y cómo se identifican.

## Scope

Definir una política para todo el repositorio sobre:

- ubicación de skills curadas vs generadas vs importadas
- requisitos de metadatos de procedencia
- comportamiento del validador para directorios de skills opcionales/generados
- si las skills generadas se distribuyen, se ignoran o se materializan durante los pasos de instalación/compilación

## Non-Goals

- Construir un mercado externo completo de skills
- Reescribir todo el contenido de skills existente en una sola pasada
- Resolver todos los problemas de calidad de contenido en la misma incidencia

## Acceptance Criteria

- Existe una política documentada de ubicación para skills generadas/importadas
- Los requisitos de procedencia son explícitos
- Los validadores ya no producen un comportamiento ambiguo alrededor de ubicaciones opcionales/generadas de skills
- La política deja claro qué es publicable frente a qué es solo local
- El trabajo de implementación posterior se divide en pasos concretos, acotados y del tamaño de una PR
```
