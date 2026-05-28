# Limpieza de la cola de todo el propietario - 2026-05-18

Esta nota registra la limpieza en vivo de la cola de GitHub fuera de los cinco repositorios de publicación de ECC rastreados por `scripts/platform-audit.js`.

## Comandos

```bash
gh search prs --owner affaan-m --state open --json repository,number,title,url,author,updatedAt --limit 100
gh search issues --owner affaan-m --state open --json repository,number,title,url,updatedAt --limit 100
```

## Resultado

- PR abiertos en todo el propietario después de la limpieza: 0.
- Issues abiertos en todo el propietario después de la limpieza: 0.
- PR obsoletos de dependency-bot cerrados: 24.
- Issues obsoletos de roadmap legacy payments/0EM cerrados: 72.
- PR finales obsoletos/generados/manual-review cerrados: 9.
- Issues finales legacy/outreach/placeholder cerrados: 5.
- Repositorios archivados desarchivados temporalmente para cerrar PR obsoletos de dependencias y
  restaurados a estado archivado:
  `affaan-m/stoictradingAI`, `affaan-m/dprc-autotrader-v2`,
  `affaan-m/polycule-secure` y `affaan-m/pragmAItism_defAInce`.
- El barrido final de repositorios archivados desarchivó temporalmente y restauró
  `affaan-m/dprc-autotrader-v2` y `affaan-m/stoictradingAI`.

## Disposición final de PR

- `affaan-m/dprc-autotrader-v2#5`: cerró un bundle ECC generado obsoleto con
  checks fallidos y base de actualización de dependencias.
- `affaan-m/x-algorithm-score#2`: cerró un PR externo obsoleto/en conflicto de feature con
  directorios locales accidentales de herramientas de IA anotados en el cuerpo del PR.
- `affaan-m/dexploy#28`: cerró un PR obsoleto de skill ECC generado con los
  cambios solicitados.
- `affaan-m/zenith#5`: cerró un PR obsoleto de skill ECC generado.
- `affaan-m/zenith#4`: cerró un PR de prueba/ruido cuyo diff solo agregó un
  comentario de script no accionable.
- `affaan-m/affaan-m#1`: cerró un PR obsoleto/en conflicto de tarjeta README de un tercero.
- `affaan-m/affaanmustafa.com#1`: cerró un PR obsoleto de nombre de Cloudflare Worker con
  cambios solicitados.
- `affaan-m/0em-payments-dashboard#11`: cerró un PR obsoleto/en conflicto de nombre de
  Cloudflare Worker.
- `affaan-m/0em-payments-dashboard#3`: cerró un PR obsoleto/en conflicto de nombre de
  Cloudflare Worker.

## Disposición final de issues

- `affaan-m/dprc-autotrader-v2#3`: cerró la propuesta de integración pública como no planificada
  para el repositorio archivado.
- `affaan-m/stoictradingAI#20`: cerró la pregunta pública de outreach como no planificada
  para el repositorio archivado.
- `affaan-m/dexploy#27`: cerró un issue de prueba obsoleto del creador de skills internos.
- `affaan-m/dexploy#25`: preservó hallazgos útiles de deployment/localStorage y
  Cloudflare en Linear `ITO-62`, luego cerró el issue obsoleto de GitHub.
- `affaan-m/telegram-mcp-ts#1`: cerró un issue de marcador de posición vacío obsoleto.

## Disposición

Los PR obsoletos de dependencias cerrados eran actualizaciones de versiones generadas y deberían
regenerarse desde las bases actuales si todavía se necesitan. Los PR cerrados de bundles ECC generados
deberían regenerarse desde el flujo actual de ECC Tools si esos repositorios vuelven a estar activos.
Los issues cerrados de legacy payments/0EM eran elementos antiguos de planificación, reemplazados por
las rutas nativas de ECC Tools para native-payments, hosted analysis,
billing-readback y roadmap de Linear/proyecto.
