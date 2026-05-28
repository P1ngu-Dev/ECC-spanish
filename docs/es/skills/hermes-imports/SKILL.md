---
name: hermes-imports
description: Convert local Hermes operator workflows into sanitized ECC skills and release-pack artifacts. Use when preparing a Hermes workflow for public ECC reuse without leaking private workspace state, credentials, or local-only paths.
origin: ECC
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Importaciones de Hermes

Usa este skill cuando conviertas un flujo repetido de Hermes en algo seguro para publicar en ECC.

Hermes es la shell del operador. ECC es la capa de flujo reutilizable. Las importaciones deben mover patrones estables de Hermes a ECC sin mover estado privado.

## Cuándo usarlo

- Un flujo de Hermes se ha repetido lo suficiente como para volverse reutilizable.
- Un prompt local del operador debe convertirse en un skill público de ECC.
- Una entrega de lanzamiento, contenido, investigación o ingeniería necesita docs de handoff sanitizados.
- Un flujo menciona rutas locales, credenciales, datasets personales o nombres de cuentas privadas que deben eliminarse antes de publicar.

## Reglas de importación

- Convierte rutas locales a rutas relativas al repo o a placeholders.
- Sustituye nombres de cuentas reales por etiquetas de rol como `operator`, `default profile` o `workspace owner`.
- Describe los requisitos de credenciales solo por nombre del proveedor.
- Mantén los ejemplos concretos y operativos.
- No publiques exports crudos del workspace, tokens, archivos OAuth, datos de salud, CRM ni finanzas.
- Si el flujo necesita estado privado para tener sentido, déjalo local.

## Lista de sanitización

Antes de commitear un flujo importado, busca:

- rutas absolutas como `/Users/...`
- rutas `~/.hermes` salvo que el doc explique explícitamente la configuración local
- API keys, tokens, cookies, archivos OAuth o bearer strings
- números de teléfono, emails privados y grafos de contactos personales
- nombres de clientes, familiares o cuentas que no sean públicos
- datos de ingresos, salud o CRM
- logs crudos que incluyan salida de herramientas de sistemas privados

## Patrón de conversión

1. Identifica el bucle del operador que se repite.
2. Elimina entradas y salidas privadas.
3. Reescribe rutas locales como ejemplos relativos al repo.
4. Convierte instrucciones puntuales en una sección `When To Use` y un proceso corto.
5. Añade requisitos concretos de salida.
6. Ejecuta un escaneo de secretos y rutas locales antes de abrir un PR.

## Ejemplo: handoff de lanzamiento

Prompt local de Hermes:

```text
Read my local workspace files and finalize launch copy.
```

Versión segura para ECC:

```text
Use the public release pack under docs/releases/<version>/.
Return one X thread, one LinkedIn post, one recording checklist, and the missing assets list.
```

## Ejemplo: tarea de operador en horas tranquilas

Trabajo local de Hermes:

```text
Run my private inbox, finance, and content checks overnight.
```

Versión segura para ECC:

```text
Describe the scheduler policy, the quiet-hours window, the escalation rules, and the categories of checks. Do not include private data sources or credentials.
```

## Contrato de salida

Devuelve:

- nombre candidato del skill ECC
- resumen sanitizado del flujo
- entradas públicas requeridas
- entradas privadas eliminadas
- riesgos restantes
- archivos que deben crearse o actualizarse
