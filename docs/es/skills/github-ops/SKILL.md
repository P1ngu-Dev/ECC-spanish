---
name: github-ops
description: GitHub repository operations, automation, and management. Issue triage, PR management, CI/CD operations, release management, and security monitoring using the gh CLI. Use when the user wants to manage GitHub issues, PRs, CI status, releases, contributors, stale items, or any GitHub operational task beyond simple git commands.
origin: ECC
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Operaciones de GitHub

Gestiona repositorios de GitHub con foco en salud de comunidad, fiabilidad de CI y experiencia de contribución.

## Cuándo usarlo

- Triar issues (clasificar, etiquetar, responder, deduplicar)
- Gestionar PRs (estado de revisión, checks de CI, PRs obsoletos, preparación para merge)
- Depurar fallos de CI/CD
- Preparar releases y changelogs
- Vigilar Dependabot y alertas de seguridad
- Gestionar la experiencia de contribución en proyectos open source
- El usuario dice “check GitHub”, “triage issues”, “review PRs”, “merge”, “release”, “CI is broken”

## Requisitos de herramienta

- **gh CLI** para todas las operaciones de la API de GitHub
- Acceso al repositorio configurado con `gh auth login`

## Triado de issues

Clasifica cada issue por tipo y prioridad:

**Tipos:** bug, feature-request, question, documentation, enhancement, duplicate, invalid, good-first-issue

**Prioridad:** critical (rotura/seguridad), high (impacto importante), medium (agradable de tener), low (cosmético)

### Flujo de triado

1. Lee el título, el cuerpo y los comentarios del issue
2. Comprueba si duplica otro issue existente (busca por palabras clave)
3. Aplica etiquetas con `gh issue edit --add-label`
4. Para preguntas: redacta y publica una respuesta útil
5. Para bugs que necesiten más info: pide pasos de reproducción
6. Para good first issues: añade la etiqueta `good-first-issue`
7. Para duplicados: comenta con el enlace al original y añade `duplicate`

```bash
# Buscar posibles duplicados
gh issue list --search "keyword" --state all --limit 20

# Añadir etiquetas
gh issue edit <number> --add-label "bug,high-priority"

# Comentar en el issue
gh issue comment <number> --body "Thanks for reporting. Could you share reproduction steps?"
```

## Gestión de PRs

### Lista de comprobación de revisión

1. Comprueba el estado de CI: `gh pr checks <number>`
2. Comprueba si es mergeable: `gh pr view <number> --json mergeable`
3. Comprueba edad y última actividad
4. Marca PRs de más de 5 días sin revisión
5. En PRs de la comunidad: asegúrate de que tengan pruebas y sigan las convenciones

### Política de obsolescencia

- Issues sin actividad durante 14+ días: añade etiqueta `stale` y comenta pidiendo actualización
- PRs sin actividad durante 7+ días: comenta preguntando si sigue activo
- Cierra automáticamente issues obsoletos tras 30 días sin respuesta (añade `closed-stale`)

```bash
# Encontrar issues obsoletos (sin actividad en 14+ días)
gh issue list --label "stale" --state open

# Encontrar PRs sin actividad reciente
gh pr list --json number,title,updatedAt --jq '.[] | select(.updatedAt < "2026-03-01")'
```

## Operaciones de CI/CD

Cuando falla CI:

1. Revisa la ejecución del workflow: `gh run view <run-id> --log-failed`
2. Identifica el paso que falla
3. Comprueba si es una prueba flaky o un fallo real
4. Para fallos reales: identifica la causa raíz y sugiere una corrección
5. Para pruebas flaky: anota el patrón para investigación futura

```bash
# Listar ejecuciones fallidas recientes
gh run list --status failure --limit 10

# Ver logs de una ejecución fallida
gh run view <run-id> --log-failed

# Reejecutar un workflow fallido
gh run rerun <run-id> --failed
```

## Gestión de releases

Al preparar un release:

1. Verifica que todo CI esté en verde en main
2. Revisa cambios sin publicar: `gh pr list --state merged --base main`
3. Genera changelog a partir de títulos de PR
4. Crea el release: `gh release create`

```bash
# Listar PRs mergeados desde el último release
gh pr list --state merged --base main --search "merged:>2026-03-01"

# Crear un release
gh release create v1.2.0 --title "v1.2.0" --generate-notes

# Crear un pre-release
gh release create v1.3.0-rc1 --prerelease --title "v1.3.0 Release Candidate 1"
```

## Monitorización de seguridad

```bash
# Ver alertas de Dependabot
gh api repos/{owner}/{repo}/dependabot/alerts --jq '.[].security_advisory.summary'

# Ver alertas de secret scanning
gh api repos/{owner}/{repo}/secret-scanning/alerts --jq '.[].state'

# Revisar y auto-mergear bumps seguros de dependencias
gh pr list --label "dependencies" --json number,title
```

- Revisa y auto-mergea bumps seguros de dependencias
- Marca de inmediato cualquier alerta crítica/alta
- Comprueba nuevas alertas de Dependabot al menos una vez por semana

## Puerta de calidad

Antes de completar cualquier tarea de operaciones de GitHub:
- todos los issues triados tienen etiquetas adecuadas
- no hay PRs de más de 7 días sin revisión o comentario
- los fallos de CI fueron investigados (no solo reejecutados)
- los releases incluyen changelogs correctos
- las alertas de seguridad están reconocidas y registradas
