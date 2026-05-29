**Idioma:** [English](../../README.md) | [Português (Brasil)](../pt-BR/README.md) | [简体中文](../../README.zh-CN.md) | [繁體中文](../zh-TW/README.md) | [日本語](../ja-JP/README.md) | [한국어](../ko-KR/README.md) | [Türkçe](../tr/README.md) | [Русский](../ru/README.md) | [Tiếng Việt](../vi-VN/README.md) | [ไทย](../th/README.md) | [Deutsch](../de-DE/README.md) | [Español]

# ECC

![ECC - el sistema operativo nativo del harness para el trabajo de agentes](../../assets/hero.png)

[![Stars](https://img.shields.io/github/stars/affaan-m/ECC?style=flat)](https://github.com/affaan-m/ECC/stargazers)
[![Forks](https://img.shields.io/github/forks/affaan-m/ECC?style=flat)](https://github.com/affaan-m/ECC/network/members)
[![Contributors](https://img.shields.io/github/contributors/affaan-m/ECC?style=flat)](https://github.com/affaan-m/ECC/graphs/contributors)
[![npm ecc-universal](https://img.shields.io/npm/dw/ecc-universal?label=ecc-universal%20weekly%20downloads&logo=npm)](https://www.npmjs.com/package/ecc-universal)
[![npm ecc-agentshield](https://img.shields.io/npm/dw/ecc-agentshield?label=ecc-agentshield%20weekly%20downloads&logo=npm)](https://www.npmjs.com/package/ecc-agentshield)
[![GitHub App Install](https://img.shields.io/badge/GitHub%20App-150%20installs-2ea44f?logo=github)](https://github.com/marketplace/ecc-tools)
[![Licencia](https://img.shields.io/badge/license-MIT-blue.svg)](../../LICENSE)
![Shell](https://img.shields.io/badge/-Shell-4EAA25?logo=gnu-bash&logoColor=white)
![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?logo=typescript&logoColor=white)
![Python](https://img.shields.io/badge/-Python-3776AB?logo=python&logoColor=white)
![Go](https://img.shields.io/badge/-Go-00ADD8?logo=go&logoColor=white)
![Java](https://img.shields.io/badge/-Java-ED8B00?logo=openjdk&logoColor=white)
![Perl](https://img.shields.io/badge/-Perl-39457E?logo=perl&logoColor=white)
![Markdown](https://img.shields.io/badge/-Markdown-000000?logo=markdown&logoColor=white)

> **182K+ estrellas** | **28K+ forks** | **170+ contribuidores** | **12+ ecosistemas de lenguajes** | **Ganador del Hackathon de Anthropic**

---

<div align="center">

**Language / 语言 / 語言 / Dil / Язык / Ngôn ngữ**

[**English**](../../README.md) | [Português (Brasil)](../pt-BR/README.md) | [简体中文](../../README.zh-CN.md) | [繁體中文](../zh-TW/README.md) | [日本語](../ja-JP/README.md) | [한국어](../ko-KR/README.md)
 | [Türkçe](../tr/README.md) | [Русский](../ru/README.md) | [Tiếng Việt](../vi-VN/README.md) | [ไทย](../th/README.md) | [Deutsch](../de-DE/README.md) | [Español](README.md)

</div>

---

**El sistema operativo nativo del harness para el trabajo de agentes. Ganador de un hackathon de Anthropic.**

No son solo configuraciones. Es un sistema completo: skills, instintos, optimización de memoria, aprendizaje continuo, análisis de seguridad y desarrollo centrado en la investigación. Agentes listos para producción, skills, hooks, rules, configuraciones MCP y shims de comandos heredados evolucionados durante más de 10 meses de uso diario intensivo construyendo productos reales.

Funciona con **Claude Code**, **Codex**, **Cursor**, **OpenCode**, **Gemini**, **Zed**, **GitHub Copilot**, y otros harnesses de agentes de IA.

ECC v2.0.0-rc.1 agrega la historia del operador Hermes público encima de esa capa reutilizable: comienza con la [guía de configuración de Hermes](guides/HERMES-SETUP.md), luego revisa las [notas de la versión rc.1](../../docs/releases/2.0.0-rc.1/release-notes.md) y la [arquitectura entre harnesses](../../docs/architecture/cross-harness.md).  

---

<table>
<tr>
<td width="25%" align="center">
  <a href="https://ecc.tools/pricing">
    <strong> ECC Pro</strong><br />
    <sub>Repositorios p rivados · GitHub App · $19/asiento/mes</sub>
  </a>
</td>
<td width="25%" align="center">
  <a href="https://github.com/sponsors/affaan-m">
    <strong> Patrocinador</strong><br />
    <sub>Patrocina el OSS · Desde $5/mes</sub>
  </a>
</td>
<td width="25%" align="center">
  <a href="https://github.com/affaan-m/ECC/discussions">
    <strong>Comunidad</strong>
    <br />
    <sub>Discusiones · Preguntas y respuestas · Presentaciones</sub>
  </a>
</td>
<td width="25%" align="center">
  <a href="https://github.com/apps/ecc-tools">
    <strong> GitHub App</strong><br />
    <sub>Instalar · Auditorías de PR · Nivel gratuito</sub>
  </a>
</td>
</tr>
</table>

<sub>**El OSS se mantiene gratuito.** Este repositorio tiene licencia MIT para siempre. ECC Pro es la GitHub App alojada para repositorios privados. <a href="https://github.com/sponsors/affaan-m">Los patrocinadores</a> y <a href="https://ecc.tools/pricing">los suscriptores Pro</a> financian el trabajo, por eso un solo mantenedor lanza actualizaciones semanales en 7 harnesses.</sub>

---

## Las guías

Este repositorio solo contiene el código en bruto. Las guías lo explican todo.

<table>
<tr>
<td width="33%">
<a href="https://x.com/affaanmustafa/status/2012378465664745795">
<img src="./assets/images/guides/shorthand-guide.png" alt="Guía breve de Everything Claude Code" />
</a>
</td>
<td width="33%">
<a href="https://x.com/affaanmustafa/status/2014040193557471352">
<img src="./assets/images/guides/longform-guide.png" alt="Guía extensa de Everything Claude Code" />
</a>
</td>
<td width="33%">
<a href="https://x.com/affaanmustafa/status/2033263813387223421">
<img src="./assets/images/security/security-guide-header.png" alt="Guía breve de seguridad para trabajo de agentes" />
</a>
</td>
</tr>
<tr>
<td align="center"><b>Guía breve</b><br/>Configuración, fundamentos, filosofía. <b>Lee esto primero.</b></td>
<td align="center"><b>Guía extensa</b><br/>Optimización de tokens, persistencia de memoria, evaluaciones, paralelización.</td>
<td align="center"><b>Guía de seguridad</b><br/>Vectores de ataque, sandboxing, saneamiento, CVEs, AgentShield.</td>
</tr>
</table>

| Tema | Lo que aprenderás |
|-------|-------------------|
| Optimización de tokens | Selección de modelo, reducción de prompt del sistema, procesos en segundo plano |
| Persistencia de memoria | Hooks que guardan/cargan contexto entre sesiones automáticamente |
| Aprendizaje continuo | Extracción automática de patrones de sesiones hacia skills reutilizables |
| Bucles de verificación | Puntos de control vs evaluaciones continuas, tipos de evaluador, métricas pass@k |
| Paralelización | Worktrees de git, método en cascada, cuándo escalar instancias |
| Orquestación de subagentes | El problema de contexto, patrón de recuperación iterativa |

---

## Novedades

### v2.0.0-rc.1 — Actualización de superficie, flujos de trabajo de operador y ECC 2.0 Alpha (Abr 2026)

- **GUI del panel** — Nueva aplicación de escritorio basada en Tkinter (`ecc_dashboard.py` o `npm run dashboard`) con selector de tema oscuro/claro, personalización de fuente y logotipo del proyecto en la cabecera y la barra de tareas.
- **Superficie pública sincronizada con el repositorio activo** — Los metadatos, conteos del catálogo, manifiestos del plugin y documentación orientada a la instalación ahora coinciden con la superficie OSS real: 61 agentes, 246 skills y 76 command shims heredados.
- **Expansión de flujos de trabajo de operador y salida** — `brand-voice`, `social-graph-ranker`, `connections-optimizer`, `customer-billing-ops`, `ecc-tools-cost-audit`, `google-workspace-ops`, `project-flow-ops` y `workspace-surface-audit` completan la línea de operador.
- **Herramientas de medios y lanzamientos** — `manim-video`, `remotion-video-creation` y las superficies mejoradas para publicación social convierten los explicadores técnicos y el contenido de lanzamiento en parte del mismo sistema.
- **Crecimiento de la superficie de framework y producto** — `nestjs-patterns`, superficies de instalación más ricas para Codex/OpenCode y un empaquetado cross-harness ampliado mantienen el repositorio útil más allá de Claude Code.
- **Paquete de skills para mercados de predicción Itô** — `ito-market-intelligence`, `ito-basket-compare`, `ito-trade-planner`, `ito-data-atlas-agent`, `prediction-market-oracle-research` y `prediction-market-risk-review` agregan flujos de trabajo públicos y no asesorativos para mercados/cestas, manteniendo el acceso en vivo a la API de Itô restringido y separado de la facturación de ECC Tools.
- **Paquete de skills de optimización** — `parallel-execution-optimizer`, `benchmark-optimization-loop`, `data-throughput-accelerator`, `latency-critical-systems` y `recursive-decision-ledger` convierten prompts repetidos sobre velocidad/recursión en flujos acotados de benchmark, rendimiento y registro de decisiones.
- **ECC 2.0 alpha ya viene en el árbol** — El prototipo del plano de control en Rust dentro de `ecc2/` ahora compila localmente y expone los comandos `dashboard`, `start`, `sessions`, `status`, `stop`, `resume` y `daemon`. Puede usarse como alpha, aunque todavía no es una versión general.
- **Instantáneas de estado del operador** — `ecc status --markdown --write status.md` convierte el almacén de estado local en un handoff portable que cubre preparación, sesiones activas, salud de ejecución de skills, salud de instalación, eventos de gobernanza pendientes y elementos de trabajo enlazados desde Linear/GitHub/handoffs. Usa `ecc work-items upsert ...` para entradas manuales, `ecc work-items sync-github --repo owner/repo` para el estado de la cola de PR/issues y `ecc status --exit-code` para hacer fallar automatizaciones cuando la preparación necesite atención.
- **Endurecimiento del ecosistema** — AgentShield, los controles de coste de ECC Tools, el trabajo del portal de facturación y las actualizaciones del sitio web siguen evolucionando alrededor del plugin principal en vez de derivar hacia silos separados.

### v1.9.0 — Instalación Selectiva y Expansión de Lenguajes (Mar 2026)

- **Arquitectura de instalación selectiva** — Pipeline de instalación guiado por manifiestos con `install-plan.js` y `install-apply.js` para instalar componentes concretos. El almacén de estado registra qué está instalado y permite actualizaciones incrementales.
- **6 agentes nuevos** — `typescript-reviewer`, `pytorch-build-resolver`, `java-build-resolver`, `java-reviewer`, `kotlin-reviewer` y `kotlin-build-resolver` amplían la cobertura a 10 lenguajes.
- **Nuevas skills** — `pytorch-patterns` para flujos de deep learning, `documentation-lookup` para consulta de referencias de API, `bun-runtime` y `nextjs-turbopack` para toolchains modernos de JS, además de 8 skills de dominio operativo y `mcp-server-patterns`.
- **Infraestructura de sesión y estado** — Almacén de estado en SQLite con CLI de consultas, adaptadores de sesión para registro estructurado y base de evolución de skills para skills auto-mejorables.
- **Revisión profunda de la orquestación** — La puntuación de auditoría del harness se volvió determinista, se endurecieron el estado de orquestación y la compatibilidad del lanzador, y se añadió prevención de bucles observadores con una guarda de 5 capas.
- **Fiabilidad del observador** — Corrección de explosión de memoria con throttling y tail sampling, corrección de acceso al sandbox, lógica de inicio perezoso y guarda contra reentrada.
- **12 ecosistemas de lenguaje** — Nuevas rules para Java, PHP, Perl, Kotlin/Android/KMP, C++ y Rust se suman a las ya existentes para TypeScript, Python, Go y common.
- **Contribuciones de la comunidad** — Traducciones al coreano y chino, optimización de hooks con biome, skills de procesamiento de video, skills operativas, instalador en PowerShell y soporte para Antigravity IDE.
- **Endurecimiento de CI** — 19 correcciones de fallos de pruebas, validación forzada del conteo del catálogo y de manifiestos de instalación, y suite completa en verde.

### v1.8.0 — Sistema de Rendimiento del Harness (Mar 2026)

- **Versión centrada en el harness** — ECC ahora se presenta explícitamente como un sistema de rendimiento para harnesses de agentes, no solo como un paquete de configuración.
- **Revisión profunda de la fiabilidad de hooks** — Fallback de raíz para SessionStart, resúmenes de sesión en la fase Stop y hooks basados en scripts reemplazando one-liners inline frágiles.
- **Controles de tiempo de ejecución para hooks** — `ECC_HOOK_PROFILE=minimal|standard|strict` y `ECC_DISABLED_HOOKS=...` para regular el comportamiento en tiempo de ejecución sin editar archivos de hooks.
- **Nuevos comandos de harness** — `/harness-audit`, `/loop-start`, `/loop-status`, `/quality-gate`, `/model-route`.
- **NanoClaw v2** — model routing, skill hot-load, session branch/search/export/compact/metrics.
- **Paridad cross-harness** — Se ajustó el comportamiento entre Claude Code, Cursor, OpenCode y la app/CLI de Codex.
- **997 pruebas internas pasando** — Suite completa en verde tras la refactorización de hooks/runtime y las actualizaciones de compatibilidad.

### v1.7.0 — Expansión Multiplataforma y Creador de Presentaciones (Feb 2026)

- **Soporte para app + CLI de Codex** — Soporte directo para Codex basado en `AGENTS.md`, instalación dirigida y documentación para Codex.
- **Skill `frontend-slides`** — Constructor de presentaciones HTML sin dependencias, con guía para convertir PPTX y reglas estrictas de ajuste al viewport.
- **5 nuevas skills genéricas de negocio/contenido** — `article-writing`, `content-engine`, `market-research`, `investor-materials`, `investor-outreach`.
- **Cobertura de herramientas más amplia** — Se reforzó el soporte para Cursor, Codex y OpenCode para que el mismo repo funcione limpiamente en los principales harnesses.
- **992 pruebas internas** — Se amplió la cobertura de validación y regresión en plugin, hooks, skills y empaquetado.

### v1.6.0 — CLI de Codex, AgentShield y Marketplace (Feb 2026)

- **Soporte para Codex CLI** — El nuevo comando `/codex-setup` genera `codex.md` para compatibilidad con OpenAI Codex CLI.
- **7 nuevas skills** — `search-first`, `swift-actor-persistence`, `swift-protocol-di-testing`, `regex-vs-llm-structured-text`, `content-hash-cache-pattern`, `cost-aware-llm-pipeline`, `skill-stocktake`.
- **Integración con AgentShield** — La skill `/security-scan` ejecuta AgentShield directamente desde Claude Code; 1282 pruebas, 102 rules.
- **GitHub Marketplace** — La GitHub App de ECC Tools está disponible en [github.com/marketplace/ecc-tools](https://github.com/marketplace/ecc-tools) con planes free/pro/enterprise.
- **30+ PRs de la comunidad fusionados** — Aportes de 30 contribuidores en 6 idiomas.
- **978 pruebas internas** — Suite de validación ampliada en agentes, skills, comandos, hooks y rules.

### v1.4.1 — Corrección de errores (Feb 2026)

- **Corrección de pérdida de contenido al importar instincts** — `parse_instinct_file()` descartaba silenciosamente todo el contenido posterior al frontmatter (secciones Action, Evidence y Examples) durante `/instinct-import`. ([#148](https://github.com/affaan-m/ECC/issues/148), [#161](https://github.com/affaan-m/ECC/pull/161))

### v1.4.0 — Reglas Multilenguaje, Asistente de Instalación y PM2 (Feb 2026)

- **Asistente interactivo de instalación** — La nueva skill `configure-ecc` ofrece una configuración guiada con detección de merge/sobrescritura.
- **PM2 y orquestación multiagente** — 6 comandos nuevos (`/pm2`, `/multi-plan`, `/multi-execute`, `/multi-backend`, `/multi-frontend`, `/multi-workflow`) para gestionar flujos de trabajo complejos de múltiples servicios.
- **Arquitectura multilenguaje de rules** — Las rules se reestructuraron desde archivos planos a directorios `common/` + `typescript/` + `python/` + `golang/`. Instala solo los lenguajes que necesites.
- **Traducciones al chino (zh-CN)** — Traducción completa de todos los agentes, comandos, skills y rules (80+ archivos).
- **Soporte para GitHub Sponsors** — Patrocina el proyecto mediante GitHub Sponsors.
- **CONTRIBUTING.md mejorado** — Plantillas detalladas de PR para cada tipo de contribución.

### v1.3.0 — Soporte para Plugin de OpenCode (Feb 2026)

- **Integración completa con OpenCode** — 12 agentes, 24 comandos y 16 skills con soporte de hooks a través del sistema de plugins de OpenCode (20+ tipos de evento).
- **3 herramientas nativas personalizadas** — `run-tests`, `check-coverage`, `security-audit`.
- **Documentación para LLM** — `llms.txt` para documentación completa de OpenCode.

### v1.2.0 — Comandos y Skills Unificados (Feb 2026)

- **Soporte para Python/Django** — Skills de patrones, seguridad, TDD y verificación para Django.
- **Skills para Java Spring Boot** — Patrones, seguridad, TDD y verificación para Spring Boot.
- **Gestión de sesiones** — Comando `/sessions` para el historial de sesiones.
- **Continuous learning v2** — Aprendizaje basado en instincts con puntuación de confianza, import/export y evolución.

Consulta el changelog completo en [Releases](https://github.com/affaan-m/ECC/releases).

---

## Inicio rápido

Ponte en marcha en menos de 2 minutos:

### Elige solo una ruta

La mayoría de los usuarios de Claude Code deberían usar exactamente una ruta de instalación:

- **Predeterminado recomendado:** instala el complemento de Claude Code, luego copia solo las carpetas de reglas que realmente necesites.
- **Usa el instalador manual solo si** deseas un control más detallado, quieres evitar la ruta del complemento por completo, o tu compilación de Claude Code tiene problemas para resolver la entrada del marketplace autoalojado.
- **No combines varios métodos de instalación.** La configuración rota más común es: primero `/plugin install`, y después `install.sh --profile full` o `npx ecc-install --profile full`.

Si ya superpusiste varias instalaciones y las cosas parecen duplicadas, ve directamente a [Restablecer / desinstalar ECC](#reset--uninstall-ecc).

### Ruta de bajo contexto / sin hooks

Si los hooks te parecen demasiado globales o solo quieres las rules, agentes, comandos y skills de flujo central de ECC, omite el complemento y usa el perfil manual mínimo:

```bash
./install.sh --profile minimal --target claude
```

```powershell
.\install.ps1 --profile minimal --target claude
# o
npx ecc-install --profile minimal --target claude
```

Este perfil excluye intencionalmente `hooks-runtime`.

Si quieres el perfil central normal pero necesitas desactivar hooks, usa:

```bash
./install.sh --profile core --without baseline:hooks --target claude
```

Añade hooks más adelante solo si quieres aplicación en tiempo de ejecución:

```bash
./install.sh --target claude --modules hooks-runtime
```

### Encuentra primero los componentes adecuados

Si no sabes qué perfil o componente de ECC instalar, consulta el asesor empaquetado desde cualquier proyecto:

```bash
npx ecc consult "security reviews" --target claude
```

Devuelve los componentes coincidentes, perfiles relacionados y comandos de vista previa/instalación. Usa el comando de vista previa antes de instalar si quieres inspeccionar el plan exacto de archivos.

Para flujos de trabajo de ML/MLOps en producción, mantén la instalación como opt-in y acotada por componentes:

```bash
npx ecc consult "mlops training model deployment" --target claude
npx ecc install --profile minimal --target claude --with capability:machine-learning
```

### Paso 1: instala el complemento (recomendado)

> NOTA: El complemento es práctico, pero el instalador OSS de abajo sigue siendo la ruta más fiable si tu compilación de Claude Code tiene problemas para resolver entradas de marketplace autoalojadas.

```bash
# Añadir marketplace
/plugin marketplace add https://github.com/affaan-m/ECC

# Instalar plugin
/plugin install ecc@ecc
```

### Nota sobre nombres y migración

ECC ahora tiene tres identificadores públicos y no son intercambiables:

- GitHub source repo: `affaan-m/ECC`
- Claude marketplace/plugin identifier: `ecc@ecc`
- npm package: `ecc-universal`

Esto es intencional. Las instalaciones de marketplace/plugin de Anthropic están asociadas a un identificador canónico de plugin, por eso ECC usa `ecc@ecc` para mantener nombres de herramientas y espacios de nombres de comandos con slash lo bastante cortos para validadores estrictos de Desktop/API. Algunas publicaciones antiguas pueden mostrar todavía el identificador largo anterior del marketplace; trátalo solo como un alias heredado. Por separado, el paquete de npm se mantuvo como `ecc-universal`, así que las instalaciones vía npm y las del marketplace usan nombres distintos de forma deliberada.

### Paso 2: instala las rules solo si las necesitas

> ADVERTENCIA: **Importante:** los complementos de Claude Code no pueden distribuir `rules` automáticamente.
>
> Si ya instalaste ECC a través de `/plugin install`, **no ejecutes `./install.sh --profile full`, `.\install.ps1 --profile full`, ni `npx ecc-install --profile full` después**. El complemento ya carga las skills, comandos y hooks de ECC. Ejecutar el instalador completo después de una instalación de complemento copia esas mismas superficies en tus directorios de usuario y puede crear skills duplicadas más comportamiento duplicado en tiempo de ejecución.
>
> Para instalaciones por complemento, copia manualmente solo los directorios `rules/` que quieras en `~/.claude/rules/ecc/`. Empieza con `rules/common` y un paquete de lenguaje o framework que realmente uses. No copies todos los directorios de rules a menos que explícitamente quieras todo ese contexto en Claude.
>
> Usa el instalador completo solo cuando estés haciendo una instalación totalmente manual de ECC en lugar de la ruta del complemento.
>
> Si tu configuración local de Claude se borró o restableció, eso no significa que tengas que volver a comprar ECC. Empieza con `node scripts/ecc.js list-installed`, luego ejecuta `node scripts/ecc.js doctor` y `node scripts/ecc.js repair` antes de reinstalar nada. Eso suele restaurar los archivos gestionados por ECC sin reconstruir tu configuración. Si el problema es de acceso a la cuenta o al marketplace de ECC Tools, gestiona la recuperación de facturación/cuenta por separado.

```bash
# Clona el repo primero
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Instala dependencias (elige tu gestor de paquetes)
npm install        # o: pnpm install | yarn install | bun install

# Ruta con plugin: copia solo las rules de ECC a un namespace propio de ECC
mkdir -p ~/.claude/rules/ecc
cp -R rules/common ~/.claude/rules/ecc/
cp -R rules/typescript ~/.claude/rules/ecc/

# Ruta de instalación totalmente manual de ECC (úsala en lugar de /plugin install)
# ./install.sh --profile full
```

```powershell
# Windows PowerShell

# Ruta con plugin: copia solo las rules de ECC a un namespace propio de ECC
New-Item -ItemType Directory -Force -Path "$HOME/.claude/rules/ecc" | Out-Null
Copy-Item -Recurse rules/common "$HOME/.claude/rules/ecc/"
Copy-Item -Recurse rules/typescript "$HOME/.claude/rules/ecc/"

# Ruta de instalación totalmente manual de ECC (úsala en lugar de /plugin install)
# .\install.ps1 --profile full
# npx ecc-install --profile full
```

Para instrucciones de instalación manual, consulta el README de la carpeta `rules/`. Al copiar rules manualmente, copia el directorio completo del lenguaje (por ejemplo `rules/common` o `rules/golang`), no los archivos internos, para que las referencias relativas sigan funcionando y los nombres de archivo no colisionen.

### Instalación totalmente manual (alternativa)

Usa esto solo si intencionalmente estás omitiendo la ruta del complemento:

```bash
./install.sh --profile full
```

```powershell
.\install.ps1 --profile full
# o
npx ecc-install --profile full
```

Si eliges esta ruta, detente ahí. No ejecutes también `/plugin install`.

### Restablecer / desinstalar ECC

Si ECC parece duplicado, intrusivo o roto, no sigas reinstalándolo encima de sí mismo.

- **Ruta con plugin:** elimina el plugin de Claude Code y luego borra las carpetas concretas de rules que copiaste manualmente en `~/.claude/rules/ecc/`.
- **Ruta con instalador manual / CLI:** desde la raíz del repo, previsualiza primero la eliminación:

```bash
node scripts/uninstall.js --dry-run
```

Luego elimina los archivos gestionados por ECC:

```bash
node scripts/uninstall.js
```

También puedes usar el wrapper de ciclo de vida:

```bash
node scripts/ecc.js list-installed
node scripts/ecc.js doctor
node scripts/ecc.js repair
node scripts/ecc.js uninstall --dry-run
```

ECC solo elimina archivos registrados en su estado de instalación. No borrará archivos no relacionados que no haya instalado.

Si superpusiste métodos, limpia en este orden:

1. Elimina la instalación del plugin de Claude Code.
2. Ejecuta el comando de desinstalación de ECC desde la raíz del repo para quitar los archivos gestionados por el estado de instalación.
3. Borra cualquier carpeta extra de rules que hayas copiado manualmente y ya no quieras.
4. Reinstala una sola vez, usando una única ruta.

### Paso 3: empezar a usar

```bash
# Las skills son la superficie principal del flujo de trabajo.
# Los nombres de comando heredados con slash siguen funcionando mientras ECC migra fuera de `commands/`.

# La instalación con plugin usa la forma canónica con namespace
/ecc:plan "Add user authentication"

# La instalación manual mantiene la forma corta con slash:
# /plan "Add user authentication"

# Ver comandos disponibles
/plugin list ecc@ecc
```

**Eso es todo.** Ahora tienes acceso a 61 agentes, 246 skills y 76 command shims heredados.

### GUI del panel

Lanza el panel de escritorio para explorar visualmente los componentes de ECC:

```bash
npm run dashboard
# o
python3 ./ecc_dashboard.py
```

**Funciones:**
- Interfaz con pestañas: Agents, Skills, Commands, Rules, Settings
- Selector de tema oscuro/claro
- Personalización de fuente (familia y tamaño)
- Logotipo del proyecto en la cabecera y la barra de tareas
- Búsqueda y filtrado en todos los componentes

### Los comandos multimodelo requieren configuración adicional

> ADVERTENCIA: los comandos `multi-*` **no** están cubiertos por la instalación base de plugin/rules anterior.
>
> Para usar `/multi-plan`, `/multi-execute`, `/multi-backend`, `/multi-frontend` y `/multi-workflow`, también debes instalar el runtime `ccg-workflow`.
>
> Inicialízalo con `npx ccg-workflow`.
>
> Ese runtime proporciona las dependencias externas que esperan estos comandos, entre ellas:
> - `~/.claude/bin/codeagent-wrapper`
> - `~/.claude/.ccg/prompts/*`
>
> Sin `ccg-workflow`, estos comandos `multi-*` no funcionarán correctamente.

---

## Soporte multiplataforma

Este complemento ahora admite completamente **Windows, macOS y Linux**, además de una integración estrecha con los principales IDEs (Cursor, Zed, OpenCode, Antigravity) y harnesses de CLI. Todos los hooks y scripts se reescribieron en Node.js para lograr la máxima compatibilidad.

### Detección del gestor de paquetes

El complemento detecta automáticamente tu gestor de paquetes preferido (npm, pnpm, yarn o bun) con la siguiente prioridad:

1. **Variable de entorno**: `CLAUDE_PACKAGE_MANAGER`
2. **Configuración del proyecto**: `.claude/package-manager.json`
3. **`package.json`**: campo `packageManager`
4. **Lock file**: detección desde `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml` o `bun.lockb`
5. **Configuración global**: `~/.claude/package-manager.json`
6. **Fallback**: primer gestor de paquetes disponible

Para configurar tu gestor de paquetes preferido:

```bash
# Vía variable de entorno
export CLAUDE_PACKAGE_MANAGER=pnpm

# Vía configuración global
node scripts/setup-package-manager.js --global pnpm

# Vía configuración del proyecto
node scripts/setup-package-manager.js --project bun

# Detectar configuración actual
node scripts/setup-package-manager.js --detect
```

O usa el comando `/setup-pm` en Claude Code.

### Controles de tiempo de ejecución de hooks

Usa banderas de ejecución para ajustar la rigurosidad o desactivar hooks específicos temporalmente:

```bash
# Hook strictness profile (default: standard)
export ECC_HOOK_PROFILE=standard

# Comma-separated hook IDs to disable
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Cap SessionStart additional context (default: 8000 chars)
export ECC_SESSION_START_MAX_CHARS=4000

# Disable SessionStart additional context entirely for low-context/local-model setups
export ECC_SESSION_START_CONTEXT=off

# Keep context/scope/loop warnings but suppress API-rate cost estimates
export ECC_CONTEXT_MONITOR_COST_WARNINGS=off
```

Windows PowerShell:

```powershell
[Environment]::SetEnvironmentVariable('ECC_CONTEXT_MONITOR_COST_WARNINGS', 'off', 'User')
```

---

## Qué incluye

Este repositorio es un **complemento de Claude Code**: instálalo directamente o copia los componentes manualmente.

```
ECC/
|-- .claude-plugin/   # Plugin and marketplace manifests
|   |-- plugin.json         # Plugin metadata and component paths
|   |-- marketplace.json    # Marketplace catalog for /plugin marketplace add
|
|-- agents/           # 61 specialized subagents for delegation
|   |-- planner.md           # Feature implementation planning
|   |-- architect.md         # System design decisions
|   |-- tdd-guide.md         # Test-driven development
|   |-- code-reviewer.md     # Quality and security review
|   |-- security-reviewer.md # Vulnerability analysis
|   |-- build-error-resolver.md
|   |-- e2e-runner.md        # Playwright E2E testing
|   |-- refactor-cleaner.md  # Dead code cleanup
|   |-- doc-updater.md       # Documentation sync
|   |-- docs-lookup.md       # Documentation/API lookup
|   |-- chief-of-staff.md    # Communication triage and drafts
|   |-- loop-operator.md     # Autonomous loop execution
|   |-- harness-optimizer.md # Harness config tuning
|   |-- cpp-reviewer.md      # C++ code review
|   |-- cpp-build-resolver.md # C++ build error resolution
|   |-- fsharp-reviewer.md   # F# functional code review
|   |-- go-reviewer.md       # Go code review
|   |-- go-build-resolver.md # Go build error resolution
|   |-- python-reviewer.md   # Python code review
|   |-- database-reviewer.md # Database/Supabase review
|   |-- typescript-reviewer.md # TypeScript/JavaScript code review
|   |-- java-reviewer.md     # Java/Spring Boot code review
|   |-- java-build-resolver.md # Java/Maven/Gradle build errors
|   |-- kotlin-reviewer.md   # Kotlin/Android/KMP code review
|   |-- kotlin-build-resolver.md # Kotlin/Gradle build errors
|   |-- harmonyos-app-resolver.md # HarmonyOS/ArkTS app development
|   |-- rust-reviewer.md     # Rust code review
|   |-- rust-build-resolver.md # Rust build error resolution
|   |-- pytorch-build-resolver.md # PyTorch/CUDA training errors
|   |-- mle-reviewer.md      # Production ML pipeline, eval, serving, and monitoring review
|
|-- skills/           # Workflow definitions and domain knowledge
|   |-- coding-standards/           # Language best practices
|   |-- clickhouse-io/              # ClickHouse analytics, queries, data engineering
|   |-- backend-patterns/           # API, database, caching patterns
|   |-- frontend-patterns/          # React, Next.js patterns
|   |-- frontend-slides/            # HTML slide decks and PPTX-to-web presentation workflows (NEW)
|   |-- article-writing/            # Long-form writing in a supplied voice without generic AI tone (NEW)
|   |-- content-engine/             # Multi-platform social content and repurposing workflows (NEW)
|   |-- market-research/            # Source-attributed market, competitor, and investor research (NEW)
|   |-- investor-materials/         # Pitch decks, one-pagers, memos, and financial models (NEW)
|   |-- investor-outreach/          # Personalized fundraising outreach and follow-up (NEW)
|   |-- continuous-learning/        # Legacy v1 Stop-hook pattern extraction
|   |-- continuous-learning-v2/     # Instinct-based learning with confidence scoring
|   |-- iterative-retrieval/        # Progressive context refinement for subagents
|   |-- strategic-compact/          # Manual compaction suggestions (Longform Guide)
|   |-- tdd-workflow/               # TDD methodology
|   |-- security-review/            # Security checklist
|   |-- eval-harness/               # Verification loop evaluation (Longform Guide)
|   |-- verification-loop/          # Continuous verification (Longform Guide)
|   |-- videodb/                   # Video and audio: ingest, search, edit, generate, stream (NEW)
|   |-- golang-patterns/            # Go idioms and best practices
|   |-- golang-testing/             # Go testing patterns, TDD, benchmarks
|   |-- cpp-coding-standards/         # C++ coding standards from C++ Core Guidelines (NEW)
|   |-- cpp-testing/                # C++ testing with GoogleTest, CMake/CTest (NEW)
|   |-- django-patterns/            # Django patterns, models, views (NEW)
|   |-- django-security/            # Django security best practices (NEW)
|   |-- django-tdd/                 # Django TDD workflow (NEW)
|   |-- django-verification/        # Django verification loops (NEW)
|   |-- laravel-patterns/           # Laravel architecture patterns (NEW)
|   |-- laravel-security/           # Laravel security best practices (NEW)
|   |-- laravel-tdd/                # Laravel TDD workflow (NEW)
|   |-- laravel-verification/       # Laravel verification loops (NEW)
|   |-- python-patterns/            # Python idioms and best practices (NEW)
|   |-- python-testing/             # Python testing with pytest (NEW)
|   |-- quarkus-patterns/            # Java Quarkus patterns (NEW)
|   |-- quarkus-security/            # Quarkus security (NEW)
|   |-- quarkus-tdd/                 # Quarkus TDD (NEW)
|   |-- quarkus-verification/        # Quarkus verification (NEW)
|   |-- springboot-patterns/        # Java Spring Boot patterns (NEW)
|   |-- springboot-security/        # Spring Boot security (NEW)
|   |-- springboot-tdd/             # Spring Boot TDD (NEW)
|   |-- springboot-verification/    # Spring Boot verification (NEW)
|   |-- configure-ecc/              # Interactive installation wizard (NEW)
|   |-- security-scan/              # AgentShield security auditor integration (NEW)
|   |-- java-coding-standards/     # Java coding standards (NEW)
|   |-- jpa-patterns/              # JPA/Hibernate patterns (NEW)
|   |-- postgres-patterns/         # PostgreSQL optimization patterns (NEW)
|   |-- nutrient-document-processing/ # Document processing with Nutrient API (NEW)
|   |-- docs/examples/project-guidelines-template.md  # Template for project-specific skills
|   |-- database-migrations/         # Migration patterns (Prisma, Drizzle, Django, Go) (NEW)
|   |-- api-design/                  # REST API design, pagination, error responses (NEW)
|   |-- deployment-patterns/         # CI/CD, Docker, health checks, rollbacks (NEW)
|   |-- docker-patterns/            # Docker Compose, networking, volumes, container security (NEW)
|   |-- e2e-testing/                 # Playwright E2E patterns and Page Object Model (NEW)
|   |-- content-hash-cache-pattern/  # SHA-256 content hash caching for file processing (NEW)
|   |-- cost-aware-llm-pipeline/     # LLM cost optimization, model routing, budget tracking (NEW)
|   |-- regex-vs-llm-structured-text/ # Decision framework: regex vs LLM for text parsing (NEW)
|   |-- swift-actor-persistence/     # Thread-safe Swift data persistence with actors (NEW)
|   |-- swift-protocol-di-testing/   # Protocol-based DI for testable Swift code (NEW)
|   |-- search-first/               # Research-before-coding workflow (NEW)
|   |-- skill-stocktake/            # Audit skills and commands for quality (NEW)
|   |-- liquid-glass-design/         # iOS 26 Liquid Glass design system (NEW)
|   |-- foundation-models-on-device/ # Apple on-device LLM with FoundationModels (NEW)
|   |-- swift-concurrency-6-2/       # Swift 6.2 Approachable Concurrency (NEW)
|   |-- mle-workflow/               # Production ML data contracts, evals, deployment, monitoring (NEW)
|   |-- perl-patterns/             # Modern Perl 5.36+ idioms and best practices (NEW)
|   |-- perl-security/             # Perl security patterns, taint mode, safe I/O (NEW)
|   |-- perl-testing/              # Perl TDD with Test2::V0, prove, Devel::Cover (NEW)
|   |-- autonomous-loops/           # Autonomous loop patterns: sequential pipelines, PR loops, DAG orchestration (NEW)
|   |-- plankton-code-quality/      # Write-time code quality enforcement with Plankton hooks (NEW)
|
|-- commands/         # Maintained slash-entry compatibility; prefer skills/
|   |-- plan.md             # /plan - Implementation planning
|   |-- code-review.md      # /code-review - Quality review
|   |-- build-fix.md        # /build-fix - Fix build errors
|   |-- refactor-clean.md   # /refactor-clean - Dead code removal
|   |-- quality-gate.md     # /quality-gate - Verification gate
|   |-- learn.md            # /learn - Extract patterns mid-session (Longform Guide)
|   |-- learn-eval.md       # /learn-eval - Extract, evaluate, and save patterns (NEW)
|   |-- checkpoint.md       # /checkpoint - Save verification state (Longform Guide)
|   |-- setup-pm.md         # /setup-pm - Configure package manager
|   |-- go-review.md        # /go-review - Go code review (NEW)
|   |-- go-test.md          # /go-test - Go TDD workflow (NEW)
|   |-- go-build.md         # /go-build - Fix Go build errors (NEW)
|   |-- skill-create.md     # /skill-create - Generate skills from git history (NEW)
|   |-- instinct-status.md  # /instinct-status - View learned instincts (NEW)
|   |-- instinct-import.md  # /instinct-import - Import instincts (NEW)
|   |-- instinct-export.md  # /instinct-export - Export instincts (NEW)
|   |-- evolve.md           # /evolve - Cluster instincts into skills
|   |-- prune.md            # /prune - Delete expired pending instincts (NEW)
|   |-- pm2.md              # /pm2 - PM2 service lifecycle management (NEW)
|   |-- multi-plan.md       # /multi-plan - Multi-agent task decomposition (NEW)
|   |-- multi-execute.md    # /multi-execute - Orchestrated multi-agent workflows (NEW)
|   |-- multi-backend.md    # /multi-backend - Backend multi-service orchestration (NEW)
|   |-- multi-frontend.md   # /multi-frontend - Frontend multi-service orchestration (NEW)
|   |-- multi-workflow.md   # /multi-workflow - General multi-service workflows (NEW)
|   |-- sessions.md         # /sessions - Session history management
|   |-- test-coverage.md    # /test-coverage - Test coverage analysis
|   |-- update-docs.md      # /update-docs - Update documentation
|   |-- update-codemaps.md  # /update-codemaps - Update codemaps
|   |-- python-review.md    # /python-review - Python code review (NEW)
|-- legacy-command-shims/   # Opt-in archive for retired shims such as /tdd and /eval
|   |-- tdd.md              # /tdd - Prefer the tdd-workflow skill
|   |-- e2e.md              # /e2e - Prefer the e2e-testing skill
|   |-- eval.md             # /eval - Prefer the eval-harness skill
|   |-- verify.md           # /verify - Prefer the verification-loop skill
|   |-- orchestrate.md      # /orchestrate - Prefer dmux-workflows or multi-workflow
|
|-- rules/            # Always-follow guidelines (copy to ~/.claude/rules/ecc/)
|   |-- README.md            # Structure overview and installation guide
|   |-- common/              # Language-agnostic principles
|   |   |-- coding-style.md    # Immutability, file organization
|   |   |-- git-workflow.md    # Commit format, PR process
|   |   |-- testing.md         # TDD, 80% coverage requirement
|   |   |-- performance.md     # Model selection, context management
|   |   |-- patterns.md        # Design patterns, skeleton projects
|   |   |-- hooks.md           # Hook architecture, TodoWrite
|   |   |-- agents.md          # When to delegate to subagents
|   |   |-- security.md        # Mandatory security checks
|   |-- typescript/          # TypeScript/JavaScript specific
|   |-- python/              # Python specific
|   |-- golang/              # Go specific
|   |-- swift/               # Swift specific
|   |-- php/                 # PHP specific (NEW)
|   |-- arkts/               # HarmonyOS / ArkTS specific
|
|-- hooks/            # Trigger-based automations
|   |-- README.md                 # Hook documentation, recipes, and customization guide
|   |-- hooks.json                # All hooks config (PreToolUse, PostToolUse, Stop, etc.)
|   |-- memory-persistence/       # Session lifecycle hooks (Longform Guide)
|   |-- strategic-compact/        # Compaction suggestions (Longform Guide)
|
|-- scripts/          # Cross-platform Node.js scripts (NEW)
|   |-- lib/                     # Shared utilities
|   |   |-- utils.js             # Cross-platform file/path/system utilities
|   |   |-- package-manager.js   # Package manager detection and selection
|   |-- hooks/                   # Hook implementations
|   |   |-- session-start.js     # Load context on session start
|   |   |-- session-end.js       # Save state on session end
|   |   |-- pre-compact.js       # Pre-compaction state saving
|   |   |-- suggest-compact.js   # Strategic compaction suggestions
|   |   |-- evaluate-session.js  # Extract patterns from sessions
|   |-- setup-package-manager.js # Interactive PM setup
|
|-- tests/            # Test suite (NEW)
|   |-- lib/                     # Library tests
|   |-- hooks/                   # Hook tests
|   |-- run-all.js               # Run all tests
|
|-- contexts/         # Dynamic system prompt injection contexts (Longform Guide)
|   |-- dev.md              # Development mode context
|   |-- review.md           # Code review mode context
|   |-- research.md         # Research/exploration mode context
|
|-- examples/         # Example configurations and sessions
|   |-- CLAUDE.md             # Example project-level config
|   |-- user-CLAUDE.md        # Example user-level config
|   |-- saas-nextjs-CLAUDE.md   # Real-world SaaS (Next.js + Supabase + Stripe)
|   |-- go-microservice-CLAUDE.md # Real-world Go microservice (gRPC + PostgreSQL)
|   |-- django-api-CLAUDE.md      # Real-world Django REST API (DRF + Celery)
|   |-- laravel-api-CLAUDE.md     # Real-world Laravel API (PostgreSQL + Redis) (NEW)
|   |-- rust-api-CLAUDE.md        # Real-world Rust API (Axum + SQLx + PostgreSQL) (NEW)
|
|-- mcp-configs/      # MCP server configurations
|   |-- mcp-servers.json    # GitHub, Supabase, Vercel, Railway, etc.
|
|-- ecc_dashboard.py  # Desktop GUI dashboard (Tkinter)
|
|-- assets/           # Assets for dashboard
|   |-- images/
|       |-- ecc-logo.png
|
|-- marketplace.json  # Self-hosted marketplace config (for /plugin marketplace add)
```

---

## Herramientas del ecosistema

### Skill Creator

Dos formas de generar skills de Claude Code a partir de tu repositorio:

#### Opción A: análisis local (incluido)

Usa el comando `/skill-create` para hacer análisis local sin servicios externos:

```bash
/skill-create                    # Analiza el repo actual
/skill-create --instincts        # También genera instincts para continuous-learning-v2
```

Esto analiza tu historial de git localmente y genera archivos `SKILL.md`.

#### Opción B: GitHub App (avanzado)

Para funciones avanzadas (10k+ commits, PRs automáticos, compartición en equipo):

[Instalar GitHub App](https://github.com/apps/skill-creator) | [ecc.tools](https://ecc.tools)

```bash
# Comenta en cualquier issue:
/skill-creator analyze

# O se activa automáticamente con push a la rama por defecto
```

Ambas opciones crean:
- **Archivos `SKILL.md`** - Skills listas para usar en Claude Code
- **Colecciones de instincts** - Para `continuous-learning-v2`
- **Extracción de patrones** - Aprende a partir de tu historial de commits

### AgentShield — Auditor de seguridad

> Creado en el hackathon de Claude Code (Cerebral Valley x Anthropic, feb. de 2026). 1282 pruebas, 98% de cobertura, 102 rules de análisis estático.

Escanea tu configuración de Claude Code en busca de vulnerabilidades, configuraciones incorrectas y riesgos de inyección.

```bash
# Escaneo rápido (sin instalación)
npx ecc-agentshield scan

# Corregir automáticamente problemas seguros
npx ecc-agentshield scan --fix

# Análisis profundo con tres agentes Opus 4.6
npx ecc-agentshield scan --opus --stream

# Generar una configuración segura desde cero
npx ecc-agentshield init
```

**Qué escanea:** `CLAUDE.md`, `settings.json`, configuraciones MCP, hooks, definiciones de agents y skills en 5 categorías: detección de secretos (14 patrones), auditoría de permisos, análisis de inyección en hooks, perfilado de riesgo de servidores MCP y revisión de configuración de agents.

**El flag `--opus`** ejecuta tres agentes Claude Opus 4.6 en un pipeline red-team/blue-team/auditor. El atacante encuentra cadenas de explotación, el defensor evalúa protecciones y el auditor sintetiza ambos resultados en una evaluación de riesgos priorizada. Razonamiento adversarial, no solo pattern matching.

**Formatos de salida:** Terminal (clasificación por colores de A a F), JSON (pipelines de CI), Markdown y HTML. Código de salida 2 cuando hay hallazgos críticos para compuertas de build.

Usa `/security-scan` en Claude Code para ejecutarlo, o añádelo a tu CI con la [GitHub Action](https://github.com/affaan-m/agentshield).

[GitHub](https://github.com/affaan-m/agentshield) | [npm](https://www.npmjs.com/package/ecc-agentshield)

### Continuous Learning v2

El sistema de aprendizaje basado en instincts aprende automáticamente tus patrones:

```bash
/instinct-status        # Muestra instincts aprendidos con su confianza
/instinct-import <file> # Importa instincts de otras personas
/instinct-export        # Exporta tus instincts para compartirlos
/evolve                 # Agrupa instincts relacionados en skills
```

Consulta `skills/continuous-learning-v2/` para ver la documentación completa.
Mantén `continuous-learning/` solo cuando explícitamente quieras el flujo heredado v1 de Stop-hook que aprende skills.

---

## Requisitos

### Versión de Claude Code CLI

**Versión mínima: v2.1.0 o superior**

Este plugin requiere Claude Code CLI v2.1.0+ debido a cambios en la forma en que el sistema de plugins maneja los hooks.

Comprueba tu versión:
```bash
claude --version
```

### Importante: comportamiento de carga automática de hooks

> ADVERTENCIA: **para contribuidores:** NO añadas un campo `"hooks"` a `.claude-plugin/plugin.json`. Esto está forzado por una prueba de regresión.

Claude Code v2.1+ **carga automáticamente** `hooks/hooks.json` desde cualquier plugin instalado por convención. Declararlo explícitamente en `plugin.json` provoca un error de detección duplicada:

```
Duplicate hooks file detected: ./hooks/hooks.json resolves to already-loaded file
```

**Historial:** esto ha provocado ciclos repetidos de fix/revert en este repo ([#29](https://github.com/affaan-m/ECC/issues/29), [#52](https://github.com/affaan-m/ECC/issues/52), [#103](https://github.com/affaan-m/ECC/issues/103)). El comportamiento cambió entre versiones de Claude Code, lo que generó confusión. Ahora tenemos una prueba de regresión para evitar que vuelva a introducirse.

---

## Instalación

### Opción 1: instalar como complemento (recomendado)

La forma más sencilla de usar este repo es instalarlo como plugin de Claude Code:

```bash
# Añade este repo como marketplace
/plugin marketplace add https://github.com/affaan-m/ECC

# Instala el plugin
/plugin install ecc@ecc
```

O añádelo directamente a tu `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "ecc": {
      "source": {
        "source": "github",
        "repo": "affaan-m/ECC"
      }
    }
  },
  "enabledPlugins": {
    "ecc@ecc": true
  }
}
```

Esto te da acceso inmediato a todos los comandos, agentes, skills y hooks.

> **Nota:** el sistema de plugins de Claude Code no admite distribuir `rules` a través de plugins ([limitación upstream](https://code.claude.com/docs/en/plugins-reference)). Debes instalar las rules manualmente:
>
> ```bash
> # Clona el repo primero
> git clone https://github.com/affaan-m/ECC.git
> cd ECC
>
> # Opción A: rules a nivel de usuario (se aplican a todos los proyectos)
> mkdir -p ~/.claude/rules/ecc
> cp -r rules/common ~/.claude/rules/ecc/
> cp -r rules/typescript ~/.claude/rules/ecc/   # elige tu stack
> cp -r rules/python ~/.claude/rules/ecc/
> cp -r rules/golang ~/.claude/rules/ecc/
> cp -r rules/php ~/.claude/rules/ecc/
>
> # Opción B: rules a nivel de proyecto (se aplican solo al proyecto actual)
> mkdir -p .claude/rules/ecc
> cp -r rules/common .claude/rules/ecc/
> cp -r rules/typescript .claude/rules/ecc/     # elige tu stack
> ```

---

### Opción 2: instalación manual

Si prefieres controlar manualmente lo que se instala:

```bash
# Clona el repo
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Copia los agents a tu configuración de Claude
cp agents/*.md ~/.claude/agents/

# Copia los directorios de rules (common + específicos por lenguaje)
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
cp -r rules/typescript ~/.claude/rules/ecc/   # elige tu stack
cp -r rules/python ~/.claude/rules/ecc/
cp -r rules/golang ~/.claude/rules/ecc/
cp -r rules/php ~/.claude/rules/ecc/
cp -r rules/arkts ~/.claude/rules/ecc/

# Copia primero las skills (superficie principal del flujo de trabajo)
# Recomendado (usuarios nuevos): solo skills core/general
mkdir -p ~/.claude/skills/ecc
cp -r .agents/skills/* ~/.claude/skills/ecc/
cp -r skills/search-first ~/.claude/skills/ecc/

# Opcional: añade skills de nicho o específicas de framework solo cuando las necesites
# for s in django-patterns django-tdd laravel-patterns springboot-patterns quarkus-patterns; do
# cp -r skills/$s ~/.claude/skills/ecc/
# done

# Opcional: mantén la compatibilidad con slash-commands durante la migración
mkdir -p ~/.claude/commands
cp commands/*.md ~/.claude/commands/

# Los shims retirados viven en `legacy-command-shims/commands/`.
# Copia archivos individuales desde ahí solo si aún necesitas nombres antiguos como `/tdd`.
```

#### Instalar hooks

No copies el archivo bruto del repo `hooks/hooks.json` dentro de `~/.claude/settings.json` ni `~/.claude/hooks/hooks.json`. Ese archivo está orientado al plugin/repo y debe instalarse mediante el instalador de ECC o cargarse como plugin, así que copiarlo directamente no es una ruta manual soportada.

Usa el instalador para instalar solo el runtime de hooks de Claude, de forma que las rutas de comandos se reescriban correctamente:

```bash
# macOS / Linux
bash ./install.sh --target claude --modules hooks-runtime
```

```powershell
# Windows PowerShell
pwsh -File .\install.ps1 --target claude --modules hooks-runtime
```

Eso escribe los hooks resueltos en `~/.claude/hooks/hooks.json` y deja intacto cualquier `~/.claude/settings.json` existente.

Si instalaste ECC mediante `/plugin install`, no copies esos hooks a `settings.json`. Claude Code v2.1+ ya carga automáticamente `hooks/hooks.json` del plugin, y duplicarlos en `settings.json` provoca ejecuciones duplicadas y conflictos de hooks entre plataformas.

Nota para Windows: el directorio de configuración de Claude es `%USERPROFILE%\\.claude`, no `~/claude`.

#### Configurar MCPs

Las instalaciones del plugin de Claude intencionalmente no habilitan automáticamente las definiciones de servidores MCP incluidas en ECC. Esto evita nombres de herramientas MCP excesivamente largos en gateways estrictos de terceros, mientras mantiene disponible la configuración manual de MCP.

Usa el comando `/mcp` de Claude Code o una configuración de MCP gestionada por CLI para cambios en vivo de servidores de Claude Code. Usa `/mcp` para deshabilitaciones en tiempo de ejecución; Claude Code persiste esas decisiones en `~/.claude.json`.

Para acceso local al repo con MCP, copia las definiciones deseadas de servidores MCP desde `mcp-configs/mcp-servers.json` a un `.mcp.json` a nivel de proyecto.

Si ya ejecutas tus propias copias de los MCP incluidos con ECC, define:

```bash
export ECC_DISABLED_MCPS="github,context7,exa,playwright,sequential-thinking,memory"
```

Los flujos de instalación gestionados por ECC y la sincronización con Codex omitirán o eliminarán esos servidores incluidos en lugar de volver a añadir duplicados. `ECC_DISABLED_MCPS` es un filtro de instalación/sincronización de ECC, no un interruptor en vivo de Claude Code.

**Importante:** reemplaza los marcadores `YOUR_*_HERE` por tus claves de API reales.

---

## Conceptos clave

### Agents

Los subagentes manejan tareas delegadas con un alcance limitado. Ejemplo:

```markdown
---
name: code-reviewer
description: Reviews code for quality, security, and maintainability
tools: ["Read", "Grep", "Glob", "Bash"]
model: opus
---

You are a senior code reviewer...
```

### Skills

Las skills son la superficie principal del flujo de trabajo. Pueden ser invocadas directamente, sugeridas automáticamente y reutilizadas por los agentes. ECC aún incluye `commands/` mantenidos durante la migración, mientras que los shims con nombres cortos retirados viven en `legacy-command-shims/` solo para habilitación explícita. El desarrollo de nuevos flujos de trabajo debe ir a `skills/` primero.

```markdown
# TDD Workflow

1. Define interfaces first
2. Write failing tests (RED)
3. Implement minimal code (GREEN)
4. Refactor (IMPROVE)
5. Verify 80%+ coverage
```

### Hooks

Los hooks se disparan en eventos de las herramientas. Ejemplo: advertencia sobre console.log:

```json
{
  "matcher": "tool == \"Edit\" && tool_input.file_path matches \"\\\\.(ts|tsx|js|jsx)$\"",
  "hooks": [{
    "type": "command",
    "command": "#!/bin/bash\ngrep -n 'console\\.log' \"$file_path\" && echo '[Hook] Remove console.log' >&2"
  }]
}
```

### Rules

Las reglas (Rules) son directrices que siempre deben seguirse, organizadas en `common/` (agnóstico al lenguaje) + directorios específicos de lenguaje:

```
rules/
  common/          # Principios universales (instálalos siempre)
  typescript/      # Patrones y herramientas específicas de TS/JS
  python/          # Patrones y herramientas específicas de Python
  golang/          # Patrones y herramientas específicas de Go
  swift/           # Patrones y herramientas específicas de Swift
  php/             # Patrones y herramientas específicas de PHP
  arkts/           # Patrones y restricciones de HarmonyOS / ArkTS
```

Consulta [`rules/README.md`](../../rules/README.md) para detalles de instalación y estructura.

---

## ¿Qué agente debo usar?

¿No estás seguro de por dónde empezar? Usa esta referencia rápida. Las skills son la superficie de flujo de trabajo canónica; las entradas de slash mantenidas siguen disponibles para los flujos centrados en comandos.

| Quiero... | Usa esta superficie | Agente utilizado |
|--------------|-----------------|------------|
| Planificar una nueva funcionalidad | `/ecc:plan "Add auth"` | planner |
| Diseñar la arquitectura del sistema | `/ecc:plan` + architect agent | architect |
| Escribir código con tests primero | `tdd-workflow` skill | tdd-guide |
| Revisar código que acabo de escribir | `/code-review` | code-reviewer |
| Corregir una compilación fallida | `/build-fix` | build-error-resolver |
| Ejecutar pruebas end-to-end | `e2e-testing` skill | e2e-runner |
| Encontrar vulnerabilidades de seguridad | `/security-scan` | security-reviewer |
| Eliminar código muerto | `/refactor-clean` | refactor-cleaner |
| Actualizar documentación | `/update-docs` | doc-updater |
| Revisar código Go | `/go-review` | go-reviewer |
| Revisar código Python | `/python-review` | python-reviewer |
| Revisar código F# | *(invoca `fsharp-reviewer` directamente)* | fsharp-reviewer |
| Revisar código TypeScript/JavaScript | *(invoca `typescript-reviewer` directamente)* | typescript-reviewer |
| Desarrollar apps HarmonyOS | *(invoca `harmonyos-app-resolver` directamente)* | harmonyos-app-resolver |
| Auditar consultas de base de datos | *(delegación automática)* | database-reviewer |
| Revisar cambios de ML en producción | `mle-workflow` skill + agente `mle-reviewer` | mle-reviewer |

### Flujos de trabajo comunes

Las formas con slash de abajo se muestran donde todavía forman parte de la superficie de comandos mantenida. Los shims retirados de nombre corto como `/tdd` y `/eval` viven en `legacy-command-shims/` y solo deben habilitarse de forma explícita.

**Empezar una nueva funcionalidad:**
```
/ecc:plan "Add user authentication with OAuth"
                                              → planner crea el plano de implementación
tdd-workflow skill                            → tdd-guide obliga a escribir los tests primero
/code-review                                  → code-reviewer revisa tu trabajo
```

**Corregir un bug:**
```
tdd-workflow skill                            → tdd-guide: escribe un test fallido que lo reproduzca
                                              → implementa la corrección y verifica que el test pase
/code-review                                  → code-reviewer: detecta regresiones
```

**Prepararse para producción:**
```
/security-scan                                → security-reviewer: auditoría OWASP Top 10
e2e-testing skill                             → e2e-runner: pruebas de flujos críticos de usuario
/test-coverage                                → verify 80%+ coverage
```

---

## Preguntas frecuentes

<details>
<summary><b>¿Cómo verifico qué agents/comandos están instalados?</b></summary>

```bash
/plugin list ecc@ecc
```

Esto muestra todos los agents, comandos y skills disponibles desde el plugin.
</details>

<details>
<summary><b>Mis hooks no funcionan / veo errores de "Duplicate hooks file"</b></summary>

Este es el problema más común. **NO añadas un campo `"hooks"` a `.claude-plugin/plugin.json`.** Claude Code v2.1+ carga automáticamente `hooks/hooks.json` desde los plugins instalados. Declararlo explícitamente provoca errores de detección duplicada. Consulta [#29](https://github.com/affaan-m/ECC/issues/29), [#52](https://github.com/affaan-m/ECC/issues/52), [#103](https://github.com/affaan-m/ECC/issues/103).
</details>

<details>
<summary><b>¿Puedo usar ECC con Claude Code sobre un endpoint de API personalizado o un model gateway?</b></summary>

Sí. ECC no hardcodea la configuración de transporte alojada por Anthropic. Se ejecuta localmente a través de la superficie normal de CLI/plugin de Claude Code, así que funciona con:

- Anthropic-hosted Claude Code
- Configuraciones oficiales de gateway de Claude Code usando `ANTHROPIC_BASE_URL` y `ANTHROPIC_AUTH_TOKEN`
- Endpoints personalizados compatibles que hablen la API de Anthropic que Claude Code espera

Ejemplo mínimo:

```bash
export ANTHROPIC_BASE_URL=https://your-gateway.example.com
export ANTHROPIC_AUTH_TOKEN=your-token
claude
```

Si tu gateway reasigna nombres de modelos, configura eso en Claude Code y no en ECC. Los hooks, skills, comandos y rules de ECC son agnósticos al proveedor del modelo una vez que el CLI `claude` ya funciona.

Referencias oficiales:
- [Claude Code LLM gateway docs](https://docs.anthropic.com/en/docs/claude-code/llm-gateway)
- [Claude Code model configuration docs](https://docs.anthropic.com/en/docs/claude-code/model-config)

</details>

<details>
<summary><b>Mi ventana de contexto se está reduciendo / Claude se queda sin contexto</b></summary>

Demasiados servidores MCP se comen tu contexto. Cada descripción de herramienta MCP consume tokens de tu ventana de 200k, reduciéndola potencialmente hasta ~70k. El contexto de SessionStart está limitado a 8000 caracteres por defecto; bájalo con `ECC_SESSION_START_MAX_CHARS=4000` o desactívalo con `ECC_SESSION_START_CONTEXT=off` en configuraciones de modelos locales o de bajo contexto.

**Solución:** desactiva los MCP no usados desde Claude Code con `/mcp`. Claude Code escribe esas decisiones de runtime en `~/.claude.json`; `.claude/settings.json` y `.claude/settings.local.json` no son interruptores fiables para servidores MCP ya cargados.

Mantén menos de 10 MCPs habilitados y menos de 80 herramientas activas.
</details>

<details>
<summary><b>¿Puedo usar solo algunos componentes (por ejemplo, solo agents)?</b></summary>

Sí. Usa la Opción 2 (instalación manual) y copia solo lo que necesites:

```bash
# Solo agents
cp agents/*.md ~/.claude/agents/

# Solo rules
mkdir -p ~/.claude/rules/ecc/
cp -r rules/common ~/.claude/rules/ecc/
```

Cada componente es totalmente independiente.
</details>

<details>
<summary><b>¿Esto funciona con Cursor / OpenCode / Codex / Antigravity / GitHub Copilot?</b></summary>

Sí. ECC es multiplataforma:
- **Cursor**: Configuraciones pretraducidas en `.cursor/`. Consulta [Soporte para Cursor IDE](#soporte-para-cursor-ide).
- **Gemini CLI**: Soporte experimental a nivel de proyecto mediante `.gemini/GEMINI.md` y la infraestructura compartida del instalador.
- **OpenCode**: Soporte completo de plugin en `.opencode/`. Consulta [Soporte para OpenCode](#soporte-para-opencode).
- **Codex**: Soporte de primer nivel para la app de macOS y la CLI, con protecciones contra drift del adaptador y fallback de SessionStart. Consulta el PR [#257](https://github.com/affaan-m/ECC/pull/257).
- **GitHub Copilot (VS Code)**: Capa de instrucciones y prompts mediante `.github/copilot-instructions.md`, `.vscode/settings.json` y `.github/prompts/`. Consulta [Soporte para GitHub Copilot](#soporte-para-github-copilot).
- **Antigravity**: Configuración fuertemente integrada para workflows, skills y rules aplanadas en `.agent/`. Consulta [Antigravity Guide](guides/ANTIGRAVITY-GUIDE.md).
- **JoyCode / CodeBuddy**: Adaptadores de instalación selectiva a nivel de proyecto para comandos, agents, skills y rules aplanadas. Consulta [JoyCode Adapter Guide](guides/JOYCODE-GUIDE.md).
- **Qwen CLI**: Adaptador de instalación selectiva en el directorio home para comandos, agents, skills, rules y configuración de Qwen. Consulta [Qwen CLI Adapter Guide](guides/QWEN-GUIDE.md).
- **Zed**: Adaptador de instalación selectiva a nivel de proyecto para `.zed/settings.json`, rules aplanadas, comandos, agents y skills.
- **Harnesses no nativos**: Ruta manual de fallback para Grok y otras interfaces similares. Consulta [Manual Adaptation Guide](guides/MANUAL-ADAPTATION-GUIDE.md).
- **Claude Code**: Nativo; este es el objetivo principal.
</details>

<details>
<summary><b>¿Cómo contribuyo con una nueva skill o agent?</b></summary>

Consulta [CONTRIBUTING.md](CONTRIBUTING.md). La versión corta:
1. Haz fork del repo.
2. Crea tu skill en `skills/your-skill-name/SKILL.md` (con frontmatter YAML).
3. O crea un agent en `agents/your-agent.md`.
4. Envía un PR con una descripción clara de lo que hace y cuándo usarlo.
</details>

---

## Ejecución de pruebas

El plugin incluye una suite de pruebas completa:

```bash
# Ejecutar todas las pruebas
node tests/run-all.js

# Ejecutar archivos de prueba individuales
node tests/lib/utils.test.js
node tests/lib/package-manager.test.js
node tests/hooks/hooks.test.js
```

---

## Contribuir

**Las contribuciones son bienvenidas y se animan activamente.**

Este repo está pensado como un recurso comunitario. Si tienes:
- agents o skills útiles
- hooks ingeniosos
- mejores configuraciones MCP
- rules mejoradas

Contribuye. Consulta [CONTRIBUTING.md](CONTRIBUTING.md) para ver las pautas.

### Ideas para contribuir

- Skills específicas por lenguaje (Rust, C#, Kotlin, Java) — Go, Python, Perl, Swift, TypeScript y HarmonyOS/ArkTS ya están incluidos
- Configuraciones específicas por framework (Rails, FastAPI) — Django, NestJS, Spring Boot y Laravel ya están incluidos
- DevOps agents (Kubernetes, Terraform, AWS, Docker)
- Testing strategies (different frameworks, visual regression)
- Domain-specific knowledge (ML, data engineering, mobile)

### Notas del ecosistema comunitario

Estos no vienen incluidos con ECC ni están auditados por este repo, pero vale la pena conocerlos si estás explorando el ecosistema más amplio de skills para Claude Code:

- [claude-seo](https://github.com/AgriciDaniel/claude-seo) — SEO-focused skill and agent collection
- [claude-ads](https://github.com/AgriciDaniel/claude-ads) — Ad-audit and paid-growth workflow collection
- [claude-cybersecurity](https://github.com/AgriciDaniel/claude-cybersecurity) — Security-oriented skill and agent collection

---

## Soporte para Cursor IDE

ECC ofrece soporte para Cursor IDE con hooks, rules, agents, skills, comandos y configuraciones MCP adaptadas al layout de proyecto de Cursor.

### Inicio rápido (Cursor)

```bash
# macOS/Linux
./install.sh --target cursor typescript
./install.sh --target cursor python golang swift php
```

```powershell
# Windows PowerShell
.\install.ps1 --target cursor typescript
.\install.ps1 --target cursor python golang swift php
```

### Qué incluye

| Componente | Cantidad | Detalles |
|-----------|----------|----------|
| Eventos de hooks | 15 | `sessionStart`, `beforeShellExecution`, `afterFileEdit`, `beforeMCPExecution`, `beforeSubmitPrompt` y 10 más |
| Scripts de hooks | 16 | Scripts ligeros en Node.js que delegan a `scripts/hooks/` mediante un adaptador compartido |
| Rules | 34 | 9 common (`alwaysApply`) + 25 específicas por lenguaje (TypeScript, Python, Go, Swift, PHP) |
| Agents | 48 | `.cursor/agents/ecc-*.md` al instalarse; prefijados para evitar colisiones con agents del usuario o del marketplace |
| Skills | Compartidas + incluidas | `.cursor/skills/` para añadidos traducidos |
| Commands | Compartidos | `.cursor/commands/` si se instalan |
| Configuración MCP | Compartida | `.cursor/mcp.json` si se instala |

### Notas sobre la carga en Cursor

ECC no instala el `AGENTS.md` raíz dentro de `.cursor/`. Cursor trata los archivos `AGENTS.md` anidados como contexto de directorio, así que copiar la identidad del repo de ECC dentro de un proyecto host contaminaría ese proyecto.

El comportamiento de carga nativo de Cursor puede variar según la build. ECC instala agents como `.cursor/agents/ecc-*.md`; si tu build de Cursor no expone agents de proyecto, esos archivos siguen funcionando como definiciones de referencia explícitas en lugar de contexto global oculto del prompt.

### Arquitectura de hooks (patrón de adaptador DRY)

Cursor tiene **más eventos de hooks que Claude Code** (20 frente a 8). El módulo `.cursor/hooks/adapter.js` transforma el JSON de stdin de Cursor al formato de Claude Code, lo que permite reutilizar `scripts/hooks/*.js` existentes sin duplicación.

```
JSON stdin de Cursor → adapter.js → transforma → scripts/hooks/*.js
                                             (compartidos con Claude Code)
```

Hooks clave:
- **beforeShellExecution** — Bloquea dev servers fuera de tmux (`exit 2`), revisión antes de `git push`
- **afterFileEdit** — Autoformato + chequeo de TypeScript + advertencia por `console.log`
- **beforeSubmitPrompt** — Detecta secretos (patrones `sk-`, `ghp_`, `AKIA`) en prompts
- **beforeTabFileRead** — Bloquea que Tab lea archivos `.env`, `.key`, `.pem` (`exit 2`)
- **beforeMCPExecution / afterMCPExecution** — Registro de auditoría de MCP

### Formato de rules

Las rules de Cursor usan frontmatter YAML con `description`, `globs` y `alwaysApply`:

```yaml
---
description: "TypeScript coding style extending common rules"
globs: ["**/*.ts", "**/*.tsx", "**/*.js", "**/*.jsx"]
alwaysApply: false
---
```

---

## Soporte para la app de Codex en macOS + CLI

ECC ofrece **soporte de primer nivel para Codex** tanto en la app de macOS como en la CLI, con una configuración de referencia, un complemento `AGENTS.md` específico para Codex y skills compartidas.

### Inicio rápido (Codex App + CLI)

```bash
# Ejecuta Codex CLI en el repo: `AGENTS.md` y `.codex/` se detectan automáticamente
codex

# Configuración automática: sincroniza recursos de ECC (AGENTS.md, skills, servidores MCP) en ~/.codex
npm install && bash scripts/sync-ecc-to-codex.sh
# o: pnpm install && bash scripts/sync-ecc-to-codex.sh
# o: yarn install && bash scripts/sync-ecc-to-codex.sh
# o: bun install && bash scripts/sync-ecc-to-codex.sh

# O manualmente: copia la configuración de referencia a tu directorio home
cp .codex/config.toml ~/.codex/config.toml
```

El script de sincronización fusiona con seguridad los servidores MCP de ECC dentro de tu `~/.codex/config.toml` existente usando una estrategia de **solo añadir**: nunca elimina ni modifica tus servidores ya existentes. Ejecútalo con `--dry-run` para previsualizar cambios o con `--update-mcp` para forzar la actualización de los servidores de ECC a la configuración recomendada más reciente.

Para Context7, ECC usa el nombre de sección canónico de Codex `[mcp_servers.context7]` mientras sigue lanzando el paquete `@upstash/context7-mcp`. Si ya tienes una entrada heredada `[mcp_servers.context7-mcp]`, `--update-mcp` la migra al nombre de sección canónico.

App de Codex en macOS:
- Abre este repositorio como tu workspace.
- El `AGENTS.md` raíz se detecta automáticamente.
- `.codex/config.toml` y `.codex/agents/*.toml` funcionan mejor si se mantienen a nivel de proyecto.
- El `.codex/config.toml` de referencia intencionalmente no fija `model` ni `model_provider`, por lo que Codex usa su valor por defecto actual salvo que lo sobrescribas.
- Opcional: copia `.codex/config.toml` a `~/.codex/config.toml` para valores globales; mantén los archivos de roles multiagente a nivel de proyecto salvo que también copies `.codex/agents/`.

### Qué incluye

| Componente | Cantidad | Detalles |
|-----------|----------|----------|
| Configuración | 1 | `.codex/config.toml` — aprobaciones/sandbox/web_search de nivel superior, servidores MCP, notificaciones y perfiles |
| AGENTS.md | 2 | Raíz (universal) + `.codex/AGENTS.md` (complemento específico de Codex) |
| Skills | 32 | `.agents/skills/` — `SKILL.md` + `agents/openai.yaml` por skill |
| Servidores MCP | 6 | GitHub, Context7, Exa, Memory, Playwright, Sequential Thinking (7 con Supabase vía sincronización `--update-mcp`) |
| Perfiles | 2 | `strict` (sandbox de solo lectura) y `yolo` (autoaprobación total) |
| Roles de agent | 3 | `.codex/agents/` — `explorer`, `reviewer`, `docs-researcher` |

### Skills

Las skills en `.agents/skills/` se cargan automáticamente en Codex:

Las skills canónicas de Anthropic como `claude-api`, `frontend-design` y `skill-creator` intencionalmente no se vuelven a incluir aquí. Instálalas desde [`anthropics/skills`](https://github.com/anthropics/skills) cuando quieras las versiones oficiales.

| Skill | Descripción |
|-------|-------------|
| agent-introspection-debugging | Debug agent behavior, routing, and prompt boundaries |
| agent-sort | Sort agent catalogs and assignment surfaces |
| api-design | REST API design patterns |
| article-writing | Long-form writing from notes and voice references |
| backend-patterns | Diseño de API, base de datos y caché |
| brand-voice | Perfiles de estilo de escritura derivados de contenido real |
| bun-runtime | Bun como runtime, gestor de paquetes, bundler y ejecutor de tests |
| coding-standards | Estándares universales de código |
| content-engine | Contenido social nativo de plataforma y reutilización |
| crosspost | Distribución de contenido multiplataforma en X, LinkedIn y Threads |
| deep-research | Investigación multisource con síntesis y atribución de fuentes |
| dmux-workflows | Orquestación multiagente usando el gestor de paneles tmux |
| documentation-lookup | Documentación actualizada de librerías y frameworks vía Context7 MCP |
| e2e-testing | Pruebas E2E con Playwright |
| eval-harness | Desarrollo guiado por evals |
| everything-claude-code | Convenciones y patrones de desarrollo para el proyecto |
| exa-search | Búsqueda neuronal vía Exa MCP para web, código e investigación de empresas |
| fal-ai-media | Generación unificada de medios para imágenes, video y audio |
| frontend-patterns | Patrones de React/Next.js |
| frontend-slides | Presentaciones HTML, conversión de PPTX y exploración visual de estilo |
| investor-materials | Decks, memos, modelos y one-pagers |
| investor-outreach | Outreach personalizado, seguimientos y blurbs de introducción |
| market-research | Investigación de mercado y competidores con atribución de fuentes |
| mcp-server-patterns | Construcción de servidores MCP con el SDK de Node/TypeScript |
| nextjs-turbopack | Next.js 16+ y bundling incremental con Turbopack |
| product-capability | Traducir objetivos de producto en mapas de capacidades acotados |
| security-review | Checklist integral de seguridad |
| strategic-compact | Gestión de contexto |
| tdd-workflow | Desarrollo guiado por pruebas con 80%+ de cobertura |
| verification-loop | Build, tests, lint, typecheck y seguridad |
| video-editing | Flujos de edición de video asistidos por IA con FFmpeg y Remotion |
| x-api | Integración con la API de X/Twitter para publicar y medir analíticas |

### Limitación clave

Codex **todavía no ofrece paridad de ejecución de hooks al estilo de Claude**. La aplicación de ECC allí se basa en instrucciones mediante `AGENTS.md`, overrides opcionales con `model_instructions_file` y configuraciones de sandbox/aprobación.

### Soporte multiagente

Las builds actuales de Codex soportan flujos de trabajo multiagente estables.

- Habilita `features.multi_agent = true` en `.codex/config.toml`
- Define roles bajo `[agents.<name>]`
- Apunta cada rol a un archivo dentro de `.codex/agents/`
- Usa `/agent` en la CLI para inspeccionar o dirigir agents hijos

ECC incluye tres configuraciones de rol de ejemplo:

| Rol | Propósito |
|------|-----------|
| `explorer` | Recolección de evidencia del codebase en modo solo lectura antes de editar |
| `reviewer` | Revisión de corrección, seguridad y tests faltantes |
| `docs_researcher` | Verificación de documentación y API antes de cambios en releases/docs |

---

## Soporte para Zed

ECC ofrece soporte para proyectos Zed mediante un adaptador conservador `.zed` para configuración local al proyecto, rules aplanadas, agents, comandos y skills.

```bash
./install.sh --profile minimal --target zed
```

```powershell
.\install.ps1 --profile minimal --target zed
```

El adaptador escribe archivos gestionados por ECC dentro de `.zed/` y mantiene las credenciales BYOK/OpenRouter fuera del repo. Configura la cuenta de Zed o las claves de API desde la propia UI de configuración de Zed o desde tu configuración local de usuario.

---

## Soporte para OpenCode

ECC ofrece **soporte completo para OpenCode**, incluyendo plugins y hooks.

### Inicio rápido

```bash
# Instala OpenCode
npm install -g opencode

# Ejecuta en la raíz del repositorio
opencode
```

La configuración se detecta automáticamente desde `.opencode/opencode.json`.

### Paridad de funciones

| Función | Claude Code | OpenCode | Estado |
|---------|-------------|----------|--------|
| Agents | PASS: 61 agents | PASS: 12 agents | **Claude Code va por delante** |
| Commands | PASS: 76 commands | PASS: 35 commands | **Claude Code va por delante** |
| Skills | PASS: 246 skills | PASS: 37 skills | **Claude Code va por delante** |
| Hooks | PASS: 8 tipos de evento | PASS: 11 eventos | **OpenCode tiene más** |
| Rules | PASS: 29 rules | PASS: 13 instrucciones | **Claude Code va por delante** |
| Servidores MCP | PASS: 14 servidores | PASS: Completo | **Paridad total** |
| Custom Tools | PASS: Vía hooks | PASS: 6 herramientas nativas | **OpenCode es mejor aquí** |

### Soporte de hooks vía plugins

El sistema de plugins de OpenCode es MÁS sofisticado que el de Claude Code, con 20+ tipos de evento:

| Hook de Claude Code | Evento de plugin en OpenCode |
|--------------------|------------------------------|
| PreToolUse | `tool.execute.before` |
| PostToolUse | `tool.execute.after` |
| Stop | `session.idle` |
| SessionStart | `session.created` |
| SessionEnd | `session.deleted` |

**Eventos adicionales de OpenCode**: `file.edited`, `file.watcher.updated`, `message.updated`, `lsp.client.diagnostics`, `tui.toast.show` y más.

### Entradas con slash mantenidas

| Comando | Descripción |
|---------|-------------|
| `/plan` | Crear un plan de implementación |
| `/code-review` | Revisar cambios de código |
| `/build-fix` | Corregir errores de compilación |
| `/refactor-clean` | Eliminar código muerto |
| `/learn` | Extraer patrones de la sesión |
| `/checkpoint` | Guardar estado de verificación |
| `/quality-gate` | Ejecutar la compuerta de verificación mantenida |
| `/update-docs` | Actualizar documentación |
| `/update-codemaps` | Actualizar codemaps |
| `/test-coverage` | Analizar cobertura |
| `/go-review` | Revisión de código Go |
| `/go-test` | Flujo TDD de Go |
| `/go-build` | Corregir errores de compilación Go |
| `/python-review` | Revisión de código Python (PEP 8, type hints, seguridad) |
| `/multi-plan` | Planificación colaborativa multimodelo |
| `/multi-execute` | Ejecución colaborativa multimodelo |
| `/multi-backend` | Flujo multimodelo centrado en backend |
| `/multi-frontend` | Flujo multimodelo centrado en frontend |
| `/multi-workflow` | Flujo completo de desarrollo multimodelo |
| `/pm2` | Generar automáticamente comandos de servicio PM2 |
| `/sessions` | Gestionar historial de sesiones |
| `/skill-create` | Generar skills desde git |
| `/instinct-status` | Ver instincts aprendidos |
| `/instinct-import` | Importar instincts |
| `/instinct-export` | Exportar instincts |
| `/evolve` | Agrupar instincts en skills |
| `/promote` | Promover instincts del proyecto a alcance global |
| `/projects` | Listar proyectos conocidos y estadísticas de instincts |
| `/prune` | Eliminar instincts pendientes expirados (TTL de 30d) |
| `/learn-eval` | Extraer y evaluar patrones antes de guardarlos |
| `/setup-pm` | Configurar gestor de paquetes |
| `/harness-audit` | Auditar fiabilidad del harness, preparación para evals y postura de riesgo |
| `/loop-start` | Iniciar un patrón controlado de ejecución de bucle de agentes |
| `/loop-status` | Inspeccionar estado y checkpoints de bucles activos |
| `/quality-gate` | Ejecutar comprobaciones de calidad para rutas o para todo el repo |
| `/model-route` | Enrutar tareas a modelos según complejidad y presupuesto |

### Instalación del plugin

**Opción 1: usar directamente**
```bash
cd ECC
opencode
```

**Opción 2: instalar como paquete npm**
```bash
npm install ecc-universal
```

Después añade esto a tu `opencode.json`:
```json
{
  "plugin": ["ecc-universal"]
}
```

Esa entrada de plugin npm habilita el módulo publicado del plugin de OpenCode para ECC (hooks/eventos y herramientas del plugin).
**No** añade automáticamente el catálogo completo de comandos/agents/instrucciones de ECC a la configuración de tu proyecto.

Para la configuración completa de ECC en OpenCode, puedes:
- ejecutar OpenCode dentro de este repositorio, o
- copiar los recursos de configuración incluidos en `.opencode/` dentro de tu proyecto y conectar las entradas `instructions`, `agent` y `command` en `opencode.json`

### Documentación

- **Migration Guide**: `.opencode/MIGRATION.md`
- **OpenCode Plugin README**: `.opencode/README.md`
- **Consolidated Rules**: `.opencode/instructions/INSTRUCTIONS.md`
- **LLM Documentation**: `llms.txt` (complete OpenCode docs for LLMs)

---

## Soporte para GitHub Copilot

ECC ofrece **soporte para GitHub Copilot** en VS Code mediante el sistema nativo de instrucciones y archivos de prompt de Copilot Chat, sin herramientas adicionales.

### Qué incluye

| Componente | Archivo | Propósito |
|-----------|---------|-----------|
| Instrucciones base | `.github/copilot-instructions.md` | Rules siempre cargadas: estilo de código, seguridad, tests y flujo de git |
| Configuración de VS Code | `.vscode/settings.json` | Archivos de instrucciones por tarea para generación de código, tests, review y mensajes de commit |
| Prompt de planificación | `.github/prompts/plan.prompt.md` | Planificación de implementación por fases |
| Prompt de TDD | `.github/prompts/tdd.prompt.md` | Ciclo Red-Green-Improve |
| Prompt de code review | `.github/prompts/code-review.prompt.md` | Revisión de calidad y seguridad |
| Prompt de revisión de seguridad | `.github/prompts/security-review.prompt.md` | Análisis profundo de seguridad alineado con OWASP |
| Prompt de corrección de build | `.github/prompts/build-fix.prompt.md` | Resolución sistemática de errores de build y CI |
| Prompt de refactor | `.github/prompts/refactor.prompt.md` | Limpieza de código muerto y simplificación |

### Inicio rápido (GitHub Copilot)

Los archivos ya están en su sitio. Abre cualquier repo que contenga este proyecto y GitHub Copilot Chat detectará automáticamente `.github/copilot-instructions.md`.
El `.vscode/settings.json` versionado habilita `chat.promptFiles` para que VS Code pueda cargar los prompts reutilizables desde `.github/prompts/`.

Para usar los prompts de workflow en Copilot Chat:
1. Abre el panel de Copilot Chat en VS Code.
2. Haz clic en el icono de **clip / adjuntar** y selecciona **Prompt...**, o escribe `/` y elige un prompt.
3. Selecciona el prompt (por ejemplo `plan`, `tdd`, `code-review`).

### Cómo funciona

GitHub Copilot en VS Code lee automáticamente dos tipos de archivos:

- **`.github/copilot-instructions.md`** — instrucciones a nivel de repositorio, siempre inyectadas en cada solicitud de Copilot Chat. Contiene los estándares base de código de ECC, checklist de seguridad, requisitos de testing y flujo de git.
- **`.github/prompts/*.prompt.md`** — archivos de prompt reutilizables que los usuarios invocan bajo demanda. Cada prompt guía a Copilot a través de un flujo específico de ECC (plan → TDD → review → ship).

El archivo **`.vscode/settings.json`** añade capas de instrucciones por tarea para que Copilot reciba el contexto adecuado según si estás generando código, escribiendo tests, revisando una selección o redactando un mensaje de commit.

### Cobertura de funciones

| Función de ECC | Equivalente en Copilot |
|----------------|------------------------|
| Estándares de código | Siempre activo vía `copilot-instructions.md` |
| Checklist de seguridad | Siempre activo + prompt `security-review` |
| Testing / TDD | Siempre activo + prompt `tdd` |
| Planificación de implementación | Prompt `plan` |
| Code review | Prompt `code-review` |
| Resolución de errores de build | Prompt `build-fix` |
| Refactorización | Prompt `refactor` |
| Formato de mensaje de commit | Instrucción por tarea en `settings.json` |
| Hooks / automatización | No soportado (Copilot no tiene sistema de hooks) |
| Agents / delegación | No soportado (Copilot no tiene API de subagentes) |

### Limitaciones

GitHub Copilot no tiene un sistema de hooks ni una API de subagentes, así que las automatizaciones de hooks de ECC (autoformato, chequeo de TypeScript, persistencia de sesión, protección de dev servers) y la delegación a agents no están disponibles. Aun así, la capa de instrucciones y prompts lleva toda la filosofía de desarrollo de ECC (estándares, seguridad, TDD y workflow) a cada sesión de Copilot Chat.

---

## Paridad de funciones entre herramientas

ECC es el **primer plugin en exprimir al máximo las principales herramientas de programación con IA**. Así se compara cada harness:

| Función | Claude Code | Cursor IDE | Codex CLI | OpenCode | GitHub Copilot |
|---------|-------------|------------|-----------|----------|----------------|
| **Agents** | 61 | Compartidos (`AGENTS.md`) | Compartidos (`AGENTS.md`) | 12 | N/A |
| **Commands** | 76 | Compartidos | Basados en instrucciones | 35 | 6 prompts |
| **Skills** | 246 | Compartidas | 10 (formato nativo) | 37 | Vía instrucciones |
| **Eventos de hooks** | 8 tipos | 15 tipos | Ninguno todavía | 11 tipos | Ninguno |
| **Scripts de hooks** | 20+ scripts | 16 scripts (adaptador DRY) | N/A | Hooks de plugin | N/A |
| **Rules** | 34 (common + lenguaje) | 34 (frontmatter YAML) | Basadas en instrucciones | 13 instrucciones | 1 archivo siempre activo |
| **Custom Tools** | Vía hooks | Vía hooks | N/A | 6 herramientas nativas | N/A |
| **Servidores MCP** | 14 | Compartidos (`mcp.json`) | 7 (fusionados automáticamente vía parser TOML) | Completo | N/A |
| **Formato de configuración** | `settings.json` | `hooks.json` + `rules/` | `config.toml` | `opencode.json` | `copilot-instructions.md` + `settings.json` |
| **Archivo de contexto** | `CLAUDE.md` + `AGENTS.md` | `AGENTS.md` | `AGENTS.md` | `AGENTS.md` | `copilot-instructions.md` |
| **Secret Detection** | Hook-based | beforeSubmitPrompt hook | Sandbox-based | Hook-based | Instruction-based |
| **Auto-Format** | PostToolUse hook | afterFileEdit hook | N/A | file.edited hook | N/A |
| **Version** | Plugin | Plugin | Reference config | 2.0.0-rc.1 | Instruction layer |

**Decisiones arquitectónicas clave:**
- El **`AGENTS.md`** de la raíz es el archivo universal entre herramientas (lo leen Claude Code, Cursor, Codex y OpenCode; GitHub Copilot usa `.github/copilot-instructions.md` en su lugar)
- El **patrón de adaptador DRY** permite que Cursor reutilice los scripts de hooks de Claude Code sin duplicación
- El **formato de skills** (`SKILL.md` con frontmatter YAML) funciona en Claude Code, Codex y OpenCode
- La ausencia de hooks en Codex se compensa con `AGENTS.md`, overrides opcionales mediante `model_instructions_file` y permisos de sandbox

---

## Contexto

He estado usando Claude Code desde su despliegue experimental. Gané el hackathon de Anthropic x Forum Ventures en sep. de 2025 con [@DRodriguezFX](https://x.com/DRodriguezFX): construimos [zenith.chat](https://zenith.chat) íntegramente usando Claude Code.

Estas configuraciones están probadas en batalla en múltiples aplicaciones en producción.

---

## Optimización de tokens

Usar Claude Code puede salir caro si no gestionas el consumo de tokens. Estas configuraciones reducen significativamente los costes sin sacrificar calidad.

### Configuración recomendada

Añade esto a `~/.claude/settings.json`:

```json
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "50"
  }
}
```

| Ajuste | Valor por defecto | Recomendado | Impacto |
|--------|-------------------|-------------|---------|
| `model` | opus | **sonnet** | ~60% de reducción de coste; cubre el 80%+ de las tareas de programación |
| `MAX_THINKING_TOKENS` | 31,999 | **10,000** | ~70% de reducción del coste oculto de razonamiento por solicitud |
| `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` | 95 | **50** | Compacta antes; mejor calidad en sesiones largas |
| `ECC_CONTEXT_MONITOR_COST_WARNINGS` | on | **off para usuarios con suscripción** | Suprime advertencias de estimación de coste visibles para el agente, manteniendo advertencias de contexto/alcance/bucles |

Cambia a Opus solo cuando necesites razonamiento arquitectónico profundo:
```
/model opus
```

### Comandos de trabajo diario

| Comando | Cuándo usarlo |
|---------|----------------|
| `/model sonnet` | Valor por defecto para la mayoría de tareas |
| `/model opus` | Arquitectura compleja, depuración, razonamiento profundo |
| `/clear` | Entre tareas no relacionadas (gratis, reinicio instantáneo) |
| `/compact` | En puntos lógicos de corte (investigación terminada, hito completado) |
| `/cost` | Monitorizar gasto de tokens durante la sesión |

Si usas una suscripción de Claude y las estimaciones de coste por tasa de API del monitor de contexto no te resultan útiles, define `ECC_CONTEXT_MONITOR_COST_WARNINGS=off`. Esto solo suprime las advertencias de coste visibles para el agente; no desactiva las advertencias por agotamiento de contexto, alcance o bucles.

### Compactación estratégica

La skill `strategic-compact` (incluida en este plugin) sugiere `/compact` en puntos lógicos de corte en lugar de depender de la autocompactación al 95% de contexto. Consulta `skills/strategic-compact/SKILL.md` para la guía completa de decisión.

**Cuándo compactar:**
- Después de la investigación/exploración, antes de implementar
- Después de completar un hito, antes de empezar el siguiente
- Después de depurar, antes de seguir con el trabajo de funcionalidad
- Después de un enfoque fallido, antes de probar uno nuevo

**Cuándo NO compactar:**
- A mitad de la implementación (perderás nombres de variables, rutas de archivos y estado parcial)

### Gestión de la ventana de contexto

**Crítico:** no habilites todos los MCPs a la vez. Cada descripción de herramienta MCP consume tokens de tu ventana de 200k, reduciéndola potencialmente hasta ~70k.

- Mantén menos de 10 MCPs habilitados por proyecto
- Mantén menos de 80 herramientas activas
- Usa `/mcp` para desactivar servidores MCP de Claude Code que no uses; esas decisiones de runtime se guardan en `~/.claude.json`
- Usa `ECC_DISABLED_MCPS` solo para filtrar configuraciones MCP generadas por ECC durante flujos de instalación/sincronización

### Advertencia de coste para Agent Teams

Agent Teams crea múltiples ventanas de contexto. Cada teammate consume tokens de forma independiente. Úsalo solo en tareas donde el paralelismo aporte un valor claro (trabajo multi-módulo, revisiones en paralelo). Para tareas secuenciales simples, los subagentes son más eficientes en tokens.

---

## ADVERTENCIA: notas importantes

### Optimización de tokens

¿Llegaste a tus límites diarios? Consulta la **[guía de Optimización de tokens](reference/token-optimization.md)** para ver configuraciones recomendadas y consejos de workflow.

Victorias rápidas:

```json
// ~/.claude/settings.json
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "50",
    "CLAUDE_CODE_SUBAGENT_MODEL": "haiku"
  }
}
```

Usa `/clear` entre tareas no relacionadas, `/compact` en puntos lógicos de corte y `/cost` para vigilar el gasto.

### Personalización

Estas configuraciones funcionan para mi workflow. Tú deberías:
1. Empezar por lo que te encaje.
2. Adaptarlo a tu stack.
3. Eliminar lo que no uses.
4. Añadir tus propios patrones.

---

## Proyectos de la comunidad

Proyectos construidos sobre ECC o inspirados por él:

| Proyecto | Descripción |
|----------|-------------|
| [EVC](https://github.com/SaigonXIII/evc) | Workspace de agents de marketing: 42 comandos para operadores de contenido, gobernanza de marca y publicación multicanal. [Resumen visual](https://saigonxiii.github.io/evc). |
| [trading-skills](https://github.com/VictorVVedtion/trading-skills) | 68 skills temáticas de trading para Claude Code con prompts de revisión previa a la operación y compuertas de riesgo inspiradas en operadores de mercado. |

¿Construiste algo con ECC? Abre un PR para añadirlo aquí.

---

## Sponsors

Este proyecto es gratuito y open source. Los patrocinadores ayudan a mantenerlo y hacerlo crecer.

[**Hazte patrocinador**](https://github.com/sponsors/affaan-m) | [Niveles de patrocinio](../../SPONSORS.md) | [Programa de patrocinio](../../SPONSORING.md)

---

## Historial de estrellas

[![Star History Chart](https://api.star-history.com/svg?repos=affaan-m/ECC&type=Date)](https://star-history.com/#affaan-m/ECC&Date)

---

## Links

- **Shorthand Guide (empieza aquí):** [The Shorthand Guide to Everything Claude Code](https://x.com/affaanmustafa/status/2012378465664745795)
- **Longform Guide (avanzada):** [The Longform Guide to Everything Claude Code](https://x.com/affaanmustafa/status/2014040193557471352)
- **Guía de seguridad:** [Security Guide](../../the-security-guide.md) | [Hilo](https://x.com/affaanmustafa/status/2033263813387223421)
- **Sigue a:** [@affaanmustafa](https://x.com/affaanmustafa)

---

## Licencia

MIT: úsalo libremente, modifícalo según necesites y contribuye de vuelta si puedes.

---

**Dale una estrella a este repo si te ayuda. Lee ambas guías. Construye algo genial.**
