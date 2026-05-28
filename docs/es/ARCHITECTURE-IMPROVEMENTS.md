Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Recomendaciones de mejora de la arquitectura

Este documento captura las mejoras a nivel de arquitecto para el proyecto Everything Claude Code (ECC). Está escrito desde la perspectiva de un arquitecto de codificación Claude Code con el objetivo de mejorar la mantenibilidad, la coherencia y la calidad a largo plazo.

---

## 1. Documentación y fuente única de verdad

### 1.1 Sincronización de agente/comando/recuento de habilidades

**Problema:** AGENTS.md indica "13 agents especializados, 50+ skills, 33 commands", mientras que el repo tiene **16 agents**, **65+ skills** y **40 commands**. README y otros documentos también varían. Esto causa confusión entre los contribuyentes y usuarios.

**Recomendación:**

- **Fuente única de verdad:** deriva recuentos (y, opcionalmente, tablas) del sistema de archivos o de un pequeño manifiesto. Opciones:
  - **Opción A:** Agregue un script (por ejemplo, `scripts/ci/catalog.js`) que escanee `agents/*.md`, `commands/*.md` y `skills/*/SKILL.md` y genere JSON/Markdown. CI y los médicos pueden consumir esto.
  - **Opción B:** Mantener un `docs/catalog.json` (o YAML) que enumere agents, commands y skills con metadatos; guiones y documentos leídos de él. Requiere disciplina para actualizar al agregar o quitar.
- **A corto plazo:** Sincronice manualmente AGENTS.md, README.md y CLAUDE.md con los recuentos reales y enumere cualquier agents nuevo (por ejemplo, jefe de personal, operador de bucle, harness-optimizador) en la tabla de agentes.

**Impacto:** Alto: afecta la primera impresión y la confianza de los contribuyentes.

---

### 1.2 Comando → Agente / Mapa de habilidades

**Problema:** No existe un mapa único legible por máquinas o humanos sobre "qué comando utiliza qué agente(s) o habilidad(s)". Esto se encuentra en tablas README y archivos de comandos individuales `.md`, que pueden variar.

**Recomendación:**

- Agregue un **registro de comandos** (por ejemplo, en `docs/` o como contenido preliminar en archivos de comandos) que enumere para cada comando: nombre, descripción, agente(s) principal(es), skills al que se hace referencia. Puede generarse a partir del contenido del archivo de comando o mantenerse manualmente.
- Exponer un "mapa" en documentos (por ejemplo, `docs/COMMAND-AGENT-MAP.md`) o en el catálogo generado para su descubrimiento y uso de herramientas (por ejemplo, "¿cuáles commands usan tdd-guide?").

**Impacto:** Medio: mejora la capacidad de descubrimiento y la seguridad de la refactorización.

---

## 2. Pruebas y calidad

### 2.1 Descubrimiento de pruebas versus lista codificada

**Problema:** `tests/run-all.js` utiliza una **lista codificada** de archivos de prueba. Los nuevos archivos de prueba no se ejecutan a menos que alguien actualice `run-all.js`, por lo que la cobertura puede quedar incompleta por omisión.

**Recomendación:**

- **Descubrimiento basado en global:** Descubra archivos de prueba por patrón (por ejemplo, `**/*.test.js` en `tests/`) y ejecútelos, con una lista de permitidos/denegados opcional para casos especiales. Esto hace que las nuevas pruebas formen parte automáticamente del conjunto.
- Mantenga un único punto de entrada (`tests/run-all.js`) que ejecute pruebas descubiertas y agregue resultados.

**Impacto:** Alto: evita la regresión cuando existen nuevas pruebas pero nunca se ejecutan.

---

### 2.2 Métricas de cobertura de pruebas

**Problema:** No existe una herramienta de cobertura (por ejemplo, nyc/c8/istanbul). El proyecto no puede afirmar que sus propios guiones tengan una "cobertura superior al 80%"; la cobertura es implícita.

**Recomendación:**

- Introducir una herramienta de cobertura para scripts de Node (por ejemplo, `c8` o `nyc`) y ejecutarla en CI. Comience con una línea de base (por ejemplo, 60%) y aumente con el tiempo; o al menos informar la cobertura en CI sin fallar para que el equipo pueda ver las tendencias.
- Centrarse en `scripts/` (lib + hooks + ci) como objetivo principal; excluya guiones únicos si es necesario.

**Impacto:** Medio: alinea el proyecto con su propia guía AGENTS.md (más del 80% de cobertura) y muestra caminos no probados.

---

## 3. Esquema y Validación

### 3.1 Utilice el esquema JSON de ganchos en CI

**Problema:** `schemas/hooks.schema.json` existe y define la forma de configuración del gancho, pero `scripts/ci/validate-hooks.js` **no** la usa. La validación está duplicada (VALID_EVENTS, estructura) y puede desviarse del esquema.

**Recomendación:**

- Utilice un validador de esquema JSON (por ejemplo, `ajv`) en `validate-hooks.js` para validar `hooks/hooks.json` contra `schemas/hooks.schema.json`. Mantenga al validador como la única fuente de verdad para la estructura; conserve solo comprobaciones específicas de gancho (por ejemplo, sintaxis JS en línea) en el script.
- Garantiza que el esquema y el validador permanezcan sincronizados y permite la validación del IDE/editor a través de `$schema` en hooks.json.

