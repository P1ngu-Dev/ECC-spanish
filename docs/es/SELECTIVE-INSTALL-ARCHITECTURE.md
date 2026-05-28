# Descubrimiento de instalación selectiva en ECC 2.0

## Propósito

Este documento convierte el requisito de instalación selectiva del gran plan del 11 de marzo en un diseño concreto de descubrimiento para ECC 2.0.

El objetivo no es solo «copiar menos archivos durante la instalación». El objetivo real es un sistema de instalación que pueda responder, de forma determinista:

- qué se solicitó
- qué se resolvió
- qué se copió o generó
- qué transformaciones específicas del destino se aplicaron
- qué pertenece a ECC y puede eliminarse o repararse con seguridad más adelante

Ese es el contrato que falta entre la instalación de ECC 1.x y una control plane de ECC 2.0.

## Base ya implementada

El primer sustrato de instalación selectiva ya existe en el repositorio:

- `manifests/install-modules.json`
- `manifests/install-profiles.json`
- `schemas/install-modules.schema.json`
- `schemas/install-profiles.schema.json`
- `schemas/install-state.schema.json`
- `scripts/ci/validate-install-manifests.js`
- `scripts/lib/install-manifests.js`
- `scripts/lib/install/request.js`
- `scripts/lib/install/runtime.js`
- `scripts/lib/install/apply.js`
- `scripts/lib/install-targets/`
- `scripts/lib/install-state.js`
- `scripts/lib/install-executor.js`
- `scripts/lib/install-lifecycle.js`
- `scripts/ecc.js`
- `scripts/install-apply.js`
- `scripts/install-plan.js`
- `scripts/list-installed.js`
- `scripts/doctor.js`

Capacidades actuales:

- catálogos legibles por máquina de módulos y perfiles
- validación en CI de que las entradas del manifest apuntan a rutas reales del repositorio
- expansión de dependencias y filtrado por destino
- planificación de operaciones consciente del adapter
- normalización canónica de solicitudes para modos de instalación legacy y manifest
- despacho explícito en tiempo de ejecución desde solicitudes normalizadas hacia la creación de planes
- tanto las instalaciones legacy como las de manifest escriben install-state duradero
- inspección de solo lectura de planes de instalación antes de cualquier mutación
- enrutamiento unificado del CLI `ecc` para comandos de instalación, planificación y lifecycle
- inspección y mutación del lifecycle mediante `list-installed`, `doctor`, `repair` y `uninstall`

Limitación actual:

- las semánticas de merge/remove específicas por destino siguen a nivel de andamiaje para algunos módulos
- la compatibilidad legacy de `ecc-install` todavía apunta a `install.sh`
- la superficie de publicación sigue siendo amplia en `package.json`

## Revisión del código actual

La pila actual del instalador ya es mucho más saludable que el instalador shell original centrado en lenguaje, pero todavía concentra demasiada responsabilidad en unos pocos archivos.

### Ruta de ejecución actual

El flujo actual en tiempo de ejecución es:

1. `install.sh`
   contenedor shell delgado que resuelve la raíz real del paquete
2. `scripts/install-apply.js`
   CLI del instalador orientado al usuario para modos legacy y manifest
3. `scripts/lib/install/request.js`
   análisis del CLI más normalización canónica de la solicitud
4. `scripts/lib/install/runtime.js`
   despacho en tiempo de ejecución desde solicitudes normalizadas hacia planes de instalación
5. `scripts/lib/install-executor.js`
   traducción de argumentos, compatibilidad legacy, materialización de operaciones, mutación del sistema de archivos y escritura de install-state
6. `scripts/lib/install-manifests.js`
   carga del catálogo de módulos/perfiles más expansión de dependencias
7. `scripts/lib/install-targets/`
   andamiaje de raíz de destino y rutas de destino
8. `scripts/lib/install-state.js`
   lectura/escritura de install-state respaldada por schema
9. `scripts/lib/install-lifecycle.js`
   comportamiento de doctor/repair/uninstall derivado de operaciones almacenadas

Eso basta para demostrar el sustrato de instalación selectiva, pero no para que la arquitectura del instalador se sienta asentada.

### Fortalezas actuales

- la intención de instalación ahora es explícita mediante `--profile` y `--modules`
- el análisis de solicitudes y la normalización de solicitudes ahora están separados del shell del CLI
- la resolución de la raíz de destino ya está adaptada
- los comandos de lifecycle ahora usan install-state duradero en lugar de adivinar
- el repositorio ya tiene un punto de entrada Node unificado a través de `ecc` y `install-apply.js`

