# Selección de superficie de capacidad

Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

Usa esto como guía de enrutamiento cuando decidas si una capacidad debe vivir en una regla, una skill, un servidor MCP o un flujo de trabajo CLI/API simple.

ECC no trata estas superficies como intercambiables. El objetivo es poner cada capacidad en la superficie más estrecha que preserve la corrección, mantenga bajo control el coste de tokens y no genere fricción innecesaria en tiempo de ejecución o en la cadena de suministro.

## Versión corta

- `rules/` es para restricciones deterministas y siempre activas que deben inyectarse cuando coincide una ruta o evento.
- `skills/` es para flujos de trabajo bajo demanda, playbooks más ricos y guía costosa en tokens que solo debe cargarse cuando sea relevante.
- `MCP` es para capacidades interactivas estructuradas que se benefician de una superficie de herramientas/recursos de larga duración entre sesiones o clientes.
- El `CLI` local o scripts del repo son para acciones deterministas simples que no necesitan un servidor persistente.
- Las llamadas directas a `API` dentro de una skill son para acciones remotas puntuales en las que un servidor MCP completo sería más pesado que el problema.

## Orden de decisión

Haz estas preguntas en orden:

1. ¿Esto debería ocurrir cada vez que coincida una ruta o evento, sin intervención del modelo?
   - Usa una `rule`.
2. ¿Esto es principalmente un playbook, flujo de trabajo o capa de asesoría que debería cargarse solo cuando la tarea realmente lo necesite?
   - Usa una `skill`.
3. ¿La capacidad necesita una interfaz estructurada de herramientas/recursos que varios harnesses o clientes deban llamar repetidamente?
   - Usa `MCP`.
4. ¿Es una acción local simple que puede ejecutarse como script sin mantener un servidor vivo?
   - Usa un punto de entrada local de `CLI` o un script del repo, y envuélvelo con una skill si hace falta.
5. ¿Es solo un paso de integración remota estrecho dentro de un flujo de trabajo más grande?
   - Llama la `API` externa directamente desde la skill o el script.

## Guía por superficie

### Rules

Usa rules para:

- invariantes de código con alcance por ruta
- pisos de seguridad y restricciones de permisos
- restricciones de harness/runtime que siempre deben aplicar
- recordatorios deterministas que no deben depender de la discreción del modelo

No uses rules para:

- playbooks grandes que inflarían cada edición coincidente
- flujos de trabajo opcionales
- contexto de dominio costoso que solo importa a veces

### Skills

Usa skills para:

- flujos de trabajo de múltiples pasos
- guía que requiere juicio
- playbooks de dominio lo bastante costosos como para cargarlos bajo demanda
- orquestación entre scripts, APIs, herramientas MCP y skills vecinas

No uses skills como un cajón de sastre para invariantes estáticas que realmente quieren enrutamiento determinista.

### MCP

Usa MCP cuando la capacidad se beneficie de:

- entradas/salidas estructuradas de herramientas
- recursos o prompts reutilizables
- uso repetido entre clientes
- una interfaz estable que deba funcionar en Claude Code, Codex, Cursor, OpenCode y harnesses relacionados
- que un servidor de larga vida compense la sobrecarga operativa

Evita MCP cuando:

- el trabajo sea un comando local de una sola ejecución
- el único propósito del servidor sea invocar un shell una vez
- el servidor añada más carga de instalación/ejecución que valor de producto

### CLI / scripts del repo

Prefiere un script local o CLI cuando:

- la acción es determinista
- el arranque es barato
- el flujo de trabajo es principalmente local
- no hay beneficio en exponer una superficie persistente de herramientas/recursos

Esto suele ser una buena opción para:

- envoltorios de lint/test/build
- transformaciones locales
- instaladores pequeños
- generación de contenido que se ejecuta una vez por invocación

### Llamadas directas a API

Prefiere llamadas directas a API dentro de una skill o script existente cuando:

- la integración es estrecha
- la acción remota forma parte de un flujo de trabajo más grande
- todavía no necesitas una superficie de transporte reutilizable

Si la misma integración remota se vuelve central, repetida y multi-cliente, esa es la señal para promoverla a una superficie MCP.

## Sesgo hacia coste y fiabilidad

Cuando dos opciones son viables:

- prefiere la superficie de ejecución más pequeña
- prefiere la menor sobrecarga de tokens
- prefiere la ruta con menos piezas externas móviles
- prefiere el empaquetado nativo de ECC antes que introducir otra dependencia de terceros

No normalices dependencias externas de plugins o paquetes como superficies ECC de primera clase salvo que la capacidad valga claramente el mantenimiento, la seguridad y la carga de instalación.

## Política del repo

Cuando traigas ideas desde repos externos:

- copia la idea subyacente, no la dependencia externa
- reempaquétala como una rule, skill, script o superficie MCP nativa de ECC
- renómbrala si la funcionalidad se expandió o se remodeló materialmente para ECC
- evita publicar instrucciones que obliguen a los usuarios a instalar paquetes de terceros no relacionados salvo que esa dependencia sea intencional, auditada y central para el flujo de trabajo

## Ejemplos

- Una invariante de autenticación de backend que debe aplicarse siempre a ediciones en `api/**`:
  - `rule`
- Un playbook más profundo de diseño de API y paginación:
  - `skill`
- Una superficie de búsqueda remota reutilizable usada en varios harnesses:
  - `MCP`
- Un analizador del repo de una sola ejecución que lee archivos locales y escribe un informe:
  - `CLI` local o script, opcionalmente envuelto por una `skill`
- Un paso único de creación de sesión en el portal de facturación dentro de un flujo más amplio de operaciones con clientes:
  - llamada directa a `API` dentro del flujo

## Heurística práctica

Si no estás seguro, empieza más pequeño:

- empieza con una `rule` para invariantes deterministas
- empieza con una `skill` para guía/flujo de trabajo
- empieza con un script para ejecución de una sola vez
- promueve a `MCP` solo cuando la frontera estructurada del servidor pague claramente su coste
