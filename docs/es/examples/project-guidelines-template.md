# Plantilla de directrices del proyecto

Esta es una plantilla de skill específica de proyecto que antes se distribuía como un skill vivo de ECC.

Ahora vive en `docs/examples/` porque es material de referencia, no un skill reusable entre proyectos.

Este es un ejemplo de skill específico de proyecto. Úsalo como plantilla para tus propios proyectos.

Basado en una aplicación de producción real: [Zenith](https://zenith.chat) - plataforma de descubrimiento de clientes impulsada por IA.

## Cuándo usarlo

Consulta este skill cuando trabajes en el proyecto específico para el que fue diseñado. Los skills de proyecto contienen:
- visión general de arquitectura
- estructura de archivos
- patrones de código
- requisitos de testing
- flujo de despliegue

---

## Visión general de arquitectura

**Stack tecnológico:**
- **Frontend**: Next.js 15 (App Router), TypeScript, React
- **Backend**: FastAPI (Python), modelos Pydantic
- **Base de datos**: Supabase (PostgreSQL)
- **IA**: API de Claude con tool calling y structured output
- **Despliegue**: Google Cloud Run
- **Testing**: Playwright (E2E), pytest (backend), React Testing Library

**Servicios:**
```
┌─────────────────────────────────────────────────────────────┐
│                         Frontend                            │
│  Next.js 15 + TypeScript + TailwindCSS                     │
│  Deployed: Vercel / Cloud Run                              │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                         Backend                             │
│  FastAPI + Python 3.11 + Pydantic                          │
│  Deployed: Cloud Run                                       │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
        ┌──────────┐   ┌──────────┐   ┌──────────┐
        │ Supabase │   │  Claude  │   │  Redis   │
        │ Database │   │   API    │   │  Cache   │
        └──────────┘   └──────────┘   └──────────┘
```

---

## Estructura de archivos

```
project/
├── frontend/
│   └── src/
│       ├── app/              # páginas del app router de Next.js
│       │   ├── api/          # API routes
│       │   ├── (auth)/       # rutas protegidas por auth
│       │   └── workspace/    # espacio principal de la app
│       ├── components/       # componentes React
│       │   ├── ui/           # componentes UI base
│       │   ├── forms/        # componentes de formularios
│       │   └── layouts/      # componentes de layout
│       ├── hooks/            # custom React hooks
│       ├── lib/              # utilidades
│       ├── types/            # definiciones TypeScript
│       └── config/           # configuración
│
├── backend/
│   ├── routers/              # handlers de rutas FastAPI
│   ├── models.py             # modelos Pydantic
│   ├── main.py               # entry de la app FastAPI
│   ├── auth_system.py        # autenticación
│   ├── database.py           # operaciones de base de datos
│   ├── services/             # lógica de negocio
│   └── tests/                # tests pytest
│
├── deploy/                   # configs de despliegue
├── docs/                     # documentación
└── scripts/                  # scripts de utilidad
```

---

## Patrones de código

### Formato de respuesta de API (FastAPI)

```python
from pydantic import BaseModel
from typing import Generic, TypeVar, Optional

T = TypeVar('T')

class ApiResponse(BaseModel, Generic[T]):
    success: bool
    data: Optional[T] = None
    error: Optional[str] = None

    @classmethod
    def ok(cls, data: T) -> "ApiResponse[T]":
        return cls(success=True, data=data)

    @classmethod
    def fail(cls, error: str) -> "ApiResponse[T]":
        return cls(success=False, error=error)
```

### Llamadas API del frontend (TypeScript)

```typescript
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
}

async function fetchApi<T>(
  endpoint: string,
  options?: RequestInit
): Promise<ApiResponse<T>> {
  try {
    const response = await fetch(`/api${endpoint}`, {
      ...options,
      headers: {
        'Content-Type': 'application/json',
        ...options?.headers,
      },
    })

    if (!response.ok) {
      return { success: false, error: `HTTP ${response.status}` }
    }

    return await response.json()
  } catch (error) {
    return { success: false, error: String(error) }
  }
}
```

### Integración con Claude AI (structured output)

```python
from anthropic import Anthropic
from pydantic import BaseModel

class AnalysisResult(BaseModel):
    summary: str
    key_points: list[str]
    confidence: float

async def analyze_with_claude(content: str) -> AnalysisResult:
    client = Anthropic()

    response = client.messages.create(
        model="claude-sonnet-4-5-20250514",
        max_tokens=1024,
        messages=[{"role": "user", "content": content}],
        tools=[{
            "name": "provide_analysis",
            "description": "Provide structured analysis",
            "input_schema": AnalysisResult.model_json_schema()
        }],
        tool_choice={"type": "tool", "name": "provide_analysis"}
    )

    # Extraer el resultado del tool use
    tool_use = next(
        block for block in response.content
        if block.type == "tool_use"
    )

    return AnalysisResult(**tool_use.input)
```

### Custom hooks (React)

```typescript
import { useState, useCallback } from 'react'

interface UseApiState<T> {
  data: T | null
  loading: boolean
  error: string | null
}

export function useApi<T>(
  fetchFn: () => Promise<ApiResponse<T>>
) {
  const [state, setState] = useState<UseApiState<T>>({
    data: null,
    loading: false,
    error: null,
  })

  const execute = useCallback(async () => {
    setState(prev => ({ ...prev, loading: true, error: null }))

    const result = await fetchFn()

    if (result.success) {
      setState({ data: result.data!, loading: false, error: null })
    } else {
      setState({ data: null, loading: false, error: result.error! })
    }
  }, [fetchFn])

  return { ...state, execute }
}
```

---

## Requisitos de testing

### Backend (pytest)

```bash
# Ejecutar todos los tests
poetry run pytest tests/

# Ejecutar con coverage
poetry run pytest tests/ --cov=. --cov-report=html

# Ejecutar archivo de test específico
poetry run pytest tests/test_auth.py -v
```

**Estructura de test:**
```python
import pytest
from httpx import AsyncClient
from main import app

@pytest.fixture
async def client():
    async with AsyncClient(app=app, base_url="http://test") as ac:
        yield ac

@pytest.mark.asyncio
async def test_health_check(client: AsyncClient):
    response = await client.get("/health")
    assert response.status_code == 200
    assert response.json()["status"] == "healthy"
```

### Frontend (React Testing Library)

```bash
# Ejecutar tests
npm run test

# Ejecutar con coverage
npm run test -- --coverage

# Ejecutar tests E2E
npm run test:e2e
```

**Estructura de test:**
```typescript
import { render, screen, fireEvent } from '@testing-library/react'
import { WorkspacePanel } from './WorkspacePanel'

describe('WorkspacePanel', () => {
  it('renders workspace correctly', () => {
    render(<WorkspacePanel />)
    expect(screen.getByRole('main')).toBeInTheDocument()
  })

  it('handles session creation', async () => {
    render(<WorkspacePanel />)
    fireEvent.click(screen.getByText('New Session'))
    expect(await screen.findByText('Session created')).toBeInTheDocument()
  })
})
```

---

## Flujo de despliegue

### Checklist previo al despliegue

- [ ] Todos los tests pasan localmente
- [ ] `npm run build` funciona (frontend)
- [ ] `poetry run pytest` pasa (backend)
- [ ] No hay secretos hardcodeados
- [ ] Las variables de entorno están documentadas
- [ ] Las migraciones de base de datos están listas

### Comandos de despliegue

```bash
# Build y despliegue del frontend
cd frontend && npm run build
gcloud run deploy frontend --source .

# Build y despliegue del backend
cd backend
gcloud run deploy backend --source .
```

### Variables de entorno

```bash
# Frontend (.env.local)
NEXT_PUBLIC_API_URL=https://api.example.com
NEXT_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJ...

# Backend (.env)
DATABASE_URL=postgresql://...
ANTHROPIC_API_KEY=sk-ant-...
SUPABASE_URL=https://xxx.supabase.co
SUPABASE_KEY=eyJ...
```

---

## Reglas críticas

1. **Sin emojis** en código, comentarios o documentación
2. **Inmutabilidad** - nunca mutar objetos o arrays
3. **TDD** - escribe tests antes de la implementación
4. **80% coverage** mínimo
5. **Muchos archivos pequeños** - 200-400 líneas típico, 800 máximo
6. **No console.log** en código de producción
7. **Manejo adecuado de errores** con try/catch
8. **Validación de entrada** con Pydantic/Zod

---

## Skills relacionados

- `coding-standards.md` - buenas prácticas generales de código
- `backend-patterns.md` - patrones de API y base de datos
- `frontend-patterns.md` - patrones de React y Next.js
- `tdd-workflow/` - metodología de desarrollo guiada por tests