### Acoplamiento que todavía existe

1. `install-executor.js` es más pequeño que antes, pero todavía lleva demasiadas capas de planificación y materialización a la vez.
   El límite de la solicitud ya está extraído, pero la traducción de solicitudes legacy, la expansión del plan de manifest y la materialización de operaciones siguen viviendo juntas.
2. los adapters de destino siguen siendo demasiado delgados.
   Hoy en su mayoría resuelven raíces y preparan rutas de destino. La semántica real de instalación sigue viviendo en ramas del executor y heurísticas de ruta.
3. el límite entre planner y executor aún no está lo bastante limpio.
   `install-manifests.js` resuelve módulos, pero el conjunto final de operaciones de instalación todavía se construye parcialmente en lógica específica del executor.
4. el comportamiento del lifecycle depende más de operaciones de bajo nivel registradas que de semánticas estables de módulo.
   Eso funciona para copia simple de archivos, pero se vuelve frágil para comportamientos de merge/generate/remove.
5. el modo de compatibilidad está mezclado directamente dentro del runtime principal del instalador.
   Las instalaciones legacy por lenguaje deberían comportarse como un adapter de solicitud, no como una arquitectura de instalador paralela.

## Cambios propuestos de arquitectura modular

El siguiente paso arquitectónico es separar el instalador en capas explícitas, donde cada capa devuelva datos estables en lugar de mutar archivos inmediatamente.

### Estado objetivo

El pipeline de instalación deseado es:

1. superficie del CLI
2. normalización de la solicitud
3. resolución de módulos
4. planificación del destino
5. planificación de operaciones
6. ejecución
7. persistencia de install-state
8. servicios de lifecycle construidos sobre el mismo contrato de operaciones

La idea principal es simple:

- los manifests describen contenido
- los adapters describen semánticas de aterrizaje específicas del destino
- los planners describen lo que debería ocurrir
- los executors aplican esos planes
- los comandos de lifecycle reutilizan el mismo modelo de plan/state en lugar de reinventarlo

### Capas de runtime propuestas

#### 1. Superficie del CLI

Responsabilidad:

- analizar solo la intención del usuario
- enrutar a install, plan, doctor, repair, uninstall
- renderizar salida humana o JSON

No debería encargarse de:

- traducción de lenguaje legacy
- reglas de instalación específicas del destino
- construcción de operaciones

Archivos sugeridos:

```text
scripts/ecc.js
scripts/install-apply.js
scripts/install-plan.js
scripts/doctor.js
scripts/repair.js
scripts/uninstall.js
```

Estos permanecen como entrypoints, pero pasan a ser contenedores delgados alrededor de módulos de biblioteca.

#### 2. Normalizador de solicitudes

Responsabilidad:

- traducir flags crudos del CLI en una solicitud de instalación canónica
- convertir instalaciones legacy por lenguaje en una forma de solicitud de compatibilidad
- rechazar pronto entradas mezcladas o ambiguas

Solicitud canónica sugerida:

```json
{
  "mode": "manifest",
  "target": "cursor",
  "profile": "developer",
  "modules": [],
  "legacyLanguages": [],
  "dryRun": false
}
```

o, en modo de compatibilidad:

```json
{
  "mode": "legacy-compat",
  "target": "claude",
  "profile": null,
  "modules": [],
  "legacyLanguages": ["typescript", "python"],
  "dryRun": false
}
```

Esto permite que el resto del pipeline ignore si la solicitud vino de la sintaxis CLI vieja o nueva.

#### 3. Resolutor de módulos

Responsabilidad:

- cargar catálogos de manifest
- expandir dependencias
- rechazar conflictos
- filtrar módulos no soportados por destino
- devolver un objeto de resolución canónico

Esta capa debe permanecer pura y de solo lectura.

No debe conocer:

- rutas del sistema de archivos de destino
- semánticas de merge
- estrategias de copia

Archivo más cercano actual:

- `scripts/lib/install-manifests.js`

Separación sugerida:

```text
scripts/lib/install/catalog.js
scripts/lib/install/resolve-request.js
scripts/lib/install/resolve-modules.js
```

#### 4. Planner de destino

Responsabilidad:

