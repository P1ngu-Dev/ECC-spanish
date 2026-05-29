# Contribuir a Everything Claude Code

Gracias por querer contribuir. Este repositorio es un recurso comunitario para usuarios de Claude Code.

## Tabla de contenidos

- [Lo que buscamos](#lo-que-buscamos)
- [Inicio rápido](#inicio-rápido)
- [Contribuir skills](#contribuir-skills)
- [Política de adaptación de skills](#política-de-adaptación-de-skills)
- [Contribuir agents](#contribuir-agents)
- [Contribuir hooks](#contribuir-hooks)
- [Contribuir comandos](#contribuir-comandos)
- [MCP y documentación (por ejemplo, Context7)](#mcp-y-documentación-por-ejemplo-context7)
- [Entre harnesses y traducciones](#entre-harnesses-y-traducciones)
- [Proceso de pull request](#proceso-de-pull-request)

---

## Lo que buscamos

### Agents
Nuevos agents que resuelvan bien tareas específicas:
- reviewers específicos por lenguaje (Python, Go, Rust)
- expertos en frameworks (Django, Rails, Laravel, Spring)
- especialistas DevOps (Kubernetes, Terraform, CI/CD)
- expertos de dominio (pipelines de ML, ingeniería de datos, mobile)

### Skills
Definiciones de flujos de trabajo y conocimiento de dominio:
- mejores prácticas de lenguaje
- patrones de framework
- estrategias de testing
- guías de arquitectura

### Hooks
Automatizaciones útiles:
- hooks de lint/formato
- comprobaciones de seguridad
- hooks de validación
- hooks de notificación

### Comandos
Comandos con slash que invoquen flujos de trabajo útiles:
- comandos de despliegue
- comandos de testing
- comandos de generación de código

---

## Inicio rápido

```bash
# 1. Fork and clone
gh repo fork affaan-m/everything-claude-code --clone
cd everything-claude-code

# 2. Create a branch
git checkout -b feat/my-contribution

# 3. Add your contribution (see sections below)

# 4. Test locally
cp -r skills/my-skill ~/.claude/skills/  # for skills
# Then test with Claude Code

# 5. Submit PR
git add . && git commit -m "feat: add my-skill" && git push -u origin feat/my-contribution
```

---

## Contribuir skills

Las skills son módulos de conocimiento que Claude Code carga según el contexto.

> **Guía completa:** para orientación detallada sobre cómo crear skills efectivas, consulta la [guía de desarrollo de skills](../../docs/SKILL-DEVELOPMENT-GUIDE.md). Cubre:
> - arquitectura y categorías de skills
> - redacción efectiva de contenido con ejemplos
> - mejores prácticas y patrones comunes
> - testing y validación
> - galería completa de ejemplos

### Estructura de directorios

```
skills/
└── your-skill-name/
    └── SKILL.md
```

### Plantilla de `SKILL.md`

```markdown
---
name: your-skill-name
description: Brief description shown in skill list and used for auto-activation
origin: ECC
---

# Your Skill Title

Brief overview of what this skill covers.

## When to Activate

Describe scenarios where Claude should use this skill. This is critical for auto-activation.

## Core Concepts

Explain key patterns and guidelines.

## Code Examples

\`\`\`typescript
// Include practical, tested examples
function example() {
  // Well-commented code
}
\`\`\`

## Anti-Patterns

Show what NOT to do with examples.

## Best Practices

- Actionable guidelines
- Do's and don'ts
- Common pitfalls to avoid

## Related Skills

Link to complementary skills (e.g., `related-skill-1`, `related-skill-2`).
```

### Categorías de skills

| Categoría | Propósito | Ejemplos |
|----------|-----------|----------|
| **Language Standards** | Idiomas, convenciones y mejores prácticas | `python-patterns`, `golang-patterns` |
| **Framework Patterns** | Guía específica de framework | `django-patterns`, `nextjs-patterns` |
| **Workflow** | Procesos paso a paso | `tdd-workflow`, `refactoring-workflow` |
| **Domain Knowledge** | Dominios especializados | `security-review`, `api-design` |
| **Tool Integration** | Uso de herramientas/librerías | `docker-patterns`, `supabase-patterns` |
| **Template** | Plantillas específicas de proyecto | `docs/examples/project-guidelines-template.md` |

### Política de adaptación de skills

Si estás portando una idea desde otro repo, plugin, harness o pack personal de prompts, lee la [política de adaptación de skills](../../docs/skill-adaptation-policy.md) antes de abrir el PR.

Versión corta:

- copia la idea subyacente, no la identidad del producto externo
- renombra la skill cuando ECC cambie o amplíe materialmente la superficie
- prefiere rules, skills, scripts y MCPs nativos de ECC frente a nuevas dependencias externas por defecto
- no publiques una skill cuyo principal valor sea decirle a la gente que instale un paquete no evaluado

### Checklist de skills

- [ ] Enfocada en un solo dominio/tecnología (no demasiado amplia)
- [ ] Incluye sección "When to Activate" para autoactivación
- [ ] Incluye ejemplos de código prácticos y copiables
- [ ] Muestra anti-patrones (qué NO hacer)
- [ ] Menos de 500 líneas (máximo 800)
- [ ] Usa encabezados claros
- [ ] Probada con Claude Code
- [ ] Enlaza skills relacionadas
- [ ] No contiene datos sensibles (API keys, tokens, rutas)
- [ ] El frontmatter declara `name:` con el mismo valor que el directorio
- [ ] El `description:` del frontmatter es inline o folded (`>`), no bloque literal (`|`, `|-`, `|+`)

### Ejemplos de skills

| Skill | Categoría | Propósito |
|-------|-----------|-----------|
| `coding-standards/` | Language Standards | Patrones de TypeScript/JavaScript |
| `frontend-patterns/` | Framework Patterns | Mejores prácticas de React y Next.js |
| `backend-patterns/` | Framework Patterns | Patrones de API y base de datos |
| `security-review/` | Domain Knowledge | Checklist de seguridad |
| `tdd-workflow/` | Workflow | Proceso de desarrollo guiado por pruebas |
| `docs/examples/project-guidelines-template.md` | Template | Plantilla de guía específica de proyecto |

---

## Contribuir agents

Los agents son asistentes especializados invocados mediante la herramienta `Task`.

### Ubicación del archivo

```
agents/your-agent-name.md
```

### Plantilla de agent

```markdown
---
name: your-agent-name
description: What this agent does and when Claude should invoke it. Be specific!
tools: ["Read", "Write", "Edit", "Bash", "Grep", "Glob"]
model: sonnet
---

You are a [role] specialist.

## Your Role

- Primary responsibility
- Secondary responsibility
- What you DO NOT do (boundaries)

## Workflow

### Step 1: Understand
How you approach the task.

### Step 2: Execute
How you perform the work.

### Step 3: Verify
How you validate results.

## Output Format

What you return to the user.

## Examples

### Example: [Scenario]
Input: [what user provides]
Action: [what you do]
Output: [what you return]
```

### Campos del agent

| Campo | Descripción | Opciones |
|-------|-------------|----------|
| `name` | Minúsculas, con guiones | `code-reviewer` |
| `description` | Se usa para decidir cuándo invocarlo | Sé específico |
| `tools` | Solo lo necesario | `Read, Write, Edit, Bash, Grep, Glob, WebFetch, Task`, o herramientas MCP |
| `model` | Nivel de complejidad | `haiku`, `sonnet`, `opus` |

### Ejemplos de agents

| Agent | Propósito |
|-------|-----------|
| `tdd-guide.md` | Desarrollo guiado por pruebas |
| `code-reviewer.md` | Revisión de código |
| `security-reviewer.md` | Escaneo de seguridad |
| `build-error-resolver.md` | Corregir errores de build |

---

## Contribuir hooks

Los hooks son comportamientos automáticos activados por eventos de Claude Code.

### Ubicación del archivo

```
hooks/hooks.json
```

### Tipos de hook

| Tipo | Disparador | Caso de uso |
|------|------------|-------------|
| `PreToolUse` | Antes de ejecutar la herramienta | Validar, advertir, bloquear |
| `PostToolUse` | Después de ejecutar la herramienta | Formatear, comprobar, notificar |
| `SessionStart` | Al comenzar la sesión | Cargar contexto |
| `Stop` | Al terminar la sesión | Limpiar, auditar |

### Formato de hook

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "tool == \"Bash\" && tool_input.command matches \"rm -rf /\"",
        "hooks": [
          {
            "type": "command",
            "command": "echo '[Hook] BLOCKED: Dangerous command' && exit 1"
          }
        ],
        "description": "Block dangerous rm commands"
      }
    ]
  }
}
```

### Sintaxis de matcher

```javascript
// Match specific tools
tool == "Bash"
tool == "Edit"
tool == "Write"

// Match input patterns
tool_input.command matches "npm install"
tool_input.file_path matches "\\.tsx?$"

// Combine conditions
tool == "Bash" && tool_input.command matches "git push"
```

### Ejemplos de hook

```json
// Block dev servers outside tmux
{
  "matcher": "tool == \"Bash\" && tool_input.command matches \"npm run dev\"",
  "hooks": [{"type": "command", "command": "echo 'Use tmux for dev servers' && exit 1"}],
  "description": "Ensure dev servers run in tmux"
}

// Auto-format after editing TypeScript
{
  "matcher": "tool == \"Edit\" && tool_input.file_path matches \"\\.tsx?$\"",
  "hooks": [{"type": "command", "command": "npx prettier --write \"$file_path\""}],
  "description": "Format TypeScript files after edit"
}

// Warn before git push
{
  "matcher": "tool == \"Bash\" && tool_input.command matches \"git push\"",
  "hooks": [{"type": "command", "command": "echo '[Hook] Review changes before pushing'"}],
  "description": "Reminder to review before push"
}
```

### Checklist de hooks

- [ ] El matcher es específico (no demasiado amplio)
- [ ] Incluye mensajes claros de error/info
- [ ] Usa códigos de salida correctos (`exit 1` bloquea, `exit 0` permite)
- [ ] Probado a fondo
- [ ] Tiene descripción

---

## Contribuir comandos

Los comandos son acciones invocadas por el usuario con `/command-name`.

### Ubicación del archivo

```
commands/your-command.md
```

### Plantilla de comando

```markdown
---
description: Brief description shown in /help
---

# Command Name

## Purpose

What this command does.

## Usage

\`\`\`
/your-command [args]
\`\`\`

## Workflow

1. First step
2. Second step
3. Final step

## Output

What the user receives.
```

### Ejemplos de comandos

| Comando | Propósito |
|---------|-----------|
| `commit.md` | Crear commits de git |
| `code-review.md` | Revisar cambios de código |
| `tdd.md` | Flujo TDD |
| `e2e.md` | Testing E2E |

---

## MCP y documentación (por ejemplo, Context7)

Las skills y los agents pueden usar herramientas **MCP (Model Context Protocol)** para obtener datos actualizados en lugar de depender solo del entrenamiento. Esto es especialmente útil para documentación.

- **Context7** es un servidor MCP que expone `resolve-library-id` y `query-docs`. Úsalo cuando el usuario pregunte por librerías, frameworks o APIs para que las respuestas reflejen documentación actual y ejemplos de código.
- Cuando contribuyas **skills** que dependan de documentación en vivo (por ejemplo, setup o uso de API), describe cómo usar las herramientas MCP relevantes y apunta a la skill `documentation-lookup` o a Context7 como patrón.
- Cuando contribuyas **agents** que respondan preguntas de docs/API, incluye los nombres de herramientas MCP de Context7 en las tools del agent y documenta el flujo resolve → query.
- **`mcp-configs/mcp-servers.json`** incluye una entrada de Context7; los usuarios la habilitan en su harness para usar la skill `documentation-lookup` y el comando `/docs`.

---

## Entre harnesses y traducciones

### Subconjuntos de skills (Codex y Cursor)

ECC distribuye subconjuntos de skills para otros harnesses:

- **Codex:** `.agents/skills/` — las skills listadas en `agents/openai.yaml` se cargan en Codex.
- **Cursor:** `.cursor/skills/` — se incluye un subconjunto de skills para Cursor.

Cuando **añadas una skill nueva** que deba estar disponible en Codex o Cursor:

1. Añade la skill bajo `skills/your-skill-name/` como siempre.
2. Si debe estar disponible en **Codex**, añádela en `.agents/skills/` y asegúrate de que esté referenciada en `agents/openai.yaml` si hace falta.
3. Si debe estar disponible en **Cursor**, añádela en `.cursor/skills/` según el layout de Cursor.

Revisa las skills existentes en esos directorios para ver la estructura esperada. Mantener estos subconjuntos sincronizados es manual; menciónalo en tu PR si los actualizaste.

### Traducciones

Las traducciones viven bajo `docs/` (por ejemplo, `docs/zh-CN`, `docs/zh-TW`, `docs/ja-JP`, `docs/es`). Si cambias agents, commands o skills que estén traducidos, considera actualizar los archivos de traducción correspondientes o abrir un issue para que maintainers o traductores puedan actualizarlos.

---

## Proceso de pull request

### 1. Formato del título del PR

```
feat(skills): add rust-patterns skill
feat(agents): add api-designer agent
feat(hooks): add auto-format hook
fix(skills): update React patterns
docs: improve contributing guide
```

### 2. Descripción del PR

```markdown
## Summary
What you're adding and why.

## Type
- [ ] Skill
- [ ] Agent
- [ ] Hook
- [ ] Command

## Testing
How you tested this.

## Checklist
- [ ] Follows format guidelines
- [ ] Tested with Claude Code
- [ ] No sensitive info (API keys, paths)
- [ ] Clear descriptions
```

### 3. Proceso de revisión

1. Los maintainers revisan dentro de 48 horas.
2. Atiende feedback si lo piden.
3. Una vez aprobado, se fusiona a `main`.

---

## Directrices

### Haz
- Mantén las contribuciones enfocadas y modulares
- Incluye descripciones claras
- Prueba antes de enviar
- Sigue los patrones existentes
- Documenta dependencias

### No hagas
- Incluir información sensible (API keys, tokens, rutas)
- Añadir configuraciones demasiado complejas o de nicho
- Enviar contribuciones sin probar
- Crear duplicados de funcionalidad existente

---

## Nombres de archivos

- Usa minúsculas y guiones: `python-reviewer.md`
- Sé descriptivo: `tdd-workflow.md` y no `workflow.md`
- Haz coincidir el nombre con el nombre del archivo

---

## ¿Preguntas?

- **Issues:** [github.com/affaan-m/everything-claude-code/issues](https://github.com/affaan-m/everything-claude-code/issues)
- **X/Twitter:** [@affaanmustafa](https://x.com/affaanmustafa)

---

Gracias por contribuir. Construyamos un gran recurso juntos.
