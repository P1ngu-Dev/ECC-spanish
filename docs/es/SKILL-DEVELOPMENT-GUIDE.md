# Guía de desarrollo de skills

Una guía integral para crear skills efectivas para Everything Claude Code (ECC).

## Tabla de contenidos

- [¿Qué son las skills?](#qué-son-las-skills)
- [Arquitectura de skills](#arquitectura-de-skills)
- [Crear tu primera skill](#crear-tu-primera-skill)
- [Categorías de skills](#categorías-de-skills)
- [Escribir contenido efectivo para una skill](#escribir-contenido-efectivo-para-una-skill)
- [Mejores prácticas](#mejores-prácticas)
- [Patrones comunes](#patrones-comunes)
- [Probar tu skill](#probar-tu-skill)
- [Enviar tu skill](#enviar-tu-skill)
- [Galería de ejemplos](#galería-de-ejemplos)

---

## ¿Qué son las skills?

Las skills son **módulos de conocimiento** que Claude Code carga según el contexto. Proporcionan:

- **Experiencia de dominio**: patrones de frameworks, idiomatismos del lenguaje, mejores prácticas
- **Definiciones de flujo de trabajo**: procesos paso a paso para tareas comunes
- **Material de referencia**: fragmentos de código, listas de verificación, árboles de decisión
- **Inyección de contexto**: se activan cuando se cumplen condiciones específicas

A diferencia de los **agents** (subasistentes especializados) o los **commands** (acciones iniciadas por el usuario), las skills son conocimiento pasivo que Claude Code consulta cuando es relevante.

### Cuándo se activan las skills

Las skills se activan cuando:
- La tarea del usuario coincide con el dominio de la skill
- Claude Code detecta contexto relevante
- Un comando referencia una skill
- Un agent necesita conocimiento de dominio

### Skill vs Agent vs Command

| Componente | Propósito | Activación |
|-----------|---------|------------|
| **Skill** | Repositorio de conocimiento | Basada en contexto (automática) |
| **Agent** | Ejecutador de tareas | Delegación explícita |
| **Command** | Acción del usuario | Iniciado por el usuario (`/command`) |
| **Hook** | Automatización | Disparado por eventos |
| **Rule** | Guías siempre activas | Siempre activa |

---

## Arquitectura de skills

### Estructura de archivos

```
skills/
└── your-skill-name/
    ├── SKILL.md           # Requerido: definición principal de la skill
    ├── examples/          # Opcional: ejemplos de código
    │   ├── basic.ts
    │   └── advanced.ts
    └── references/        # Opcional: referencias externas
        └── links.md
```

### Formato de SKILL.md

```markdown
---
name: skill-name
description: Breve descripción mostrada en la lista de skills y usada para la autoactivación
origin: ECC
---

# Título de la skill

Resumen breve de lo que cubre esta skill.

## Cuándo activar

Describe escenarios en los que Claude debe usar esta skill.

## Conceptos clave

Patrones y pautas principales.

## Ejemplos de código

\`\`\`typescript
// Ejemplos prácticos y probados
\`\`\`

## Anti-patrones

Muestra qué NO hacer con ejemplos concretos.

## Mejores prácticas

- Pautas accionables
- Lo que sí y lo que no

## Skills relacionadas

Enlaza skills complementarias.
```

### Campos del frontmatter YAML

| Campo | Requerido | Descripción |
|-------|----------|-------------|
| `name` | Sí | Identificador en minúsculas, con guiones (por ejemplo, `react-patterns`) |
| `description` | Sí | Descripción en una línea para la lista de skills y la autoactivación |
| `origin` | No | Identificador de origen (por ejemplo, `ECC`, `community`, nombre del proyecto) |
| `tags` | No | Arreglo de etiquetas para categorización |
| `version` | No | Versión de la skill para seguimiento de actualizaciones |

---

## Crear tu primera skill

### Paso 1: Elige un enfoque

Las buenas skills son **enfocadas y accionables**:

| PASS: Enfoque bueno | FAIL: Demasiado amplio |
|---------------|--------------|
| `react-hook-patterns` | `react` |
| `postgresql-indexing` | `databases` |
| `pytest-fixtures` | `python-testing` |
| `nextjs-app-router` | `nextjs` |

### Paso 2: Crea el directorio

```bash
mkdir -p skills/your-skill-name
```

### Paso 3: Escribe SKILL.md

Aquí tienes una plantilla mínima:

```markdown
---
name: your-skill-name
description: Breve descripción de cuándo usar esta skill
---

# Título de tu skill

Resumen breve (1-2 oraciones).

## Cuándo activar

- Escenario 1
- Escenario 2
- Escenario 3

## Conceptos clave

### Concepto 1

Explicación con ejemplos.

### Concepto 2

Otro patrón con código.

## Ejemplos de código

\`\`\`typescript
// Ejemplo práctico
\`\`\`

## Mejores prácticas

- Haz esto
- Evita esto

## Skills relacionadas

- `related-skill-1`
- `related-skill-2`
```

### Paso 4: Agrega contenido

Escribe contenido que Claude pueda **usar de inmediato**:

- PASS: Ejemplos de código que se pueden copiar y pegar
- PASS: Árboles de decisión claros
- PASS: Listas de verificación para validación
- FAIL: Explicaciones vagas sin ejemplos
- FAIL: Prosa larga sin orientación accionable

---

## Categorías de skills

### Estándares de lenguaje

Enfócate en código idiomático, convenciones de nombres y patrones específicos del lenguaje.

**Ejemplos:** `python-patterns`, `golang-patterns`, `typescript-standards`

```markdown
---
name: python-patterns
description: Idiomas de Python, mejores prácticas y patrones para código limpio e idiomático.
---

# Patrones de Python

## Cuándo activar

- Escribir código Python
- Refactorizar módulos Python
- Revisar código Python

## Conceptos clave

### Administradores de contexto

\`\`\`python
# Usa siempre administradores de contexto para recursos
with open('file.txt') as f:
    content = f.read()
\`\`\`
```

### Patrones de framework

Enfócate en convenciones específicas del framework, patrones comunes y anti-patrones.

**Ejemplos:** `django-patterns`, `nextjs-patterns`, `springboot-patterns`

```markdown
---
name: django-patterns
description: Mejores prácticas de Django para modelos, vistas, URLs y plantillas.
---

# Patrones de Django

## Cuándo activar

- Construir aplicaciones Django
- Crear modelos y vistas
- Configuración de URLs en Django
```

### Skills de flujo de trabajo

Define procesos paso a paso para tareas comunes de desarrollo.

**Ejemplos:** `tdd-workflow`, `code-review-workflow`, `deployment-checklist`

```markdown
---
name: code-review-workflow
description: Proceso sistemático de revisión de código para calidad y seguridad.
---

# Flujo de trabajo de revisión de código

## Pasos

1. **Entender el contexto** - Lee la descripción del PR y los issues vinculados
2. **Revisar pruebas** - Verifica cobertura y calidad de las pruebas
3. **Revisar la lógica** - Analiza la implementación en busca de corrección
4. **Revisar seguridad** - Busca vulnerabilidades
5. **Verificar estilo** - Asegúrate de que el código siga las convenciones
```

### Conocimiento de dominio

Conocimiento especializado para dominios específicos (seguridad, rendimiento, etc.).

**Ejemplos:** `security-review`, `performance-optimization`, `api-design`

```markdown
---
name: api-design
description: Patrones de diseño de APIs REST y GraphQL, versionado y mejores prácticas.
---

# Patrones de diseño de API

## Convenciones RESTful

| Método | Endpoint | Propósito |
|--------|----------|---------|
| GET | /resources | Listar todos |
| GET | /resources/:id | Obtener uno |
| POST | /resources | Crear |
```

### Integración de herramientas

Guía para usar herramientas, bibliotecas o servicios específicos.

**Ejemplos:** `supabase-patterns`, `docker-patterns`, `mcp-server-patterns`

---

## Escribir contenido efectivo para una skill

### 1. Empieza con "Cuándo activar"

Esta sección es **crítica** para la autoactivación. Sé específico:

```markdown
## Cuándo activar

- Crear nuevos componentes React
- Refactorizar componentes existentes
- Depurar problemas de estado en React
- Revisar código React en busca de mejores prácticas
```

### 2. Muestra, no cuentes

Malo:
```markdown
## Manejo de errores

Siempre maneja los errores correctamente en funciones async.
```

Bueno:
```markdown
## Manejo de errores

\`\`\`typescript
async function fetchData(url: string) {
  try {
    const response = await fetch(url)

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`)
    }

    return await response.json()
  } catch (error) {
    console.error('Fetch failed:', error)
    throw new Error('Failed to fetch data')
  }
}
\`\`\`

### Puntos clave

- Verifica `response.ok` antes de analizar
- Registra errores para depuración
- Relanza con un mensaje amigable para el usuario
```

### 3. Incluye anti-patrones

Muestra qué NO hacer:

```markdown
## Anti-patrones

### FAIL: Mutación directa de estado

\`\`\`typescript
// NUNCA hagas esto
user.name = 'New Name'
items.push(newItem)
\`\`\`

### PASS: Actualizaciones inmutables

\`\`\`typescript
// SIEMPRE haz esto
const updatedUser = { ...user, name: 'New Name' }
const updatedItems = [...items, newItem]
\`\`\`
```

### 4. Proporciona listas de verificación

Las listas de verificación son accionables y fáciles de seguir:

```markdown
## Lista de verificación previa al despliegue

- [ ] Todas las pruebas pasan
- [ ] No hay console.log en código de producción
- [ ] Las variables de entorno están documentadas
- [ ] No hay secretos hardcodeados
- [ ] El manejo de errores está completo
- [ ] La validación de entrada está implementada
```

### 5. Usa árboles de decisión

Para decisiones complejas:

```markdown
## Cómo elegir el enfoque correcto

\`\`\`
¿Necesitas obtener datos?
├── ¿Una sola solicitud? → usa fetch directamente
├── ¿Múltiples independientes? → Promise.all()
├── ¿Múltiples dependientes? → espera secuencialmente
└── ¿Con caché? → usa SWR o React Query
\`\`\`
```

---

## Mejores prácticas

### HAZ

| Práctica | Ejemplo |
|----------|---------|
| **Sé específico** | "Usa `useCallback` para manejadores de eventos pasados a componentes hijos" |
| **Muestra ejemplos** | Incluye código que se pueda copiar y pegar |
| **Explica el POR QUÉ** | "La inmutabilidad previene efectos secundarios inesperados en el estado de React" |
| **Enlaza skills relacionadas** | "Ver también: `react-performance`" |
| **Mantén el enfoque** | Una skill = un dominio/concepto |
| **Usa secciones** | Encabezados claros para fácil exploración |

### NO HAGAS

| Práctica | Por qué es malo |
|----------|--------------|
| **Ser vago** | "Escribe buen código" - no es accionable |
| **Prosa larga** | Difícil de analizar, mejor como código |
| **Cubrir demasiado** | "Patrones de Python, Django y Flask" - demasiado amplio |
| **Omitir ejemplos** | La teoría sin práctica es menos útil |
| **Ignorar anti-patrones** | Aprender qué NO hacer también es valioso |

### Pautas de contenido

1. **Longitud**: 200-500 líneas típicamente, 800 líneas máximo
2. **Bloques de código**: Incluye identificador de lenguaje
3. **Encabezados**: Usa la jerarquía `##` y `###`
4. **Listas**: Usa `-` para no ordenadas, `1.` para ordenadas
5. **Tablas**: Para comparaciones y referencias

---

## Patrones comunes

### Patrón 1: Skill de estándares

```markdown
---
name: language-standards
description: Estándares de código y mejores prácticas para [language].
---

# Estándares de código de [Language]

## Cuándo activar

- Escribir código [language]
- Revisión de código
- Configurar linting

## Convenciones de nombres

| Elemento | Convención | Ejemplo |
|---------|------------|---------|
| Variables | camelCase | userName |
| Constantes | SCREAMING_SNAKE | MAX_RETRY |
| Funciones | camelCase | fetchUser |
| Clases | PascalCase | UserService |

## Ejemplos de código

[Incluye ejemplos prácticos]

## Configuración de linting

[Incluye configuración]

## Skills relacionadas

- `language-testing`
- `language-security`
```

### Patrón 2: Skill de flujo de trabajo

```markdown
---
name: task-workflow
description: Flujo de trabajo paso a paso para [task].
---

# Flujo de trabajo de [Task]

## Cuándo activar

- [Trigger 1]
- [Trigger 2]

## Requisitos previos

- [Requirement 1]
- [Requirement 2]

## Pasos

### Paso 1: [Nombre]

[Descripción]

\`\`\`bash
[Commands]
\`\`\`

### Paso 2: [Nombre]

[Descripción]

## Verificación

- [ ] [Check 1]
- [ ] [Check 2]

## Solución de problemas

| Problema | Solución |
|---------|----------|
| [Issue] | [Fix] |
```

### Patrón 3: Skill de referencia

```markdown
---
name: api-reference
description: Referencia rápida para [API/Library].
---

# Referencia de [API/Library]

## Cuándo activar

- Usar [API/Library]
- Consultar sintaxis de [API/Library]

## Operaciones comunes

### Operación 1

\`\`\`typescript
// Uso básico
\`\`\`

### Operación 2

\`\`\`typescript
// Uso avanzado
\`\`\`

## Configuración

[Incluye ejemplos de configuración]

## Manejo de errores

[Incluye patrones de error]
```

---

## Probar tu skill

### Pruebas locales

1. **Copia al directorio de skills de Claude Code**:
   ```bash
   cp -r skills/your-skill-name ~/.claude/skills/
   ```

2. **Prueba con Claude Code**:
   ```
   Tú: "Necesito [tarea que debería activar tu skill]"

   Claude debería referirse a los patrones de tu skill.
   ```

3. **Verifica la activación**:
   - Pide a Claude que explique un concepto de tu skill
   - Comprueba si usa tus ejemplos y patrones
   - Asegúrate de que siga tus pautas

### Lista de validación

- [ ] **Frontmatter YAML válido** - Sin errores de sintaxis
- [ ] **El nombre sigue la convención** - lowercase-con-guiones
- [ ] **La descripción es clara** - Indica cuándo usarla
- [ ] **Los ejemplos funcionan** - El código compila y se ejecuta
- [ ] **Los enlaces son válidos** - Las skills relacionadas existen
- [ ] **No hay datos sensibles** - Sin claves API, tokens ni rutas

### Prueba de ejemplos de código

Prueba todos los ejemplos de código:

```bash
# Desde la raíz del repositorio
npx tsc --noEmit skills/your-skill-name/examples/*.ts

# O desde dentro del directorio de la skill
npx tsc --noEmit examples/*.ts

# Desde la raíz del repositorio
python -m py_compile skills/your-skill-name/examples/*.py

# O desde dentro del directorio de la skill
python -m py_compile examples/*.py

# Desde la raíz del repositorio
go build ./skills/your-skill-name/examples/...

# O desde dentro del directorio de la skill
go build ./examples/...
```

---

## Enviar tu skill

### 1. Fork y clonación

```bash
gh repo fork affaan-m/everything-claude-code --clone
cd everything-claude-code
```

### 2. Crear rama

```bash
git checkout -b feat/skill-your-skill-name
```

### 3. Agregar tu skill

```bash
mkdir -p skills/your-skill-name
# Crear SKILL.md
```

### 4. Validar

```bash
# Verificar frontmatter YAML
head -10 skills/your-skill-name/SKILL.md

# Verificar estructura
ls -la skills/your-skill-name/

# Ejecutar pruebas si están disponibles
npm test
```

### 5. Confirmar y enviar

```bash
git add skills/your-skill-name/
git commit -m "feat(skills): add your-skill-name skill"
git push -u origin feat/skill-your-skill-name
```

### 6. Crear Pull Request

Usa esta plantilla de PR:

```markdown
## Resumen

Breve descripción de la skill y por qué es valiosa.

## Tipo de skill

- [ ] Estándares de lenguaje
- [ ] Patrones de framework
- [ ] Flujo de trabajo
- [ ] Conocimiento de dominio
- [ ] Integración de herramientas

## Pruebas

Cómo probé esta skill localmente.

## Lista de verificación

- [ ] Frontmatter YAML válido
- [ ] Ejemplos de código probados
- [ ] Sigue las pautas de skills
- [ ] Sin datos sensibles
- [ ] Triggers de activación claros
```

---

## Galería de ejemplos

### Ejemplo 1: Estándares de lenguaje

**Archivo:** `skills/rust-patterns/SKILL.md`

```markdown
---
name: rust-patterns
description: Idiomas de Rust, patrones de ownership y mejores prácticas para código seguro e idiomático.
origin: ECC
---

# Patrones de Rust

## Cuándo activar

- Escribir código Rust
- Manejar ownership y borrowing
- Manejo de errores con Result/Option
- Implementar traits

## Patrones de ownership

### Reglas de borrowing

\`\`\`rust
// PASS: CORRECTO: Usa borrowing cuando no necesitas ownership
fn process_data(data: &str) -> usize {
    data.len()
}

// PASS: CORRECTO: Toma ownership cuando necesitas modificar o consumir
fn consume_data(data: Vec<u8>) -> String {
    String::from_utf8(data).unwrap()
}
\`\`\`

## Manejo de errores

### Patrón Result

\`\`\`rust
use thiserror::Error;

#[derive(Error, Debug)]
pub enum AppError {
    #[error("IO error: {0}")]
    Io(#[from] std::io::Error),

    #[error("Parse error: {0}")]
    Parse(#[from] std::num::ParseIntError),
}

pub type AppResult<T> = Result<T, AppError>;
\`\`\`

## Skills relacionadas

- `rust-testing`
- `rust-security`
```

### Ejemplo 2: Patrones de framework

**Archivo:** `skills/fastapi-patterns/SKILL.md`

```markdown
---
name: fastapi-patterns
description: Patrones de FastAPI para routing, inyección de dependencias, validación y operaciones async.
origin: ECC
---

# Patrones de FastAPI

## Cuándo activar

- Construir aplicaciones FastAPI
- Crear endpoints de API
- Implementar inyección de dependencias
- Manejar operaciones async de base de datos

## Estructura del proyecto

```text
app/
├── main.py              # Punto de entrada de la app FastAPI
├── routers/             # Manejadores de rutas
│   ├── users.py
│   └── items.py
├── models/              # Modelos Pydantic
│   ├── user.py
│   └── item.py
├── services/            # Lógica de negocio
│   └── user_service.py
└── dependencies.py      # Dependencias compartidas
```

## Inyección de dependencias

```python
from fastapi import Depends
from sqlalchemy.ext.asyncio import AsyncSession

async def get_db() -> AsyncSession:
    async with AsyncSessionLocal() as session:
        yield session

@router.get("/users/{user_id}")
async def get_user(
    user_id: int,
    db: AsyncSession = Depends(get_db)
):
    # Usar la sesión de db
    pass
```

## Skills relacionadas

- `python-patterns`
- `pydantic-validation`
```

### Ejemplo 3: Skill de flujo de trabajo

**Archivo:** `skills/refactoring-workflow/SKILL.md`

```markdown
---
name: refactoring-workflow
description: Flujo de trabajo sistemático de refactorización para mejorar la calidad del código sin cambiar el comportamiento.
origin: ECC
---

# Flujo de trabajo de refactorización

## Cuándo activar

- Mejorar la estructura del código
- Reducir deuda técnica
- Simplificar código complejo
- Extraer componentes reutilizables

## Requisitos previos

- Todas las pruebas pasan
- El directorio de trabajo de Git está limpio
- La rama de función fue creada

## Pasos del flujo de trabajo

### Paso 1: Identificar el objetivo de refactorización

- Busca code smells (métodos largos, código duplicado, clases grandes)
- Revisa la cobertura de pruebas del área objetivo
- Documenta el comportamiento actual

### Paso 2: Asegurar que existan pruebas

```bash
# Ejecutar pruebas para verificar el comportamiento actual
npm test

# Verificar cobertura para archivos objetivo
npm run test:coverage
```

### Paso 3: Hacer cambios pequeños

- Una refactorización a la vez
- Ejecuta pruebas después de cada cambio
- Confirma con frecuencia

### Paso 4: Verificar que el comportamiento no cambió

```bash
# Ejecutar la suite completa de pruebas
npm test

# Ejecutar pruebas E2E
npm run test:e2e
```

## Refactorizaciones comunes

| Olor | Refactorización |
|-------|-------------|
| Método largo | Extraer método |
| Código duplicado | Extraer a función compartida |
| Clase grande | Extraer clase |
| Lista larga de parámetros | Introducir objeto de parámetros |

## Lista de verificación

- [ ] Existen pruebas para el código objetivo
- [ ] Se hicieron cambios pequeños y enfocados
- [ ] Las pruebas pasan después de cada cambio
- [ ] El comportamiento no cambió
- [ ] Se confirmó con un mensaje claro
```

---

## Recursos adicionales

- [CONTRIBUTING.md](../CONTRIBUTING.md) - Pautas generales para contribuir
- [project-guidelines-template](./examples/project-guidelines-template.md) - Plantilla de skill específica de proyecto
- [coding-standards](../skills/coding-standards/SKILL.md) - Ejemplo de skill de estándares
- [tdd-workflow](../skills/tdd-workflow/SKILL.md) - Ejemplo de skill de flujo de trabajo
- [security-review](../skills/security-review/SKILL.md) - Ejemplo de skill de conocimiento de dominio

---

**Recuerda**: Una buena skill es enfocada, accionable y útil de inmediato. Escribe skills que tú mismo querrías usar.
