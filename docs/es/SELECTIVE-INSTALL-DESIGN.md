# Diseño de instalación selectiva de ECC

## Propósito

Este documento define el diseño de instalación selectiva orientado al usuario para ECC.

Complementa `docs/SELECTIVE-INSTALL-ARCHITECTURE.md`, que se centra en la arquitectura interna de tiempo de ejecución y en los límites del código.

Este documento responde primero a las preguntas de producto y operación:

- cómo eligen los usuarios los componentes de ECC
- cómo debería sentirse el CLI
- qué archivo de configuración debería existir
- cómo debería comportarse la instalación entre distintos destinos de harness
- cómo el diseño se mapea sobre la base de código actual de ECC sin exigir una reescritura

## Problema

Hoy ECC todavía se siente como un instalador de carga grande, aunque el repositorio ya cuenta con soporte inicial de manifiestos y ciclo de vida.

Los usuarios necesitan un modelo mental más simple:

- instalar la base
- agregar los paquetes de lenguaje que realmente usan
- agregar las configuraciones de framework que realmente quieren
- agregar paquetes opcionales de capacidades como seguridad, investigación u orquestación

El sistema de instalación selectiva debería hacer que ECC se sienta componible en lugar de todo o nada.

En la base actual, los componentes orientados al usuario siguen siendo una capa de alias sobre módulos internos más gruesos. Eso significa que incluir/excluir ya resulta útil a nivel de selección de módulos, pero algunos límites a nivel de archivo siguen siendo imperfectos hasta que el grafo de módulos subyacente se divida con mayor fineza.

## Objetivos

1. Permitir que los usuarios instalen rápidamente una huella base pequeña de ECC.
2. Permitir que los usuarios compongan instalaciones a partir de familias reutilizables de componentes:
   - reglas centrales
   - paquetes de lenguaje
   - paquetes de framework
   - paquetes de capacidades
   - configuraciones de destino/plataforma
3. Mantener una UX coherente en Claude, Cursor, Antigravity, Codex y OpenCode.
4. Mantener las instalaciones inspeccionables, reparables y desinstalables.
5. Preservar la compatibilidad hacia atrás con el estilo actual `ecc-install typescript` durante la transición.

## Fuera de alcance

- empaquetar ECC en múltiples paquetes npm en la primera fase
- construir un marketplace remoto
- una UI completa de plano de control en la misma fase
- resolver todos los problemas de clasificación de skills antes de que se lance la instalación selectiva

## Principios de la experiencia de usuario

### 1. Empezar pequeño

Un usuario debería poder obtener una instalación útil de ECC con un solo comando:

```bash
ecc install --target claude --profile core
```

La experiencia por defecto no debería asumir que el usuario quiere todas las familias de skills y todos los frameworks.

### 2. Crecer por intención

El usuario debería pensar en términos de:

- "Quiero la base para desarrolladores"
- "Necesito TypeScript y Python"
- "Quiero Next.js y Django"
- "Quiero el paquete de seguridad"

El usuario no debería tener que conocer rutas internas crudas del repositorio.

### 3. Previsualizar antes de mutar

Cada ruta de instalación debería admitir planificación en seco:

```bash
ecc install --target cursor --profile developer --with lang:typescript --with framework:nextjs --dry-run
```

El plan debería mostrar claramente:

- componentes seleccionados
- componentes omitidos
- raíz de destino
- rutas administradas
- ubicación esperada del estado de instalación

### 4. La configuración local debe ser de primera clase

Los equipos deberían poder comprometer una configuración de instalación a nivel de proyecto y usar:

```bash
ecc install --config ecc-install.json
```

Eso permite instalaciones deterministas entre colaboradores y en CI.

## Modelo de componentes

El manifiesto actual ya usa módulos de instalación y perfiles. El diseño orientado al usuario debería mantener esa estructura interna, pero presentarla como cuatro familias principales de componentes.

Nota de implementación a corto plazo: algunos IDs de componentes orientados al usuario aún se resuelven a módulos internos compartidos, especialmente en la capa de lenguaje/framework. El catálogo mejora la UX de inmediato mientras preserva un camino claro hacia una granularidad de módulos más fina en fases posteriores.

### 1. Base

Estos son los bloques de construcción predeterminados de ECC:

- reglas centrales
- agentes base
- comandos centrales
- hooks de tiempo de ejecución
- configuraciones de plataforma
- primitivas de calidad de flujo de trabajo

Ejemplos de módulos internos actuales:

- `rules-core`
- `agents-core`
- `commands-core`
- `hooks-runtime`
- `platform-configs`
- `workflow-quality`

### 2. Paquetes de lenguaje

