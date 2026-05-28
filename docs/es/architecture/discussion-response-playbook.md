# Playbook de respuesta a discusiones

Este playbook convierte GitHub Discussions en la misma cola operativa que los
PRs, issues, el trabajo en Linear y la evidencia de release. Es una guía de
operador, no una promesa de que cada hilo informativo necesite una respuesta
pública.

## Bucle de auditoría

Ejecuta estas comprobaciones antes de un release, después de un lote de merge
importante y cuando se refresque Linear ITO-59:

```bash
npm run discussion:audit -- --json
node scripts/platform-audit.js --json
```

La cola solo está al día cuando:

- los errores al obtener discusiones están explicados o corregidos;
- `needsMaintainerTouch` es cero para categorías de discusión tipo soporte; 
- las discusiones de Q&A con respuesta posible tienen una respuesta aceptada o
  una nota clara de ruteo; y
- cualquier hilo de alcance de producto está vinculado a un issue de GitHub, un
  issue de Linear, una fila de roadmap o una postergación explícita.

Los hilos informativos como anuncios, referencias, show-and-tell o updates
escritos por maintainers pueden permanecer visibles sin convertirse en deuda de
respuesta.

## Categorías

| Category | Ruta | Readback requerido |
| --- | --- | --- |
| Soporte de producto o confusión de instalación | Responde con el comando exacto o la ruta del doc; marca la respuesta aceptada para Q&A cuando el fix esté completo | URL de la discusión más URL de la respuesta aceptada cuando aplique |
| Informe de bug | Pide un repro mínimo, versión, harness y logs; crea o vincula un issue de GitHub cuando sea reproducible | URL del issue o razón de postergación |
| Solicitud de feature | Reconoce el resultado deseado y enlaza el issue de roadmap más cercano; no insinúes compromiso salvo que esté acotado | Enlace de roadmap de Linear/GitHub |
| Preocupación de seguridad | Mueve los detalles del exploit y los secretos a un canal privado; mantén la respuesta pública corta y no operativa | Nota de escalado privada más respuesta pública de seguridad |
| Pregunta de release o billing | Responde desde el ledger de URLs de release y los gates de publication-readiness; no afirmes URLs no publicadas, readiness de billing ni disponibilidad de plugin | Artefacto de evidencia o enlace de bloqueo |
| Show-and-tell, referencia o anuncio | Déjalo como informativo salvo que haya una pregunta directa o una señal de alcance de producto | Enlace opcional al roadmap si es útil |
| Hilo obsoleto o concluido | Resume el estado actual y enlaza el doc/issue duradero; evita reactivar hilos de baja señal | Nota de cierre o razón explícita de no acción |

## Plantillas

### Soporte público

Gracias por el reporte. La ruta soportada actual es:

```bash
<command>
```

El doc relevante es `<doc path or URL>`. Si esto no coincide con tu setup,
responde con el harness, OS, package manager y el texto exacto del error.

### Coordinación de maintainers

Estoy ruteando esto a `<issue or Linear key>` para que no se pierda en la cola
de discusión. La siguiente decisión es `<specific decision>`. Hasta que eso
llegue, el workaround soportado es `<workaround or "none">`.

### Obsoleto o concluido

Este hilo parece resuelto o superado por `<doc/issue/release>`. Lo dejo visible
por historial, pero ya no es un ítem activo de la cola de soporte. Los nuevos
detalles de repro deben ir a `<issue/discussion path>`.

### Anuncio de release

El estado actual del release es `<rc/beta/GA state>`. Las URLs en vivo se
registran en `docs/releases/2.0.0-rc.1/release-url-ledger-2026-05-18.md`. Todo
lo marcado como pendiente allí no debe anunciarse como publicado todavía.

### Escalado de seguridad

Gracias por señalarlo. Por favor no publiques pasos del exploit, tokens, datos
de clientes ni valores secretos en el hilo público. Estoy ruteando esto por la
ruta de respuesta de seguridad y mantendré el hilo público limitado a
actualizaciones de estado seguras.

## Registro de resultados

Para cada discusión de alta señal, registra uno de estos resultados:

- respondida públicamente y readback de la respuesta aceptada;
- enlazada a un issue de GitHub o Linear;
- ruteada a la ruta de respuesta de seguridad;
- clasificada como informativa; o
- postergada explícitamente con una razón.

Espeja el resumen en ITO-59 cuando cierre el lote, e incluye los conteos en el
siguiente dashboard de operador o actualización de evidencia de publicación.