- seleccionar el adapter de destino de instalación
- resolver la raíz del destino
- resolver la ruta de install-state
- expandir reglas de mapeo módulo-a-destino
- emitir intents de operación conscientes del destino

Aquí es donde debe vivir el significado específico del destino.

Ejemplos:

- Claude puede preservar la jerarquía nativa bajo `~/.claude`
- Cursor puede sincronizar de forma distinta los hijos de la raíz empaquetada `.cursor` respecto de las reglas
- los configs generados pueden requerir semánticas de merge o replace según el destino

Archivos más cercanos actuales:

- `scripts/lib/install-targets/helpers.js`
- `scripts/lib/install-targets/registry.js`

Evolución sugerida:

```text
scripts/lib/install/targets/registry.js
scripts/lib/install/targets/claude-home.js
scripts/lib/install/targets/cursor-project.js
scripts/lib/install/targets/antigravity-project.js
```

Cada adapter debería exponer eventualmente más que `resolveRoot`.
Debería ser dueño del mapeo de ruta y estrategia para su familia de destino.

#### 5. Planner de operaciones

Responsabilidad:

- convertir la resolución de módulos más las reglas del adapter en un grafo tipado de operaciones
- emitir operaciones de primera clase como:
  - `copy-file`
  - `copy-tree`
  - `merge-json`
  - `render-template`
  - `remove`
- adjuntar metadatos de ownership y validación

Esta es la costura arquitectónica que falta en el instalador actual.

Hoy, las operaciones están parcialmente a nivel de andamiaje y parcialmente específicas del executor.
ECC 2.0 debería hacer que la planificación de operaciones sea una fase independiente para que:

- `plan` sea una verdadera vista previa de la ejecución
- `doctor` pueda validar el comportamiento previsto, no solo los archivos actuales
- `repair` pueda reconstruir con seguridad el trabajo exacto que falta
- `uninstall` pueda revertir solo operaciones administradas

#### 6. Motor de ejecución

Responsabilidad:

- aplicar un grafo tipado de operaciones
- imponer reglas de sobrescritura y ownership
- preparar escrituras de forma segura
- recopilar los resultados finales de operaciones aplicadas

Esta capa no debería decidir *qué* hacer.
Solo debería decidir *cómo* aplicar de forma segura un tipo de operación proporcionado.

Archivo más cercano actual:

- `scripts/lib/install-executor.js`

Refactor recomendado:

```text
scripts/lib/install/executor/apply-plan.js
scripts/lib/install/executor/apply-copy.js
scripts/lib/install/executor/apply-merge-json.js
scripts/lib/install/executor/apply-remove.js
```

Eso convierte la lógica del executor de un runtime grande con muchas ramas en un conjunto de pequeños handlers de operaciones.

#### 7. Almacén de install-state

Responsabilidad:

- validar y persistir install-state
- registrar la solicitud canónica, la resolución y las operaciones aplicadas
- dar soporte a comandos de lifecycle sin obligarlos a reconstruir la instalación por ingeniería inversa

Archivo más cercano actual:

- `scripts/lib/install-state.js`

Esta capa ya está bastante cerca de la forma correcta. El cambio principal que queda es almacenar metadatos de operación más ricos una vez que las semánticas de merge/generate sean reales.

#### 8. Servicios de lifecycle

Responsabilidad:

- `list-installed`: inspeccionar solo el estado
- `doctor`: comparar la vista deseada/install-state contra el sistema de archivos actual
- `repair`: reconstruir un plan desde el estado y volver a aplicar operaciones seguras
- `uninstall`: eliminar solo salidas propiedad de ECC

Archivo más cercano actual:

- `scripts/lib/install-lifecycle.js`

Esta capa debería operar eventualmente sobre tipos de operación y políticas de ownership, no solo sobre registros `copy-file` en bruto.

## Propuesta de layout de archivos

El estado final modular limpio debería verse aproximadamente así:

```text
scripts/lib/install/
  catalog.js
  request.js
  resolve-modules.js
  plan-operations.js
  state-store.js
  targets/
    registry.js
    claude-home.js
    cursor-project.js
    antigravity-project.js
    codex-home.js
    opencode-home.js
  executor/
    apply-plan.js
    apply-copy.js
    apply-merge-json.js
    apply-render-template.js
    apply-remove.js
  lifecycle/
    discover.js
    doctor.js
    repair.js
    uninstall.js
```