Los paquetes de lenguaje agrupan reglas, orientación y flujos de trabajo para un ecosistema de lenguaje.

Ejemplos:

- `lang:typescript`
- `lang:python`
- `lang:go`
- `lang:java`
- `lang:rust`

Cada paquete de lenguaje debería resolver a uno o más módulos internos más activos específicos del destino.

### 3. Paquetes de framework

Los paquetes de framework se sitúan por encima de los paquetes de lenguaje y traen reglas, skills y configuración opcional específicos del framework.

Ejemplos:

- `framework:react`
- `framework:nextjs`
- `framework:django`
- `framework:springboot`
- `framework:laravel`

Los paquetes de framework deberían depender del paquete de lenguaje correcto o de las primitivas base cuando corresponda.

### 4. Paquetes de capacidades

Los paquetes de capacidades son paquetes transversales de funciones de ECC.

Ejemplos:

- `capability:security`
- `capability:research`
- `capability:orchestration`
- `capability:media`
- `capability:content`

Estos deberían mapearse a las familias de módulos que ya se están introduciendo en los manifiestos.

## Perfiles

Los perfiles siguen siendo la ruta de acceso más rápida.

Perfiles recomendados orientados al usuario:

- `core`
  base mínima, valor predeterminado seguro para la mayoría de los usuarios que prueban ECC
- `developer`
  mejor valor por defecto para trabajo activo de ingeniería de software
- `security`
  base más orientación intensiva en seguridad
- `research`
  base más herramientas de investigación/contenido/investigación
- `full`
  todo lo clasificado y actualmente compatible

Los perfiles deberían poder componerse con banderas adicionales `--with` y `--without`.

Ejemplo:

```bash
ecc install --target claude --profile developer --with lang:typescript --with framework:nextjs --without capability:orchestration
```

## Diseño propuesto del CLI

### Comandos principales

```bash
ecc install
ecc plan
ecc list-installed
ecc doctor
ecc repair
ecc uninstall
ecc catalog
```

### CLI de instalación

Forma recomendada:

```bash
ecc install [--target <target>] [--profile <name>] [--with <component>]... [--without <component>]... [--config <path>] [--dry-run] [--json]
```

Ejemplos:

```bash
ecc install --target claude --profile core
ecc install --target cursor --profile developer --with lang:typescript --with framework:nextjs
ecc install --target antigravity --with capability:security --with lang:python
ecc install --config ecc-install.json
```

### CLI de planificación

Forma recomendada:

```bash
ecc plan [same selection flags as install]
```

Propósito:

- producir una vista previa sin mutación
- actuar como la superficie de depuración canónica para la instalación selectiva

### CLI de catálogo

Forma recomendada:

```bash
ecc catalog profiles
ecc catalog components
ecc catalog components --family language
ecc catalog show framework:nextjs
```

Propósito:

- permitir que los usuarios descubran nombres válidos de componentes sin leer la documentación
- mantener la autoría de configuración accesible

### CLI de compatibilidad

Estos flujos heredados deberían seguir funcionando durante la migración:

```bash
ecc-install typescript
ecc-install --target cursor typescript
ecc typescript
```

Internamente, estos deberían normalizarse al nuevo modelo de solicitud y escribir el estado de instalación de la misma forma que las instalaciones modernas.

## Archivo de configuración propuesto

### Nombre de archivo

Valor predeterminado recomendado:

- `ecc-install.json`

Soporte futuro opcional:

- `.ecc/install.json`

### Forma de la configuración

```json
{
  "$schema": "./schemas/ecc-install-config.schema.json",
  "version": 1,
  "target": "cursor",
  "profile": "developer",
  "include": [
    "lang:typescript",
    "lang:python",
    "framework:nextjs",
    "capability:security"
  ],
  "exclude": [
    "capability:media"
  ],
  "options": {
    "hooksProfile": "standard",
    "mcpCatalog": "baseline",
    "includeExamples": false
  }
}
```

### Semántica de campos

- `target`
  destino de harness seleccionado como `claude`, `cursor`, o `antigravity`
- `profile`
  perfil base desde el que comenzar
- `include`
  componentes adicionales a agregar
- `exclude`
  componentes a sustraer del resultado del perfil
- `options`
  banderas de ajuste para destino/tiempo de ejecución que no cambian la identidad del componente

### Reglas de precedencia

1. Los argumentos del CLI sobrescriben los valores del archivo de configuración.
2. El archivo de configuración sobrescribe los valores predeterminados del perfil.
3. Los valores predeterminados del perfil sobrescriben los valores predeterminados de los módulos internos.

Esto mantiene el comportamiento predecible y fácil de explicar.

## Flujo modular de instalación

