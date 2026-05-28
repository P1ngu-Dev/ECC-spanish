# Política de ubicación y procedencia de skills

Este documento define dónde pertenecen las skills generadas, importadas y curadas, cómo se identifican y qué se distribuye.

## Tipos de skills y ubicación

| Tipo | Ruta raíz | Se distribuye | Procedencia |
|------|-----------|---------------|-------------|
| Curated | `skills/` (repo) | Sí | No requerida |
| Learned | `~/.claude/skills/learned/` | No | Requerida |
| Imported | `~/.claude/skills/imported/` | No | Requerida |
| Evolved | `~/.claude/homunculus/evolved/skills/` (global) o `projects/<hash>/evolved/skills/` (por proyecto) | No | Hereda de la fuente instinct |

Las skills curadas viven en el repo bajo `skills/`. Los manifiestos de instalación solo referencian rutas curadas. Las skills generadas e importadas viven bajo el directorio de usuario y nunca se distribuyen.

## Skills curadas

Ubicación: `skills/<skill-name>/` con `SKILL.md` en la raíz.

- Incluidas en las rutas de `manifests/install-modules.json`.
- Validadas por `scripts/ci/validate-skills.js`.
- No tienen archivo de procedencia. Usa `origin` en el frontmatter de `SKILL.md` (ECC, community) para la atribución.

## Skills aprendidas

Ubicación: `~/.claude/skills/learned/<skill-name>/`.

Creada por continuous-learning (hook `evaluate-session`, comando `/learn`). La ruta predeterminada es configurable mediante `skills/continuous-learning/config.json` → `learned_skills_path`.

- No están en el repo. No se distribuyen.
- Deben tener `.provenance.json` junto a `SKILL.md`.
- Se cargan en tiempo de ejecución cuando existe el directorio.

## Skills importadas

Ubicación: `~/.claude/skills/imported/<skill-name>/`.

Skills instaladas por el usuario desde fuentes externas (URL, copia de archivo, etc.). Aún no existe un importador automatizado; la ubicación es por convención.

- No están en el repo. No se distribuyen.
- Deben tener `.provenance.json` junto a `SKILL.md`.

## Skills evolucionadas (Continuous Learning v2)

Ubicación: `~/.claude/homunculus/evolved/skills/` (global) o `~/.claude/homunculus/projects/<hash>/evolved/skills/` (por proyecto).

Generadas por `instinct-cli evolve` a partir de instincts agrupados. Sistema separado de learned/imported.

- No están en el repo. No se distribuyen.
- La procedencia se hereda de los instincts de origen; no se requiere `.provenance.json` separado.

## Metadatos de procedencia

Requeridos para skills learned e imported. Archivo: `.provenance.json` en el directorio de la skill.

Campos requeridos:

| Campo | Tipo | Descripción |
|-------|------|-------------|
| source | string | Origen (URL, ruta o identificador) |
| created_at | string | Marca de tiempo ISO 8601 |
| confidence | number | 0–1 |
| author | string | Quién o qué produjo la skill |

Esquema: `schemas/provenance.schema.json`. Validación: `scripts/lib/skill-evolution/provenance.js` → `validateProvenance`.

## Comportamiento del validador

### validate-skills.js

Alcance: solo skills curadas (`skills/` en el repo).

- Si `skills/` no existe: sale con código 0 (nada que validar).
- Por cada subdirectorio: debe contener `SKILL.md`, no vacío.
- No toca las rutas learned/imported/evolved.

### validate-install-manifests.js

Alcance: solo rutas curadas. Todas las `paths` en los módulos deben existir en el repo.

- Las rutas generated/imported están fuera de alcance. Ningún manifiesto las referencia.
- Ruta faltante → error. Sin manejo de rutas opcionales.

### Scripts que usan rutas generadas

`scripts/skills-health.js`, `scripts/lib/skill-evolution/health.js`, hooks de sesión: verifican `~/.claude/skills/learned` y `~/.claude/skills/imported`. Los directorios faltantes se tratan como vacíos; no hay errores.

## Distribuibles vs solo local

| Distribuible | Solo local |
|-------------|------------|
| `skills/*` (curated) | `~/.claude/skills/learned/*` |
| | `~/.claude/skills/imported/*` |
| | `~/.claude/homunculus/**/evolved/**` |

Solo las skills curadas aparecen en los manifiestos de instalación y se copian durante la instalación.

## Hoja de ruta de implementación

1. Documento de política y esquema de procedencia (este cambio).
2. Agregar validación de procedencia a las rutas de escritura de learned-skill (`evaluate-session`, salida de `/learn`) para que las nuevas skills learned siempre obtengan `.provenance.json`.
3. Actualizar `instinct-cli evolve` para escribir procedencia opcional al generar skills evolved.
4. Agregar `scripts/validate-provenance.js` a CI para cualquier ruta del repo que no deba contener contenido learned/imported (si hace falta).
5. Documentar las rutas learned/imported en `CONTRIBUTING.md` o en la documentación de usuario para que los contribuyentes sepan que no deben confirmarlas.