Esto no es una división de empaquetado.
Es una división de ownership de código dentro del repositorio actual para que cada capa tenga una sola tarea.

## Mapa de migración desde los archivos actuales

La ruta de migración de menor riesgo es evolutiva, no una reescritura.

### Conservar

- `install.sh` como shim de compatibilidad público
- `scripts/ecc.js` como el CLI unificado
- `scripts/lib/install-state.js` como punto de partida para el almacén de estado
- los IDs actuales de adapters de destino y ubicaciones de estado

### Extraer

- el análisis de solicitudes y la traducción de compatibilidad fuera de `scripts/lib/install-executor.js`
- la planificación de operaciones consciente del destino fuera de las ramas del executor y dentro de adapters de destino más módulos planner
- el análisis específico de lifecycle fuera del monolito compartido de lifecycle hacia servicios más pequeños

### Reemplazar gradualmente

- heurísticas amplias de copia por ruta con operaciones tipadas
- planificación de adapter solo de andamiaje con semánticas propiedad del adapter
- ramas legacy de instalación por lenguaje con traducción de solicitudes legacy hacia el mismo pipeline planner/executor

## Cambios arquitectónicos inmediatos a hacer después

Si el objetivo es ECC 2.0 y no solo «suficientemente funcional», los siguientes pasos de modularización deberían ser:

1. dividir `install-executor.js` en módulos de normalización de solicitudes, planificación de operaciones y ejecución
2. mover las decisiones de estrategia específicas del destino hacia métodos de planificación propiedad del adapter
3. hacer que `repair` y `uninstall` operen sobre handlers de operaciones tipadas en lugar de solo registros `copy-file` planos
4. enseñar a los manifests sobre estrategia de instalación y ownership para que el planner ya no dependa de heurísticas de ruta
5. reducir la superficie de publicación npm solo después de que los límites internos de módulos estén estables

## Por qué el modelo actual no basta

Hoy ECC todavía se comporta como un copiador amplio de payload:

- `install.sh` es orientado a lenguaje y pesado en ramas por destino
- los destinos son en parte implícitos en la estructura de directorios
- uninstall, repair y doctor ya existen, pero siguen siendo comandos tempranos de lifecycle
- el repositorio no puede probar qué escribió realmente una instalación anterior
- la superficie de publicación sigue siendo amplia en `package.json`

Eso crea los problemas ya señalados en el mega plan:

- los usuarios descargan más contenido del que su harness o flujo de trabajo necesita
- el soporte y las actualizaciones son más difíciles porque las instalaciones no quedan registradas
- el comportamiento por destino deriva porque la lógica de instalación está duplicada en ramas de shell
- futuros destinos como Codex u OpenCode requieren más lógica de casos especiales en lugar de reutilizar un contrato estable de instalación

## Tesis de diseño de ECC 2.0

La instalación selectiva debería modelarse como:

1. resolver la intención solicitada en un grafo canónico de módulos
2. traducir ese grafo a través de un adapter de destino
3. ejecutar un conjunto determinista de operaciones de instalación
4. escribir install-state como la fuente de verdad duradera

Eso significa que ECC 2.0 necesita dos contratos, no uno:

- un contrato de contenido
  qué módulos existen y cómo dependen entre sí
- un contrato de destino
  cómo aterrizan esos módulos dentro de Claude, Cursor, Antigravity, Codex u OpenCode

El repositorio actual solo tenía la primera mitad en una forma temprana.
El repositorio actual ahora tiene la primera vertical slice completa, pero no las semánticas completas específicas del destino.

## Restricciones de diseño

1. Mantener `everything-claude-code` como el repositorio fuente canónico.
2. Preservar los flujos existentes de `install.sh` durante la migración.
3. Soportar destinos con alcance de home y de proyecto desde el mismo planner.
4. Hacer posible uninstall/repair/doctor sin adivinar.
5. Evitar que la lógica de copia por destino se filtre de vuelta a las definiciones de módulo.
6. Mantener el soporte futuro para Codex y OpenCode como adición, no como reescritura.

## Artefactos canónicos

### 1. Catálogo de módulos

El catálogo de módulos es el grafo canónico de contenido.

Campos ya implementados actualmente:

- `id`
- `kind`
- `description`
- `paths`
- `targets`
- `dependencies`
- `defaultInstall`
- `cost`
- `stability`

Campos que todavía hacen falta para ECC 2.0:

