Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Guía de uso y configuración de antigravedad

[Antigravity](https://antigravity.dev) de Google es un IDE de codificación de IA que utiliza una convención de directorio `.agent/` para la configuración. ECC proporciona soporte de primera clase para Antigravity a través de su sistema de instalación selectiva.

## Inicio rápido

```bash
# Instalar ECC con el destino Antigravity
./install.sh --target antigravity typescript

# O con varios módulos de lenguaje
./install.sh --target antigravity typescript python go
```

Esto instala los componentes ECC en el directorio `.agent/` de su proyecto, listos para que Antigravity los retome.

## Cómo funciona el mapeo de instalación

ECC reasigna la estructura de sus componentes para que coincida con el diseño esperado de Antigravity:

| ECC Fuente | Destino antigravedad | Qué contiene |
|------------|------------------------|------------------|
| `rules/` | `.agent/rules/` | Reglas del lenguaje y estándares de codificación (aplanados) |
| `commands/` | `.agent/workflows/` | Barra oblicua commands se convierte en Antigravedad workflows |
| `agents/` | `.agent/skills/` | Las definiciones de agentes se convierten en Antigravedad skills |

> **Nota sobre `.agents/` vs `.agent/` vs `agents/`**: El instalador solo maneja tres rutas de origen explícitamente: `rules` → `.agent/rules/`, `commands` → `.agent/workflows/` y `agents` (sin prefijo de punto) → `.agent/skills/`. El directorio `.agents/` con prefijo de punto en ECC repo es un **diseño estático** para las definiciones de habilidades Codex/Antigravity y las configuraciones `openai.yaml`; el instalador no lo asigna directamente. Cualquier ruta `.agents/` pasa a la operación de andamio predeterminada. Si desea que el contenido `.agents/skills/` esté disponible en el tiempo de ejecución de Antigravity, debe copiarlo manualmente en `.agent/skills/`.

### Diferencias clave con Claude Code

- **Las reglas están aplanadas**: Claude Code anida reglas en subdirectorios (`rules/common/`, `rules/typescript/`). Antigravity espera un directorio plano `rules/`; el instalador lo maneja automáticamente.
- **Los comandos se convierten en workflows**: los archivos `/command` de ECC aterrizan en `.agent/workflows/`, que es el equivalente de Antigravity a la barra diagonal commands.
- **Los agentes se convierten en skills**: ECC las definiciones de agentes se asignan a `.agent/skills/`, donde Antigravity busca configuraciones de habilidades.

## Estructura del directorio después de la instalación

```
your-project/
├── .agent/
│   ├── rules/
│   │   ├── coding-standards.md
│   │   ├── testing.md
│   │   ├── security.md
│   │   └── typescript.md          # language-specific rules
│   ├── workflows/
│   │   ├── plan.md
│   │   ├── code-review.md
│   │   ├── tdd.md
│   │   └── ...
│   ├── skills/
│   │   ├── planner.md
│   │   ├── code-reviewer.md
│   │   ├── tdd-guide.md
│   │   └── ...
│   └── ecc-install-state.json     # tracks what ECC installed
```

## La configuración del agente `openai.yaml`

Cada directorio de habilidades en `.agents/skills/` contiene un archivo `agents/openai.yaml` en la ruta `.agents/skills/<skill-name>/agents/openai.yaml` que configura la habilidad para Antigravity:

```yaml
interface:
  display_name: "API Design"
  short_description: "REST API design patterns and best practices"
  brand_color: "#F97316"
  default_prompt: "Design REST API: resources, status codes, pagination"
policy:
  allow_implicit_invocation: true
```

| Campo | Objetivo |
|-------|---------|
| `display_name` | Nombre legible por humanos mostrado en la interfaz de usuario de Antigravity |
| `short_description` | Breve descripción de lo que hace la habilidad. |
| `brand_color` | Color hexadecimal para la insignia visual de la habilidad. |
| `default_prompt` | Mensaje sugerido cuando la habilidad se invoca manualmente |
| `allow_implicit_invocation` | Cuando `true`, Antigravity puede activar la habilidad automáticamente según el contexto. |

## Administrar su instalación

### Verifique lo que está instalado

```bash
node scripts/list-installed.js --target antigravity
```

### Reparar una instalación rota

```bash
# First, diagnose what's wrong
node scripts/doctor.js --target antigravity

# Then, restore missing or drifted files
node scripts/repair.js --target antigravity
```

### Desinstalar

```bash
node scripts/uninstall.js --target antigravity
```

### Estado de instalación

El instalador escribe `.agent/ecc-install-state.json` para rastrear qué archivos posee ECC. Esto permite la desinstalación y reparación seguras: ECC nunca tocará archivos que no creó.

## Agregar habilidades personalizadas para antigravedad

Si estás aportando una nueva habilidad y quieres que esté disponible en Antigravity:

1. Cree la habilidad en `skills/your-skill-name/SKILL.md` como de costumbre.
2. Agregue una definición de agente en `agents/your-skill-name.md`: esta es la ruta que el instalador asigna a `.agent/skills/` en tiempo de ejecución, lo que hace que su habilidad esté disponible en Antigravity harness
3. Agregue la configuración del agente Antigravity en `.agents/skills/your-skill-name/agents/openai.yaml`: este es un diseño estático repo consumido por Codex para metadatos de invocación implícita
4. Refleje el contenido de `SKILL.md` en `.agents/skills/your-skill-name/SKILL.md`; esta copia estática la utiliza Codex y sirve como referencia para Antigravity.
5. Mencione en su PR que agregó soporte antigravedad.

> **Distinción clave**: El instalador implementa `agents/` (sin punto) → `.agent/skills/`: esto es lo que hace que skills esté disponible en tiempo de ejecución. El directorio `.agents/` (con prefijo de punto) es un diseño estático separado para las configuraciones Codex `openai.yaml` y el instalador no lo implementa automáticamente.

Consulte [CONTRIBUTING.md](../../CONTRIBUTING.md) para obtener la guía de contribución completa.

## Comparación con otros objetivos

| Característica | Claude Code | Cursor | Codex | Antigravedad |
|---------|-------------|--------|-------|-------------|
| Instalar destino | `claude-home` | `cursor-project` | `codex-home` | `antigravity` |
| raíz de configuración | `~/.claude/` | `.cursor/` | `~/.codex/` | `.agent/` |
| Alcance | Nivel de usuario | Nivel de proyecto | Nivel de usuario | Nivel de proyecto |
| Formato de reglas | directorios anidados | Departamento | Departamento | Departamento |
| Comandos | `commands/` | N / A | N / A | `workflows/` |
| Agentes/Habilidades | `agents/` | N / A | N / A | `skills/` |
| Estado de instalación | `ecc-install-state.json` | `ecc-install-state.json` | `ecc-install-state.json` | `ecc-install-state.json` |

## Solución de problemas

### Las habilidades no se cargan en Antigravity

- Verifique que el directorio `.agent/` exista en la raíz de su proyecto (no en el directorio de inicio)
- Verifique que se creó `ecc-install-state.json`; si falta, vuelva a ejecutar el instalador
- Asegúrese de que los archivos tengan la extensión `.md` y un texto inicial válido

### Reglas que no se aplican

- Las reglas deben estar en `.agent/rules/`, no anidadas en subdirectorios
- Ejecute `node scripts/doctor.js --target antigravity` para verificar la instalación.

### Flujos de trabajo no disponibles

- Antigravedad busca workflows en `.agent/workflows/`, no `commands/`
- Si copió manualmente ECC commands, cambie el nombre del directorio

## Recursos relacionados

- [Arquitectura de instalación selectiva](../../docs/SELECTIVE-INSTALL-ARCHITECTURE.md): cómo funciona el sistema de instalación bajo el capó
- [Diseño de instalación selectiva](../../docs/SELECTIVE-INSTALL-DESIGN.md): decisiones de diseño y contratos de adaptadores de destino
- [CONTRIBUTING.md](../../CONTRIBUTING.md) — cómo contribuir skills, agents y commands
