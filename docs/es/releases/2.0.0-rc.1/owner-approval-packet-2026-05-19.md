# Paquete de aprobación del propietario de ECC v2.0.0-rc.1

Fecha de instantánea: 2026-05-19.

Este paquete es la hoja final de decisión humana para el lanzamiento público de rc.1. No publica nada por sí mismo. Úsalo para aprobar, diferir o bloquear cada acción de lanzamiento después de volver a ejecutar los comandos finales de evidencia desde el commit de lanzamiento previsto.

Commit de origen para la línea base limpia de evidencia que extiende este paquete:
`9819626459a662773be7d0b1c18d82c1316b8c36`.

## Evidencia actual

| Evidencia | Estado registrado actual | Repetir antes de aprobar |
| --- | --- | --- |
| Auditoría de la plataforma | listo true, 0 PR abiertos, 0 issues abiertas, 0 brechas de discusión, 0 archivos sucios | sí |
| Smoke del paquete de vista previa | listo true, digest `531328aaaa53`, 5/5 comprobaciones | sí |
| Puerta de aprobación del lanzamiento | listo false, digest `ef8f49f727b7`, 4/6 comprobaciones pasan; decisiones del propietario y lecturas de URL en vivo pendientes | sí |
| Suite de video | listo true, 15/15 activos fuente, 13/13 artefactos de la suite, 12/12 candidatos de publicación | sí |
| Pruebas de la superficie de lanzamiento | 27/27 aprobadas después de agregar este paquete | sí |
| Suite local completa | 2568/2568 aprobadas antes de fusionar la PR #2013; la regresión enfocada de GateGuard pasó 91/91 de nuevo antes de fusionar la PR #2011 | sí |
| CI de GitHub | PR #1998, PR #1999, PR #2000, PR #2001, PR #2002, PR #2004, PR #2008, post-PR #2006 `main`, PR #2009, post-PR #2009 `main`, post-PR #2011 `main` y post-PR #2013 `main` ya se fusionaron o avanzaron después de checks obligatorios en verde | verificar HEAD actual |

## Registro de decisiones

| Decisión | Aprobar / diferir / bloquear | Evidencia requerida primero | Notas |
| --- | --- | --- | --- |
| Prelanzamiento de GitHub | diferir | rama limpia final, ledger de URL, notas de lanzamiento, video adjunto o enlace al video | Aprobar solo después de que las notas finales de lanzamiento contengan URLs en vivo de paquete/plugin/video o URLs explícitamente marcadas como bloqueadas. |
| Publicación `next` de npm | diferir | `npm pack --dry-run`, `npm publish --tag next --dry-run`, plan de lectura del dist-tag del registro | Mantener `ecc-universal@2.0.0-rc.1` en `next`; no mover `latest` durante rc.1. |
| Etiqueta del plugin de Claude | diferir | `claude plugin validate .claude-plugin/plugin.json`, `claude plugin tag .claude-plugin --dry-run` | Crear y empujar la etiqueta real solo después de la aprobación del lanzamiento. |
| Marketplace del repositorio de Codex | diferir | smoke de agregado al marketplace en temp-home y estado actual oficial de Plugin Directory | Reivindicar solo distribución repo-marketplace; no afirmar listado oficial en Plugin Directory sin evidencia de listado. |
| Lenguaje de facturación de ECC Tools | diferir | lectura de disponibilidad en vivo para la cuenta objetivo y el estado de facturación/producto | No anunciar pagos nativos ni Pro administrado por Marketplace hasta que la puerta esté en vivo. |
| Carga de video | diferir | el propietario selecciona el corte principal de lanzamiento más clips cortos, la autoevaluación permanece limpia | Cargar solo los cortes aprobados; conservar preservada la salida editable de timeline/proyecto. |
| X, LinkedIn, GitHub Discussion, formato largo | diferir | actualizaciones en vivo del ledger de URLs de lanzamiento, npm, plugin, video y facturación | Las publicaciones desde cuentas personales y el texto saliente requieren aprobación explícita. |
| Alcance a sponsor, partner, consulting, conferencia y podcast | diferir | URLs públicas finales más texto saliente aprobado por el propietario | No enviar borradores hasta que el propietario apruebe el lote exacto. |

## Relleno final de URLs

Actualiza estas superficies después de que terminen las acciones de publicación aprobadas:

| Superficie | Fuente del valor final | Objetivos de actualización |
| --- | --- | --- |
| URL de prelanzamiento de GitHub | `gh release view v2.0.0-rc.1 --repo affaan-m/ECC --json url` | notas de lanzamiento, ledger de URLs, copia social |
| URL del paquete rc de npm | `npm view ecc-universal@2.0.0-rc.1 version dist-tags --json` | ledger de URLs, quickstart, notas de lanzamiento |
| URL de la etiqueta del plugin de Claude | etiqueta `ecc--v2.0.0-rc.1` enviada o lectura de marketplace | ledger de URLs, docs del plugin, notas de lanzamiento |
| Evidencia de repo-marketplace de Codex | lectura de `codex plugin marketplace add <local-checkout>` en temp-home | ledger de URLs, preparación de publicación |
| URL principal del video de lanzamiento | video principal de lanzamiento aprobado por el propietario y cargado | release de GitHub, X, LinkedIn, formato largo |
| URLs de clips cortos | clips aprobados y cargados | hilo de X, LinkedIn, pack para partner/sponsor/charla |
| URL de facturación/disponibilidad de ECC Tools | lectura de disponibilidad en vivo o estado bloqueado explícito | texto para sponsor, texto para Pro, notas de lanzamiento |

## Comandos finales de evidencia

Ejecuta esto desde el commit exacto de lanzamiento antes de aprobar la publicación:

```bash
git status --short --branch
node scripts/platform-audit.js --json
npm run preview-pack:smoke -- --format json
npm run release:approval-gate -- --format json
npm run release:video-suite -- --format json
npm run harness:adapters -- --check
npm run harness:audit -- --format json
npm run observability:ready
npm run security:ioc-scan
npm audit --audit-level=moderate
npm audit signatures
node tests/docs/ecc2-release-surface.test.js
node tests/hooks/gateguard-fact-force.test.js
node tests/run-all.js
cd ecc2 && cargo test
```

## Texto de aprobación

Usa aprobaciones cortas y explícitas. Ejemplo:

```text
Aprobado para el prelanzamiento de GitHub rc.1, la publicación next de npm, la
etiqueta del plugin de Claude y el anuncio del lanzamiento después de que los
comandos finales de evidencia pasen desde el commit <sha>.
Las cargas de video están aprobadas para <primary-video> y <shorts-list>.
Los mensajes salientes para sponsor, partner, consulting, conferencia y podcast
siguen bloqueados hasta que apruebe el lote exacto.
```

## No aprobar si

- La rama final está sucia o ya no coincide con el commit de lanzamiento previsto.
- Cualquier comando de evidencia requerido falla o se omite sin una postergación escrita.
- La copia de lanzamiento afirma facturación en vivo, propagación en marketplace del plugin, `next` de npm o listado oficial en Codex Plugin Directory antes de que exista la lectura de retorno.
- La copia del anuncio contiene URLs obsoletas, rutas privadas o decisiones sin resolver sobre enlaces en vivo.
- El corte de video seleccionado tiene fotogramas negros, audio faltante, URLs obsoletas, prueba débil del producto o subtítulos sin revisar.
- El lote saliente no ha sido revisado exactamente como se enviará.

Este paquete por sí solo no autoriza ningún correo saliente, publicación en cuenta personal, publicación de paquete, etiqueta de plugin ni anuncio de facturación.