**Impacto:** Medio: reduce la deriva y mejora la experiencia del colaborador al editar hooks.

---

## 4. Arnés cruzado e i18n

### 4.1 Sincronización del subconjunto de habilidades/agentes (.agents/skills, .cursor/skills)

**Problema:** `.agents/skills/` (Codex) y `.cursor/skills/` son subconjuntos de `skills/`. Agregar o eliminar una habilidad en el repo principal requiere actualizar manualmente estos subconjuntos, que pueden olvidarse.

**Recomendación:**

- Documento en CONTRIBUTING.md que para agregar una habilidad puede requerir la actualización de `.agents/skills` y `.cursor/skills` (y cómo hacerlo).
- Opcionalmente: una verificación de CI o script que compara `skills/` con los subconjuntos y falla o advierte si una habilidad está en un conjunto pero no en el otro cuando debería estar (por ejemplo, por convención o por un pequeño manifiesto).

**Impacto:** Bajo a Medio: reduce la deriva cruzada harness.

---

### 4.2 Deriva de la traducción (docs/ zh-CN, zh-TW, ja-JP)

**Problema:** Traducciones en `docs/` duplicado agents, commands, skills. A medida que la fuente en inglés evoluciona, las traducciones pueden quedar obsoletas sin un proceso o herramientas claras.

**Recomendación:**

- Documente un **proceso de traducción:** cuándo actualizar (por ejemplo, en el momento del lanzamiento), quién es el propietario de cada configuración regional y cómo detectar contenido obsoleto (por ejemplo, listas de archivos de diferencias o secciones clave).
- Considere: archivo de estado de traducción (por ejemplo, `docs/i18n-status.md`) o CI que verifique la existencia/marcas de tiempo del archivo de traducción y advierta si el inglés se actualizó más recientemente que una traducción.
- A largo plazo: considere el formato de extracción/marcador de posición (por ejemplo, claves i18n) para que las traducciones hagan referencia a la misma estructura que la fuente en inglés.

**Impacto:** Medio: mejora la experiencia para los usuarios que no hablan inglés y reduce la confusión causada por traducciones obsoletas.

---

## 5. Ganchos y guiones

### 5.1 Consistencia en tiempo de ejecución del gancho

**Problema:** Los ganchos deben mantener una superficie de despacho en modo Nodo consistente. La observación de aprendizaje continuo ahora se envía a través de `run-with-flags.js` y `observe-runner.js`, que delega a la implementación `observe.sh` existente sin exponer una entrada de enlace en modo shell.

**Recomendación:**

- Prefiera Node para el nuevo hooks cuando sea posible (multiplataforma, tiempo de ejecución único). Si se requiere una carcasa, documente el motivo y mantenga la superficie pequeña.
- Asegúrese de que `ECC_HOOK_PROFILE` y `ECC_DISABLED_HOOKS` se respeten en todas las rutas de código (incluido el shell) para que el comportamiento sea coherente.

**Impacto:** Bajo: mantiene el diseño actual; mejora si más hooks migran a Node.

---

## 6. Tabla resumen

| Área              | Mejora                          | Prioridad | Esfuerzo  |
|-------------------|--------------------------------------|----------|---------|
| Sincronización de documentos          | Sincronizar AGENTS.md/README recuentos y tabla | Alto     | Bajo     |
| Fuente única     | Script o manifiesto del catálogo           | Alto     | Medio  |
| Descubrimiento de prueba    | Ejecutor de pruebas basado en global               | Alto     | Bajo     |
| Cobertura          | Agregue cobertura c8/nyc y CI           | Medio   | Medio  |
| Esquema de gancho en CI | Validar hooks.json mediante esquema       | Medio   | Bajo     |
| mapa de comando       | Comando → registro de agente/habilidad       | Medio   | Medio  |
| Sincronización de subconjuntos       | Documento/CI para .agents/.cursor       | Bajo-medio  | Bajo-medio |
| Traducciones      | Proceso + detección obsoleta             | Medio   | Medio  |
| Tiempo de ejecución del gancho      | Preferir nodo; uso del shell de documentos       | Bajo      | Bajo     |

---

## 7. Ganancias rápidas (inmediatas)

1. **Actualización AGENTS.md:** Establezca el recuento de agentes en 16; agregue jefe de personal, operador de bucle, optimizador harness a la tabla de agentes; alinear el recuento de habilidades/comandos con repo.
2. **Descubrimiento de pruebas:** Cambie `run-all.js` para descubrir `**/*.test.js` en `tests/` (con lista de permitidos opcional) para que siempre se ejecuten nuevas pruebas.
3. **Esquema de conexión hooks:** En `validate-hooks.js`, valide `hooks/hooks.json` contra `schemas/hooks.schema.json` usando ajv (o similar) y mantenga solo comprobaciones específicas de gancho en el script.

Estos tres se pueden realizar en uno o dos sessions y mejoran materialmente la coherencia y la confiabilidad.