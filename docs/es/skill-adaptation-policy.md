# Política de adaptación de skills

ECC acepta ideas de repos externos, pero las skills publicadas deben convertirse en superficies nativas de ECC.

## Regla predeterminada

Cuando una contribución parte de otro repositorio open source, un prompt pack, un plugin, un harness o una configuración personal:

- copia la idea, el flujo de trabajo o la estructura subyacente
- adáptala a las superficies de instalación, el flujo de validación y las convenciones del repositorio de ECC
- elimina el branding externo innecesario, las suposiciones sobre dependencias y el encuadre específico del upstream

El objetivo es reutilizar sin convertir ECC en un simple envoltorio sobre el runtime de otra persona.

## Cuándo conservar el nombre original

Conserva el nombre original de la skill solo cuando todo lo siguiente sea cierto:

- la contribución es casi un port directo
- el nombre ya es descriptivo y neutral
- la superficie sigue comportándose como el concepto del upstream
- no existe ya en el repositorio un mejor nombre nativo de ECC

Ejemplos:

- nombres de frameworks como `nestjs-patterns`
- nombres de protocolos o productos que son el tema, no el discurso del proveedor

## Cuándo renombrar

Renombra la skill cuando ECC amplía, reduce o reempaqueta de forma significativa el trabajo original.

Activadores típicos:

- ECC añade nuevo comportamiento, estructura o guía sustancial
- el nombre original está orientado al proveedor o a la marca de la comunidad en lugar de al flujo de trabajo
- la contribución se solapa con una superficie existente de ECC y necesita un límite más claro
- la contribución ahora encaja mejor como una capability, un flujo de trabajo de operador o una capa de política, en lugar de un port literal

Ejemplos:

- conserva un primitivo de grafo reutilizable como `social-graph-ranker`, pero convierte capas de flujo de trabajo más amplias en `lead-intelligence` o `connections-optimizer`
- prefiere nombres nativos de ECC como `product-capability` en lugar de etiquetas de planificación importadas y vagas si el alcance cambió de forma material

## Política de dependencias

ECC prefiere la superficie nativa más estrecha que resuelva el problema:

- `rules/` para restricciones deterministas
- `skills/` para flujos de trabajo bajo demanda
- MCP cuando se justifica una frontera de herramienta interactiva de larga duración
- scripts/CLI locales para ejecución determinista de una sola vez
- APIs directas cuando la llamada remota es acotada y no justifica MCP

Evita publicar una skill que exista principalmente para decirles a los usuarios que instalen o confíen en un paquete de terceros no verificado.

Si vale la pena conservar la funcionalidad externa:

- internaliza o recrea la lógica relevante dentro de ECC cuando sea práctico
- o mantén la integración opcional y claramente marcada como externa
- nunca permitas que una nueva dependencia externa se convierta en la ruta predeterminada sin una justificación explícita

## Preguntas de revisión

Antes de fusionar una skill contribuida, responde estas preguntas:

1. ¿Es esta una superficie realmente reutilizable en ECC, o solo documentación para otra herramienta?
2. ¿El nombre actual sigue coincidiendo con la superficie con forma de ECC?
3. ¿Ya existe una skill de ECC que posea la mayor parte de este comportamiento?
4. ¿Estamos importando un concepto, o estamos importando la identidad de producto de otra persona?
5. ¿Un usuario de ECC entendería el propósito de esta skill sin conocer el repositorio upstream?

Si esas respuestas son débiles, adapta más, reduce el alcance o no la publiques.
