---
name: homelab-network-readiness
description: Readiness checklist for homelab VLAN segmentation, local DNS filtering, and WireGuard-style remote access before changing router, firewall, DHCP, or VPN configuration.
origin: community
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Preparación de red para homelab

Usa este skill antes de cambiar una red doméstica o de pequeño laboratorio que mezcle VLAN, Pi-hole u otro resolvedor DNS local, reglas de firewall y acceso remoto por VPN.

Este es un skill de planificación y revisión. No lo conviertas en configuración lista para copiar y pegar de router, firewall o VPN a menos que la plataforma objetivo, la topología actual, la vía de rollback, el acceso a consola y la ventana de mantenimiento estén todos confirmados.

## Cuándo usarlo

- Prepararte para dividir una red plana en VLAN de confianza, IoT, invitados, servidores o administración.
- Mover clientes DHCP a Pi-hole, AdGuard Home, Unbound u otro resolvedor DNS local.
- Añadir WireGuard, Tailscale, ZeroTier, OpenVPN o acceso VPN nativo del router.
- Revisar si un cambio en el homelab puede dejar sin acceso al gateway, switch, punto de acceso, servidor DNS o servidor VPN.
- Convertir una idea informal de red doméstica en un plan de migración por etapas con evidencia de validación.

## Reglas de seguridad

- Mantén la primera respuesta solo de lectura: inventario, riesgos, plan por etapas, validación y rollback.
- No expongas paneles de administración del gateway, resolutores DNS, SSH, consolas NAS ni UIs de gestión de VPN directamente a Internet.
- No proporciones comandos de firewall, NAT, VLAN, DHCP o VPN sin una plataforma confirmada y un procedimiento de rollback.
- Exige acceso a consola fuera de banda o en la misma sala antes de cambiar VLAN de administración, puertos trunk, políticas por defecto del firewall o ajustes DHCP/DNS.
- Conserva una forma de volver a Internet antes de apuntar toda la red a un nuevo resolutor DNS o ruta VPN.
- Trata las redes IoT, invitado, cámaras y servidores de laboratorio como zonas de confianza distintas hasta que el operador indique lo contrario.

## Inventario requerido

Recoge esto antes de dar pasos de implementación:

| Área | Preguntas |
| --- | --- |
| Borde de Internet | ¿Qué es el módem o ONT? ¿El router del ISP está en puente o sigue ruteando? |
| Gateway | ¿Qué enruta, filtra, maneja DHCP y termina VPN? |
| Conmutación | ¿Qué puertos del switch son uplinks, access, trunk o no administrados? |
| Wi-Fi | ¿Qué SSID corresponden a qué redes y los AP están por cable o en malla? |
| Direccionamiento | ¿Qué subredes existen hoy y cuáles chocan con sitios VPN? |
| DNS/DHCP | ¿Qué servicio entrega leases y direcciones de resolutor ahora? |
| Gestión | ¿Cómo alcanzará el operador el gateway, switch y AP después de los cambios? |
| Recuperación | ¿Qué se puede revertir localmente si fallan DNS, DHCP, VLAN o rutas VPN? |

## Plan de zonas de confianza y VLAN

Empieza por la intención, no por la sintaxis del proveedor.

| Zona | Contenido típico | Política por defecto |
| --- | --- | --- |
| Trusted | Portátiles, teléfonos, estaciones de admin | Puede llegar a servicios compartidos y administración solo cuando hace falta |
| Servers | NAS, Home Assistant, hosts de laboratorio, resolutor DNS | Acepta flujos entrantes limitados desde clientes de confianza |
| IoT | TVs, enchufes inteligentes, cámaras, altavoces | Solo acceso a Internet y excepciones explícitas |
| Guest | Dispositivos de visitantes | Solo Internet, sin acceso a la LAN |
| Management | Gateway, switches, APs, controladores | Accesible solo desde dispositivos de admin confiables |
| VPN | Clientes remotos | Igual o menos acceso que los clientes de confianza |

Antes de recomendar IDs de VLAN o subredes, confirma:

1. Que el gateway soporta enrutamiento inter-VLAN y reglas de firewall.
2. Que el switch soporta el comportamiento requerido de puertos tagged y untagged.
3. Que los AP pueden asignar SSID a VLAN.
4. Que el operador sabe por qué puerto está conectado durante el cambio.
5. Que la red de administración sigue accesible después de cambios en trunk y SSID.

## Preparación para filtrado DNS

