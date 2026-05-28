# Copy de lanzamiento social (X + LinkedIn)

Usa estas plantillas como puntos de partida listos para lanzamiento. Revisa el
tono del canal antes de publicar.

## Post de X: anuncio de release

```text
ECC v2.0.0-rc.1 preview pack ya está listo para la revisión final del release.

ECC 2.0 es el sistema operativo de operador nativo del harness para trabajo
agentic: skills, hooks, rules, convenciones MCP, gates de release y una shell de
operador Hermes opcional.

Qué incluye:
- guía de setup de Hermes
- notas de release y material de lanzamiento
- docs de arquitectura cross-harness
- guía de importación de Hermes para convertir workflows locales de operador en skills públicos de ECC

Empieza aquí: https://github.com/affaan-m/ECC
Notas de release: https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/release-notes.md
```

## Post de X: prueba + métricas

```text
ECC v2.0.0-rc.1 mantiene honesta la superficie pública:
- substrate ECC reusable en el repo
- Hermes documentado como la shell de operador
- estado privado del workspace fuera
- metadatos y docs de release cubiertos por tests

Esta es la línea de release-candidate: forma del sistema pública ahora, integraciones locales más profundas solo después de sanitizar.
```

## Quote tweet de X: artículo sobre eval skills

```text
Muy buen punto sobre la disciplina de evals.

En ECC convertimos esto en checks de producción mediante:
- /harness-audit
- /quality-gate
- resúmenes de sesión en fase Stop

En v2.0.0-rc.1, esa disciplina se extiende a la superficie de release: docs, manifiestos, copy de lanzamiento y fronteras público/privado están respaldados por tests.
```

## Quote tweet de X: workflow plankton / deslop

```text
Esta dirección de workflow es la correcta: optimizar el harness, no solo los prompts.

ECC v2.0.0-rc.1 lleva eso más lejos: skills reutilizables, adapters delgados de harness y Hermes como shell de operador encima.
```

## Publicación de LinkedIn: resumen amigable para partners

```text
ECC v2.0.0-rc.1 preview pack ya está listo para la revisión final del release.

ECC 2.0 es el sistema operativo de operador nativo del harness para trabajo agentic. La misma capa reusable ahora llega a los workflows de Claude Code, Codex, OpenCode, Cursor, Gemini, Zed, GitHub Copilot y a las líneas de operador solo terminal.

Esta superficie de release-candidate incluye:
- documentación saneada de setup de Hermes
- notas de release y material de lanzamiento
- notas de arquitectura cross-harness
- guía de importación de Hermes para convertir patrones locales de operador en skills públicos de ECC

No incluye estado privado del workspace, credenciales, exports locales en bruto ni datasets personales.

Repo: https://github.com/affaan-m/ECC
Notas de release: https://github.com/affaan-m/ECC/blob/main/docs/releases/2.0.0-rc.1/release-notes.md
```