El flujo orientado al usuario debería ser:

1. cargar el archivo de configuración si se proporciona o se detecta automáticamente
2. combinar la intención del CLI sobre la intención de configuración
3. normalizar la solicitud en una selección canónica
4. expandir el perfil en componentes base
5. agregar componentes `include`
6. sustraer componentes `exclude`
7. resolver dependencias y compatibilidad con el destino
8. renderizar un plan
9. aplicar operaciones si no está en modo dry-run
10. escribir el estado de instalación

La propiedad importante de UX es que exactamente el mismo flujo impulsa:

- `install`
- `plan`
- `repair`
- `uninstall`

Los comandos difieren en la acción, no en cómo ECC entiende la instalación seleccionada.

## Comportamiento por destino

La instalación selectiva debería preservar el mismo grafo conceptual de componentes en todos los destinos, al tiempo que permite que los adaptadores de destino decidan cómo aterriza el contenido.

### Claude

Mejor ajuste para:

- base de ECC acotada al ámbito del usuario
- comandos, agentes, reglas, hooks, configuración de plataforma, orquestación

### Cursor

Mejor ajuste para:

- instalaciones acotadas al proyecto
- reglas más automatización y configuración local del proyecto

### Antigravity

Mejor ajuste para:

- instalaciones acotadas al proyecto de agentes/reglas/flujo de trabajo

### Codex / OpenCode

Deberían seguir siendo destinos aditivos en lugar de bifurcaciones especiales del instalador.

El diseño de instalación selectiva debería hacer que estos sean solo nuevos adaptadores más nuevas reglas de mapeo específicas del destino, no nuevas arquitecturas de instalador.

## Viabilidad técnica

Este diseño es viable porque el repositorio ya tiene:

- manifiestos de módulos de instalación y perfiles
- adaptadores de destino con rutas de estado de instalación
- inspección de planificación
- registro del estado de instalación
- comandos de ciclo de vida
- una superficie CLI unificada `ecc`

El trabajo faltante no es una invención conceptual. El trabajo faltante es convertir el sustrato actual en un modelo de componentes orientado al usuario más limpio.

### Viable en la fase 1

- selección por perfil + include/exclude
- análisis del archivo de configuración `ecc-install.json`
- comando de catálogo/descubrimiento
- mapeo de alias desde IDs de componentes orientados al usuario a conjuntos de módulos internos
- planificación en seco y JSON

### Viable en la fase 2

- semántica de adaptador de destino más rica
- operaciones aware de merge para activos tipo configuración
- mejor comportamiento de repair/uninstall para operaciones que no son copy

### Más adelante

- superficie de publicación reducida
- bundles delgados generados
- obtención remota de componentes

## Mapeo a los manifiestos actuales de ECC

Los manifiestos actuales todavía no exponen una taxonomía verdadera orientada al usuario `lang:*` / `framework:*` / `capability:*`. Eso debería introducirse como una capa de presentación encima de los módulos existentes, no como un segundo motor de instalación.

Enfoque recomendado:

- mantener `install-modules.json` como el catálogo interno de resolución
- agregar un catálogo de componentes orientado al usuario que mapee IDs de componentes amigables a uno o más módulos internos
- permitir que los perfiles referencien módulos internos o IDs de componentes orientados al usuario durante la ventana de migración

Eso evita romper el sustrato actual de instalación selectiva mientras mejora la UX.

## Despliegue sugerido

### Fase 1: Diseño y descubrimiento

- finalizar la taxonomía de componentes orientados al usuario
- agregar el esquema de configuración
- agregar el diseño del CLI y las reglas de precedencia

### Fase 2: Capa de resolución orientada al usuario

- implementar alias de componentes
- implementar análisis del archivo de configuración
- implementar `include` / `exclude`
- implementar `catalog`

### Fase 3: Semántica de destino más sólida

- mover más lógica a la planificación propiedad del destino
- admitir operaciones de merge/generate de forma limpia
- mejorar la fidelidad de repair/uninstall

### Fase 4: Optimización del empaquetado

- reducir la superficie publicada
- evaluar bundles generados

## Recomendación

El siguiente movimiento de implementación no debería ser "reescribir el instalador".

Debería ser:

1. mantener el sustrato actual de manifiesto/tiempo de ejecución
2. agregar un catálogo de componentes orientado al usuario y un archivo de configuración
3. agregar selección `include` / `exclude` y descubrimiento de catálogo
4. permitir que el planner existente y la pila de ciclo de vida consuman ese modelo

Ese es el camino más corto desde la base de código actual de ECC hasta una experiencia real de instalación selectiva que se sienta como ECC 2.0 en lugar de un instalador heredado grande.