Pi-hole u otro resolvedor local debe introducirse como dependencia, no como un único punto de fallo.

1. Dale una dirección reservada antes de usarlo en opciones DHCP.
2. Confirma que puede resolver DNS público y nombres locales `home.arpa`.
3. Mantén el gateway o un segundo resolutor disponible como respaldo temporal.
4. Prueba un cliente o una VLAN antes de cambiar todos los scopes DHCP.
5. Documenta qué redes pueden omitir el filtrado y por qué.
6. Comprueba que las reglas de bloqueo no rompen portales cautivos, VPN de trabajo, actualizaciones de firmware o dispositivos médicos/de seguridad.

Evidencia útil de validación:

```text
El cliente obtiene el lease DHCP esperado
El cliente recibe el resolutor DNS esperado
La resolución DNS pública funciona
La resolución local home.arpa funciona
El dominio de prueba bloqueado solo se bloquea donde corresponde
Las interfaces de administración del gateway y DNS no son accesibles desde las redes guest o IoT
```

## Preparación para acceso remoto

Para acceso estilo WireGuard, decide a qué puede llegar la VPN antes de generar claves o abrir puertos.

| Modo | Cuándo usarlo | Notas de riesgo |
| --- | --- | --- |
| Split tunnel a una subred | Administración remota de NAS o hosts de laboratorio | Mantén la lista de rutas estrecha |
| Split tunnel a servicios de confianza | Acceso a apps seleccionadas por IP o DNS | Requiere reglas de firewall precisas |
| Full tunnel | Redes no confiables o viajes | Más ancho de banda y responsabilidad sobre DNS |
| Overlay VPN | Acceso remoto más simple con controles de identidad | Aun así requiere revisión de ACL |

No recomiendes port forwarding hasta que el operador confirme:

- Que el endpoint VPN está parcheado y mantenido activamente.
- Que el puerto reenviado apunta solo al servicio VPN, no a una UI de administración.
- Que se entiende el comportamiento de Dynamic DNS, IP pública y CGNAT del ISP.
- Que las claves de peers pueden revocarse sin reconstruir toda la red.
- Que los logs o el estado de conexión pueden verificar quién se conectó y cuándo.

## Secuencia de cambio

Prefiere cambios pequeños y reversibles:

1. Toma una captura de la topología actual, plan de IP, ajustes DHCP, DNS y reglas de firewall.
2. Reserva direcciones de infraestructura para gateway, DNS, controlador, AP, NAS y endpoint VPN.
3. Crea la nueva zona o VLAN sin mover dispositivos críticos.
4. Mueve un cliente de prueba y valida DHCP, DNS, routing, Internet y comportamiento de bloqueo.
5. Añade excepciones de firewall estrechas para los flujos requeridos.
6. Mueve un grupo de dispositivos de bajo riesgo.
7. Añade acceso VPN con la ruta y política de firewall más estrechas que satisfagan el caso de uso.
8. Documenta el estado final, las excepciones conocidas y los comandos o pasos de rollback.

## Lista de revisión

- Cada red existe por una razón y tiene un límite de confianza claro.
- Ninguna interfaz de administración es accesible desde guest, IoT o Internet público.
- La caída de DNS no elimina la capacidad del operador para recuperarse localmente.
- Los cambios de scope DHCP se probaron en un cliente antes de un despliegue amplio.
- Los clientes VPN reciben solo las rutas y el DNS que necesitan.
- Las reglas de firewall son deny-by-default entre zonas, con excepciones con nombre.
- El operador aún puede alcanzar las superficies de administración de gateway, switch, AP, DNS y VPN.
- El rollback está documentado con la misma terminología que la UI o CLI de la plataforma elegida.

## Antipatrónes

- Segmentar redes sin saber qué puertos de switch y SSID transportan qué VLAN.
- Mover la estación de administración fuera de la única red de gestión accesible.
- Apuntar todos los scopes DHCP a un Pi-hole antes de probar el DNS de respaldo.
- Publicar NAS, DNS, router o gestión del hipervisor directamente a Internet.
- Tratar el acceso VPN como equivalente a toda la LAN confiable.
- Añadir reglas de firewall allow-all temporales y olvidar retirarlas.
- Copiar comandos de otro proveedor o versión de firmware sin comprobar la sintaxis exacta de la plataforma.

## Ver también

- Skill: `homelab-network-setup`
- Skill: `network-config-validation`
- Skill: `network-interface-health`
