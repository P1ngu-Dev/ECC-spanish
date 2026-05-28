# Quickstart de ECC v2.0.0-rc.1

Esta ruta es para una persona contribuidora nueva que quiera verificar la superficie de release antes de tocar trabajo de características.

## Clonar

```bash
git clone https://github.com/affaan-m/ECC.git
cd ECC
```

Empieza desde un checkout limpio. No copies al repo estado privado del operador, exportaciones brutas del workspace, tokens ni archivos locales de Hermes.

## Instalar

```bash
npm ci
```

Esto instala la herramienta de validación y empaquetado basada en Node que usa la superficie pública de release.

## Verificar

```bash
node tests/run-all.js
```

Resultado esperado: todos los tests pasan sin fallos. Para el drift específico de release, ejecuta la comprobación focalizada:

```bash
node tests/docs/ecc2-release-surface.test.js
```

Luego revisa la superficie local de observabilidad:

```bash
npm run observability:ready
```

Esto ejecuta el [gate de preparación de observabilidad](../../architecture/observability-readiness.md) para estado de bucle, trazas de sesión, auditoría del harness y registros de riesgo de herramientas ECC2. <!-- TODO: no existe equivalente traducido; consulta la versión en inglés -->

## Primera skill

Lee primero `skills/hermes-imports/SKILL.md`.

Muestra el patrón previsto de ECC 2.0:

- tomar un workflow de operador repetido
- eliminar credenciales, rutas privadas, exportaciones brutas del workspace y memoria personal
- conservar la forma durable del workflow
- publicar el resultado saneado como un `SKILL.md` reutilizable

No empieces importando un workflow privado de Hermes en bloque. Empieza destilando una sola skill reutilizable.

## Cambiar de harness

Usa la misma fuente de skill en todos los harnesses:

- Claude Code consume ECC mediante el plugin de Claude y los hooks nativos.
- Codex consume ECC mediante `AGENTS.md`, `.codex-plugin/plugin.json` y la configuración MCP de referencia.
- OpenCode consume ECC mediante la superficie de paquete/plugin de OpenCode.

La unidad portátil sigue siendo `skills/*/SKILL.md`. Los archivos específicos del harness deberían cargar o adaptar esa fuente, no redefinir el workflow.

## Próxima documentación

- [Configuración de Hermes](../../HERMES-SETUP.md) <!-- TODO: no existe equivalente traducido; consulta la versión en inglés -->
- [Arquitectura entre harnesses](../../architecture/cross-harness.md) <!-- TODO: no existe equivalente traducido; consulta la versión en inglés -->
- [Notas de la versión](release-notes.md)
