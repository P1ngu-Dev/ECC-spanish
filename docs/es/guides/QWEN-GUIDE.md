# Guía del adaptador CLI de Qwen

ECC puede instalar sus superficies administradas de comandos, agentes, skills, reglas y MCP en el directorio principal de Qwen CLI.

## Instalar

Desde la raíz del repositorio de ECC:

```bash
./install.sh --target qwen --profile minimal
```

Previsualiza una instalación más grande antes de copiar archivos:

```bash
./install.sh --target qwen --profile full --dry-run
```

El adaptador de Qwen escribe en `~/.qwen/` y registra la propiedad de archivos administrados en `~/.qwen/ecc-install-state.json`.

## Distribución instalada

La instalación administrada puede poblar:

```text
~/.qwen/
  QWEN.md
  agents/
  commands/
  mcp-configs/
  rules/
  skills/
  ecc-install-state.json
```

El instalador preserva la estructura de origen para las reglas, por lo que los conjuntos de reglas por idioma permanecen en rutas como `~/.qwen/rules/common/` y `~/.qwen/rules/typescript/`.

## Actualización

Vuelve a ejecutar el mismo comando de instalación después de obtener actualizaciones de ECC. El instalador usa el archivo de estado de instalación para actualizar los archivos administrados por ECC sin reclamar archivos de usuario no relacionados en `~/.qwen/`.

## Desinstalación

Usa la ruta de desinstalación administrada en lugar de borrar todo el directorio de Qwen:

```bash
node scripts/uninstall.js --target qwen
```

Eso elimina los archivos registrados en `~/.qwen/ecc-install-state.json` y deja intacta la configuración no relacionada de Qwen.

## Alcance

Este destino es intencionalmente más estrecho que la PR obsoleta #1352. Adapta la intención mantenible del objetivo de instalación de Qwen al instalador selectivo actual y evita afirmaciones no verificadas sobre el tiempo de ejecución de hooks hasta que se confirme el contrato de hooks/eventos de Qwen.