- `installStrategy`
  por ejemplo `copy`, `flatten-rules`, `generate`, `merge-config`
- `ownership`
  si ECC posee completamente la ruta de destino o solo los archivos generados debajo de ella
- `pathMode`
  por ejemplo `preserve`, `flatten`, `target-template`
- `conflicts`
  módulos o familias de rutas que no pueden coexistir en un solo destino
- `publish`
  si el módulo se empaqueta por defecto, como opcional, o se genera después de la instalación

Forma futura sugerida:

```json
{
  "id": "hooks-runtime",
  "kind": "hooks",
  "paths": ["hooks", "scripts/hooks"],
  "targets": ["claude", "cursor", "opencode"],
  "dependencies": [],
  "installStrategy": "copy",
  "pathMode": "preserve",
  "ownership": "managed",
  "defaultInstall": true,
  "cost": "medium",
  "stability": "stable"
}
```

### 2. Catálogo de perfiles

Los perfiles se mantienen delgados.

Deben expresar la intención del usuario, no duplicar la lógica del destino.

Ejemplos ya implementados actualmente:

- `core`
- `developer`
- `security`
- `research`
- `full`

Campos que todavía hacen falta:

- `defaultTargets`
- `recommendedFor`
- `excludes`
- `requiresConfirmation`

Eso permite que ECC 2.0 diga cosas como:

- `developer` es el valor predeterminado recomendado para Claude y Cursor
- `research` puede ser pesado para instalaciones locales pequeñas
- `full` está permitido pero no es el valor predeterminado

### 3. Adapters de destino

Esta es la principal capa que falta.

El grafo de módulos no debería saber:

- dónde vive el home de Claude
- cómo Cursor aplana o remapea contenido
- qué archivos de configuración necesitan semánticas de merge en lugar de copia ciega

Eso pertenece a un adapter de destino.

Interfaz sugerida:

```ts
type InstallTargetAdapter = {
  id: string;
  kind: "home" | "project";
  supports(target: string): boolean;
  resolveRoot(input?: string): Promise<string>;
  planOperations(input: InstallOperationInput): Promise<InstallOperation[]>;
  validate?(input: InstallOperationInput): Promise<ValidationIssue[]>;
};
```

Primeros adapters sugeridos:

1. `claude-home`
   escribe en `~/.claude/...`
2. `cursor-project`
   escribe en `./.cursor/...`
3. `antigravity-project`
   escribe en `./.agent/...`
4. `codex-home`
   más adelante
5. `opencode-home`
   más adelante

Esto sigue el mismo patrón ya propuesto en el documento de descubrimiento de adapters de sesión: contrato canónico primero, adapter específico del harness después.

## Modelo de planificación de instalación

El `scripts/install-plan.js` actual demuestra que el repositorio puede resolver módulos solicitados en un conjunto filtrado de módulos.

ECC 2.0 necesita la siguiente capa: planificación de operaciones.

Fases sugeridas:

1. normalización de entrada
   - analizar `--target`
   - analizar `--profile`
   - analizar `--modules`
   - opcionalmente traducir argumentos legacy de lenguaje
2. resolución de módulos
   - expandir dependencias
   - rechazar conflictos
   - filtrar por destinos soportados
3. planificación por adapter
   - resolver la raíz del destino
   - derivar operaciones exactas de copia o generación
   - identificar merges de config y remapeos por destino
4. salida de dry-run
   - mostrar módulos seleccionados
   - mostrar módulos omitidos
   - mostrar operaciones exactas de archivo
5. mutación
   - ejecutar el plan de operaciones
6. escritura de estado
   - persistir install-state solo después de completar con éxito

Forma sugerida de operación:

```json
{
  "kind": "copy",
  "moduleId": "rules-core",
  "source": "rules/common/coding-style.md",
  "destination": "/Users/example/.claude/rules/ecc/common/coding-style.md",
  "ownership": "managed",
  "overwritePolicy": "replace"
}
```

Otros tipos de operación:

- `copy`
- `copy-tree`
- `flatten-copy`
- `render-template`
- `merge-json`
- `merge-jsonc`
- `mkdir`
- `remove`

## Contrato de install-state

Install-state es el contrato duradero que le falta a ECC 1.x.

Convenciones de ruta sugeridas:

- destino Claude:
  `~/.claude/ecc/install-state.json`
- destino Cursor:
  `./.cursor/ecc-install-state.json`
