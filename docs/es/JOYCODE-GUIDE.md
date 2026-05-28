# Guía del adaptador de JoyCode

JoyCode puede consumir ECC mediante el instalador selectivo. El adaptador instala comandos, agentes, skills y reglas aplanadas compartidas de ECC en un directorio `.joycode/` local al proyecto.

## Instalación

Previsualiza el plan de instalación:

```bash
node scripts/install-plan.js --target joycode --profile full
```

Aplica esto al proyecto actual:

```bash
node scripts/install-apply.js --target joycode --profile full
```

Para una instalación más pequeña, selecciona módulos explícitamente:

```bash
node scripts/install-apply.js --target joycode --modules rules-core,commands-core,workflow-quality
```

## Estructura

El adaptador del proyecto escribe archivos administrados en:

```text
.joycode/
  agents/
  commands/
  rules/
  skills/
  mcp-configs/
  scripts/
  ecc-install-state.json
```

Las reglas se aplanan en nombres de archivo con espacio de nombres para que un proyecto de JoyCode no reciba directorios anidados de reglas como `rules/common/coding-style.md`. Los comandos, agentes y skills conservan la misma estructura que usan en el resto de ECC.
El perfil completo también incluye archivos compartidos de MCP y de ayuda para configuración que usan otros adaptadores locales por proyecto de ECC.

## Desinstalación

Usa la ruta de desinstalación administrada de ECC en lugar de borrar archivos manualmente:

```bash
node scripts/uninstall.js --target joycode
```

El comando de desinstalación lee `.joycode/ecc-install-state.json` y elimina solo los archivos que instaló ECC. Los archivos de JoyCode creados por el usuario se conservan.

## PR de origen

Este adaptador rescata la intención útil de JoyCode local al proyecto desde el PR obsoleto #1429, mientras reemplaza el instalador independiente en shell por la maquinaria actual de ECC para estado de instalación y desinstalación.
