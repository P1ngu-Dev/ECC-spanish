# Playbook de métricas y sponsorship

Este archivo es un guion práctico para llamadas con sponsors y revisiones de
partners del ecosistema.

## Qué rastrear

Usa cuatro categorías en cada update:

1. **Distribución** — paquetes npm e instalaciones de GitHub App
2. **Adopción** — stars, forks, contributors, cadencia de releases
3. **Superficie del producto** — commands/skills/agents y soporte cross-platform
4. **Confiabilidad** — conteos de tests aprobados y tiempo de resolución de bugs en producción

## Obtener métricas en vivo

### Descargas de npm

```bash
# Descargas semanales
curl -s https://api.npmjs.org/downloads/point/last-week/ecc-universal
curl -s https://api.npmjs.org/downloads/point/last-week/ecc-agentshield

# Últimos 30 días
curl -s https://api.npmjs.org/downloads/point/last-month/ecc-universal
curl -s https://api.npmjs.org/downloads/point/last-month/ecc-agentshield
```

### Adopción del repositorio GitHub

```bash
gh api repos/affaan-m/ECC \
  --jq '{stars:.stargazers_count,forks:.forks_count,contributors_url:.contributors_url,open_issues:.open_issues_count}'
```

### Tráfico de GitHub (requiere acceso de maintainer)

```bash
gh api repos/affaan-m/ECC/traffic/views
gh api repos/affaan-m/ECC/traffic/clones
```

### Instalaciones de GitHub App

El conteo de instalaciones de GitHub App es actualmente más fiable en el
Marketplace/dashboard de App.
Usa el valor más reciente de:

- [ECC Tools Marketplace](https://github.com/marketplace/ecc-tools)

## Lo que todavía no puede medirse públicamente

- Los conteos de instalación/descarga del plugin de Claude no se exponen
  actualmente mediante una API pública.
- Para conversaciones con partners, usa métricas npm + instalaciones de GitHub
  App + tráfico del repo como paquete proxy.

## Packaging sugerido para sponsors

Usa estos como puntos de partida en la negociación:

- **Pilot Partner:** `$200/month`
  - Ideal para la primera validación de partnership y actualizaciones mensuales
    simples para sponsors.
- **Growth Partner:** `$500/month`
  - Incluye check-ins de roadmap y bucle de feedback de implementación.
- **Strategic Partner:** `$1,000+/month`
  - Colaboración multicanal, soporte de lanzamiento y alineación operativa más
    profunda.

## Guion de 60 segundos

Úsalo en llamadas:

> ECC ahora está posicionado como un sistema de rendimiento para agent harness,
> no como un repo de configuración.
> Medimos la adopción mediante distribución npm, instalaciones de GitHub App y
> crecimiento del repositorio.
> Las instalaciones del plugin de Claude están estructuralmente infra-contadas
> de forma pública, así que usamos un modelo de métricas combinado.
> El proyecto soporta Claude Code, Cursor, OpenCode y Codex app/CLI con
> confiabilidad de hooks de grado producción y una gran suite de tests que pasa.

Para snippets de copy social listos para lanzamiento, consulta
[`social-launch-copy.md`](./social-launch-copy.md).