- destino Antigravity:
  `./.agent/ecc-install-state.json`
- futuro destino Codex:
  `~/.codex/ecc/install-state.json`

Payload sugerido:

```json
{
  "schemaVersion": "ecc.install.v1",
  "installedAt": "2026-03-13T00:00:00Z",
  "lastValidatedAt": "2026-03-13T00:00:00Z",
  "target": {
    "id": "claude-home",
    "root": "/Users/example/.claude"
  },
  "request": {
    "profile": "developer",
    "modules": ["orchestration"],
    "legacyLanguages": ["typescript", "python"]
  },
  "resolution": {
    "selectedModules": [
      "rules-core",
      "agents-core",
      "commands-core",
      "hooks-runtime",
      "platform-configs",
      "workflow-quality",
      "framework-language",
      "database",
      "orchestration"
    ],
    "skippedModules": []
  },
  "source": {
    "repoVersion": "2.0.0-rc.1",
    "repoCommit": "git-sha",
    "manifestVersion": 1
  },
  "operations": [
    {
      "kind": "copy",
      "moduleId": "rules-core",
      "destination": "/Users/example/.claude/rules/ecc/common/coding-style.md",
      "digest": "sha256:..."
    }
  ]
}
```

Requisitos del estado:

- suficiente detalle para que uninstall elimine solo salidas administradas por ECC
- suficiente detalle para que repair compare lo deseado frente a los archivos realmente instalados
- suficiente detalle para que doctor explique el drift en lugar de adivinar

## Comandos de lifecycle

Los siguientes comandos son la superficie de lifecycle para install-state:

1. `ecc list-installed`
2. `ecc uninstall`
3. `ecc doctor`
4. `ecc repair`

Estado actual de implementación:

- `ecc list-installed` enruta a `node scripts/list-installed.js`
- `ecc uninstall` enruta a `node scripts/uninstall.js`
- `ecc doctor` enruta a `node scripts/doctor.js`
- `ecc repair` enruta a `node scripts/repair.js`
- los entrypoints legacy del script siguen disponibles durante la migración

### `list-installed`

Responsabilidades:

- mostrar id y raíz del destino
- mostrar perfil/módulos solicitados
- mostrar módulos resueltos
- mostrar versión de origen y tiempo de instalación

### `uninstall`

Responsabilidades:

- cargar install-state
- eliminar solo los destinos administrados por ECC registrados en el estado
- dejar intactos los archivos no relacionados escritos por el usuario
- borrar install-state solo después de una limpieza exitosa

### `doctor`

Responsabilidades:

- detectar archivos administrados faltantes
- detectar drift inesperado de configuración
- detectar raíces de destino que ya no existen
- detectar desajustes de manifest/versión

### `repair`

Responsabilidades:

- reconstruir el plan de operaciones deseado desde install-state
- volver a copiar archivos administrados faltantes o desviados
- rechazar repair si los módulos solicitados ya no existen en el manifest actual, salvo que exista un mapa de compatibilidad

## Capa de compatibilidad legacy

`install.sh` actual acepta:

- `--target <claude|cursor|antigravity>`
- una lista de nombres de lenguajes

Ese comportamiento no puede desaparecer en un solo corte porque los usuarios ya dependen de él.

ECC 2.0 debería traducir los argumentos legacy de lenguaje a una solicitud de compatibilidad.

Enfoque sugerido:

1. mantener la forma actual del CLI para el modo legacy
2. mapear nombres de lenguaje a solicitudes de módulo como:
   - `rules-core`
   - subconjuntos de reglas compatibles con el destino
3. escribir install-state incluso para instalaciones legacy
4. etiquetar la solicitud como `legacyMode: true`

Ejemplo:

```json
{
  "request": {
    "legacyMode": true,
    "legacyLanguages": ["typescript", "python"]
  }
}
```

Esto mantiene disponible el comportamiento antiguo mientras mueve todas las instalaciones al mismo contrato de estado.

## Límite de publicación

El paquete npm actual todavía publica un payload amplio a través de `package.json`.

ECC 2.0 debería mejorar esto con cuidado.

Secuencia recomendada:

1. mantener primero un único paquete npm canónico
2. usar manifests para impulsar la selección en tiempo de instalación antes de cambiar la forma de publicación
3. considerar más tarde reducir la superficie empaquetada solo donde sea seguro

Por qué:

