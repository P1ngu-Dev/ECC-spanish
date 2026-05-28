---
name: homelab-network-setup
description: Practical home and homelab network planning for gateways, switches, access points, IP ranges, DHCP reservations, DNS, cabling, and common beginner mistakes.
origin: community
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# Configuración de red para homelab

Usa este skill para diseñar una red doméstica o de pequeño laboratorio que pueda crecer sin necesitar una reconstrucción completa.

## Cuándo usarlo

- Planificar una red doméstica nueva o rediseñar una configuración solo con router del ISP.
- Elegir roles para gateway, switch y puntos de acceso.
- Diseñar rangos IP, scopes DHCP, reservas estáticas y DNS.
- Prepararte para futuras VLAN, Pi-hole, NAS, servidores de laboratorio o acceso VPN.
- Solucionar una red nueva que tenga doble NAT, Wi‑Fi inestable o direcciones de servidor cambiantes.

## Cómo funciona

Empieza separando los roles de los dispositivos:

```text
Internet
  |
Modem or ONT
  |
Gateway or router      NAT, firewall, DHCP, DNS, inter-VLAN routing
  |
Managed switch         wired clients, AP uplinks, optional VLAN trunks
  |
Access points          Wi-Fi only; ideally wired backhaul
Servers and NAS        stable addresses, DNS names, monitoring
Clients and IoT        DHCP pools, isolated later if VLANs are available
```

Elige un gateway que encaje con el operador, no solo con la lista de funciones:

| Opción | Encaje ideal | Notas |
| --- | --- | --- |
| ISP router | Solo Internet básico | Control limitado y a menudo mal soporte de VLAN |
| UniFi gateway | Red doméstica gestionada | Buena UI, dependencia del ecosistema |
| OPNsense o pfSense | Homelab flexible | Fuerte control de VLAN, firewall, VPN y DNS |
| MikroTik | Usuarios avanzados de red | Potente, pero fácil de configurar mal |
| Linux router | Quienes disfrutan trastear | Documenta el rollback antes de usarlo como gateway principal |

## Plan de IP

Evita el valor predeterminado más común, `192.168.1.0/24`, cuando esperas usar VPN.
Suele chocar con hoteles, oficinas y routers del ISP.

```text
Example small homelab plan:

192.168.10.0/24  trusted clients
192.168.20.0/24  IoT and media devices
192.168.30.0/24  servers and NAS
192.168.40.0/24  guest Wi-Fi
192.168.99.0/24  network management

Gateway convention: .1
Infrastructure reservations: .2 through .49
Dynamic DHCP pool: .50 through .240
Spare room: .241 through .254
```

Usa `home.arpa` para nombres locales. Está reservado para redes domésticas y evita los problemas de fuga/conflicto de nombres improvisados como `home.lan`.

```text
nas.home.arpa
pihole.home.arpa
gateway.home.arpa
switch-01.home.arpa
```

## DHCP y DNS

- Usa reservas DHCP para cualquier host al que hagas SSH, marques en favoritos, monitorices o expongas como servicio.
- Entrega el gateway como DNS hasta que se despliegue intencionalmente un resolvedor local.
- Si usas Pi-hole u otro filtro DNS, dale primero una reserva y luego apunta las opciones DNS de DHCP a esa dirección.
- Mantén un pequeño rango estático/reservado por subred para que los reemplazos no choquen con leases dinámicos.

## Cableado y Wi‑Fi

- Prioriza el backhaul por cable para los AP antes que malla cuando puedas tender Ethernet.
- Usa un switch PoE para AP y cámaras si el presupuesto lo permite.
- Etiqueta ambos extremos de cada cable y mantén un mapa simple de puertos.
- Pon el gateway, switch, servidor DNS y NAS en un UPS si los cortes son frecuentes.

## Ejemplos

### Mejora para principiantes

Objetivo: conservar el router del ISP pero estabilizar un laboratorio pequeño.

1. Define reservas DHCP para NAS, Pi y cualquier host con SSH.
2. Mueve los nombres locales a `home.arpa`.
3. Desactiva servidores DHCP duplicados en routers secundarios o AP.
4. Conecta el AP principal por cable en vez de depender del backhaul inalámbrico.

### Plan preparado para VLAN

Objetivo: preparar segmentación futura sin habilitarla todavía.

1. Elige rangos /24 que no se solapen para trusted, IoT, servers, guest y management.
2. Reserva .1 para el gateway y .2-.49 para infraestructura en cada subred.
3. Compra un gateway y un switch que soporten VLAN y reglas firewall inter-VLAN.
4. Documenta qué SSID y puertos de switch se mapearán más adelante a cada red.

## Antipatrónes

- Doble NAT sin una razón o documentación.
- Usar `192.168.1.0/24` cuando se planea acceso VPN.
- Direcciones dinámicas para NAS, Pi-hole, Home Assistant u otros hosts de servicio.
- Routers de consumo reutilizados como AP mientras siguen teniendo DHCP activo.
- Redes planas con cámaras, enchufes inteligentes, portátiles y servidores compartiendo el mismo límite de confianza.

## Ver también

- Skill: `network-interface-health`
- Skill: `network-config-validation`
