---
name: google-workspace-ops
description: Operate across Google Drive, Docs, Sheets, and Slides as one workflow surface for plans, trackers, decks, and shared documents. Use when the user needs to find, summarize, edit, migrate, or clean up Google Workspace assets without dropping to raw tool calls.
origin: ECC
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Operaciones de Google Workspace

Este skill sirve para operar documentos, hojas de cálculo y presentaciones compartidas como sistemas de trabajo, no solo como archivos aislados.

## Cuándo usarlo

- El usuario necesita encontrar un doc, sheet o deck y actualizarlo en su lugar
- Consolidar planes, trackers, notas o listas de clientes almacenadas en Google Drive
- Limpiar o reestructurar una hoja compartida
- Importar, reparar o reformatear una presentación de Google Slides
- Producir resúmenes de Docs, Sheets o Slides para la toma de decisiones

## Superficie de herramientas preferida

Usa Google Drive como punto de entrada y luego cambia al especialista correcto:

- Google Docs para documentos con mucho texto
- Google Sheets para trabajo tabular, fórmulas y gráficos
- Google Slides para decks, importación, migración de plantillas y limpieza

No adivines la estructura solo por el nombre del archivo. Inspecciona primero.

## Flujo de trabajo

### 1. Encontrar el activo

Empieza con la búsqueda en Drive para localizar:

- el archivo exacto
- activos hermanos
- duplicados probables
- versiones modificadas recientemente

Si varios documentos se parecen, confirma por título, propietario, hora de modificación o carpeta.

### 2. Inspeccionar antes de editar

Antes de cambiar nada:

- resume la estructura actual
- identifica pestañas, encabezados o número de diapositivas
- detecta si la tarea es limpieza local o cirugía estructural

Elige la herramienta más pequeña que pueda hacer el trabajo de forma segura.

### 3. Editar con precisión

- Para Docs: usa ediciones conscientes del índice, no reescrituras vagas
- Para Sheets: opera sobre pestañas y rangos explícitos
- Para Slides: distingue cambios de contenido de la limpieza visual o la migración de plantilla

Si el trabajo es visual o sensible al layout, itera con inspección y verificación en vez de una sola actualización ciega.

### 4. Mantener limpio el sistema de trabajo

Cuando el archivo forma parte de un flujo más grande, también muestra:

- trackers duplicados
- decks obsoletos
- docs desactualizados frente a docs canónicos
- si el activo debe archivarse, fusionarse o renombrarse

## Formato de salida

Usa:

```text
ASSET
- nombre del archivo
- tipo
- por qué este es el archivo correcto

CURRENT STATE
- resumen de estructura
- problemas o bloqueos clave

ACTION
- cambios realizados o recomendados

FOLLOW-UPS
- archivar / fusionar / limpiar duplicados / siguiente archivo a actualizar
```

## Casos de uso buenos

- “Encuentra el documento de planificación activo y condénsalo”
- “Limpia esta hoja de clientes y muéstrame las filas de riesgo de churn”
- “Importa este deck a Slides y déjalo presentable”
- “Encuentra el tracker actual, no el duplicado obsoleto”