- la instalación selectiva puede llegar antes que una cirugía agresiva del paquete
- uninstall y repair dependen más de install-state que de cambios en la publicación
- el soporte para Codex/OpenCode es más fácil si el origen del paquete sigue unificado

Posibles direcciones futuras:

- bundles delgados generados por perfil
- tarballs generados específicos por destino
- fetch remoto opcional de módulos pesados

Eso es fase 3 o posterior, no un prerrequisito para instalaciones conscientes de perfil.

## Recomendación de layout de archivos

Archivos sugeridos para el siguiente paso:

```text
scripts/lib/install-targets/
  claude-home.js
  cursor-project.js
  antigravity-project.js
  registry.js
scripts/lib/install-state.js
scripts/ecc.js
scripts/install-apply.js
scripts/list-installed.js
scripts/uninstall.js
scripts/doctor.js
scripts/repair.js
tests/lib/install-targets.test.js
tests/lib/install-state.test.js
tests/lib/install-lifecycle.test.js
```

`install.sh` puede seguir siendo el punto de entrada visible para el usuario durante la migración, pero debería convertirse en una capa shell delgada alrededor de un planner y executor basados en Node, en lugar de seguir creciendo con ramas shell por destino.

## Secuencia de implementación

### Fase 1: Planner a contrato

1. mantener el schema actual del manifest y el resolvedor
2. agregar planificación de operaciones sobre los módulos resueltos
3. definir el schema de estado `ecc.install.v1`
4. escribir install-state en una instalación exitosa

### Fase 2: Adapters de destino

1. extraer el comportamiento de instalación de Claude en el adapter `claude-home`
2. extraer el comportamiento de instalación de Cursor en el adapter `cursor-project`
3. extraer el comportamiento de instalación de Antigravity en el adapter `antigravity-project`
4. reducir `install.sh` a análisis de argumentos más invocación del adapter

### Fase 3: Lifecycle

1. añadir semánticas de merge/remove más fuertes y específicas del destino
2. ampliar la cobertura de repair/uninstall para operaciones que no sean copia
3. reducir la superficie de publicación del paquete al grafo de módulos en lugar de carpetas amplias
4. decidir cuándo `ecc-install` debería convertirse en un alias delgado de `ecc install`

### Fase 4: Publicación y futuros destinos

1. evaluar una reducción segura de la superficie de `package.json`
2. añadir `codex-home`
3. añadir `opencode-home`
4. considerar bundles de perfil generados si la presión de empaquetado sigue siendo alta

## Próximos pasos inmediatos en el repositorio

Los movimientos siguientes con mayor señal en este repositorio son:

1. agregar semánticas de merge/remove específicas del destino para módulos tipo configuración
2. ampliar repair y uninstall más allá de operaciones simples `copy-file`
3. reducir la superficie de publicación del paquete al grafo de módulos en lugar de carpetas amplias
4. decidir si `ecc-install` sigue separado o se convierte en `ecc install`
5. agregar pruebas que fijen:
   - el comportamiento de merge/remove específico del destino
   - la seguridad de repair y uninstall para operaciones que no sean copia
   - el enrutamiento unificado del CLI `ecc` y las garantías de compatibilidad

## Preguntas abiertas

1. ¿Deben las reglas seguir siendo direccionables por lenguaje en modo legacy para siempre, o solo durante la ventana de migración?
2. ¿`platform-configs` debería instalarse siempre con `core`, o dividirse en módulos más pequeños y específicos del destino?
3. ¿Queremos que las semánticas de merge de configuración queden registradas a nivel de operación o solo en la lógica del adapter?
4. ¿Las familias de skills pesadas deberían moverse eventualmente a fetch bajo demanda en lugar de incluirse en tiempo de empaquetado?
5. ¿Los adapters de destino para Codex y OpenCode deberían publicarse solo después de que los comandos de lifecycle para Claude/Cursor estén estables?

## Recomendación

Trata el resolvedor de manifest actual como el adapter `0` para instalaciones:

1. preservar la superficie actual de instalación
2. mover el comportamiento real de copia detrás de adapters de destino
3. escribir install-state para cada instalación exitosa
4. hacer que uninstall, doctor y repair dependan solo de install-state
5. solo entonces reducir el empaquetado o añadir más destinos

Ese es el camino más corto desde la dispersión del instalador de ECC 1.x hacia un contrato de instalación/control de ECC 2.0 que sea determinista, mantenible y extensible.
