---
name: homelab-pihole-dns
description: Pi-hole installation, blocklist management, DNS-over-HTTPS setup, DHCP integration, local DNS records, and troubleshooting broken DNS resolution on a home network.
origin: community
---
Traducción al español (es).
<!-- translator: OpenCode translation; date: 2026-05-28 -->

# DNS de Pi-hole para homelab

Pi-hole es un bloqueador de anuncios a nivel de DNS para toda la red que corre en una Raspberry Pi o cualquier host Linux.
Cada dispositivo de tu red obtiene bloqueo automático de dominios de anuncios y malware, sin necesidad de extensión del navegador.

## Cuándo usarlo

- Instalar Pi-hole en una Raspberry Pi o host Linux
- Configurar Pi-hole como servidor DNS para una red doméstica
- Añadir o gestionar listas de bloqueo
- Configurar resolutores upstream con DNS-over-HTTPS (DoH)
- Crear registros DNS locales (por ejemplo `nas.home.lan`, `pi.home.lan`)
- Solucionar dispositivos que pierden Internet después de instalar Pi-hole
- Ejecutar Pi-hole junto con DHCP o en su lugar

## Cómo funciona Pi-hole

```text
Normal flow (without Pi-hole):
  Device → requests ads.tracker.com → ISP DNS → real IP → ads load

With Pi-hole:
  Device → requests ads.tracker.com → Pi-hole DNS → blocked (returns 0.0.0.0) → no ad

All DNS queries go through Pi-hole first.
Pi-hole checks against blocklists.
Blocked domains return a null response — the ad/tracker never loads.
Allowed domains get forwarded to your upstream resolver (Cloudflare, Google, etc.).
```

## Instalación

### Docker (recomendado)

Docker es la forma más sencilla de instalar Pi-hole y hace que las actualizaciones y copias de seguridad sean directas.

```yaml
# docker-compose.yml
services:
  pihole:
    image: pihole/pihole:<pinned-release-tag>
    container_name: pihole
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80/tcp"          # Web admin
    environment:
      TZ: "America/New_York"
      WEBPASSWORD: "${PIHOLE_WEBPASSWORD}"   # set via .env file or secret
      PIHOLE_DNS_: "1.1.1.1;1.0.0.1"
      DNSMASQ_LISTENING: "all"
    volumes:
      - "./etc-pihole:/etc/pihole"
      - "./etc-dnsmasq.d:/etc/dnsmasq.d"
    restart: unless-stopped
    cap_add:
      - NET_ADMIN              # only needed if Pi-hole will serve DHCP
```

Replace `<pinned-release-tag>` with a current Pi-hole release tag before deploying.
Avoid `latest` for long-lived DNS infrastructure so upgrades are deliberate and reviewable.

Set `PIHOLE_WEBPASSWORD` in a `.env` file next to `docker-compose.yml`, chmod it to `600`, and keep it out of git — do not put the password directly in the compose file.

Access web admin at: `http://<pi-ip>/admin`

### Instalación bare-metal (Raspberry Pi OS / Debian / Ubuntu)

Pi-hole requires a static IP before installing.

```bash
# Step 1: Assign a static IP (edit /etc/dhcpcd.conf on Pi OS)
sudo nano /etc/dhcpcd.conf
# Add at the bottom:
interface eth0
static ip_address=192.168.3.2/24
static routers=192.168.3.1
static domain_name_servers=192.168.3.1

# Step 2: Download and inspect the installer before running it.
# Prefer the package or installer path documented by Pi-hole for your OS/version.
curl -sSL https://install.pi-hole.net -o pi-hole-install.sh
less pi-hole-install.sh   # review before proceeding

# Step 3: Run
bash pi-hole-install.sh

# Follow the interactive installer:
#   1. Select network interface (eth0 for wired — recommended)
#   2. Select upstream DNS (Cloudflare or leave default — can change later)
#   3. Confirm static IP
#   4. Install the web admin interface (recommended)
#   5. Note the admin password shown at the end
```

## Apuntar tu red a Pi-hole

```text
# Method 1: Change DNS in your router DHCP settings (recommended)
  Router admin UI → DHCP Settings → DNS Server
  Primary DNS: 192.168.3.2  (Pi-hole IP)
  Secondary DNS: leave blank for strict blocking, or use a second Pi-hole.
                 A public fallback such as 1.1.1.1 improves availability during
                 rollout but can bypass blocking because clients may query it.

  All devices get Pi-hole as DNS automatically on next DHCP renewal.
  Force renewal: reconnect Wi-Fi or run 'sudo dhclient -r && sudo dhclient' on Linux

# Method 2: Per-device DNS (useful for testing before network-wide rollout)
  Windows: Control Panel → Network Adapter → IPv4 Properties → set DNS manually
  macOS: System Settings → Network → Details → DNS → set manually
  Linux: /etc/resolv.conf or NetworkManager

# Method 3: Pi-hole as DHCP server (replaces router DHCP)
  Pi-hole admin → Settings → DHCP → Enable
  Disable DHCP on your router first — two DHCP servers on the same network cause conflicts
  Advantage: hostname resolution works automatically (devices register their names)
```

## Gestión de listas de bloqueo

```text
# Pi-hole admin → Adlists → Add new adlist

# Recommended blocklists:
  https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts
  # default — 200k+ domains

  https://blocklistproject.github.io/Lists/malware.txt
  # malware domains

  https://blocklistproject.github.io/Lists/tracking.txt
  # tracking/telemetry

# After adding a list:
  Tools → Update Gravity  (downloads and compiles all blocklists)

# If a site is blocked that should not be (false positive):
  Pi-hole admin → Whitelist → Add domain
  Example: api.my-legitimate-service.com

# Check what is being blocked in real time:
  Dashboard → Query Log  (live DNS query stream with block/allow status)
```

## Upstream DNS-over-HTTPS

DNS-over-HTTPS cifra tus consultas DNS para que tu ISP no vea qué sitios resuelves.

```bash
# Install cloudflared (Cloudflare's DoH proxy).
# Prefer Cloudflare's package repository for automatic signed package verification.
# If you download a binary directly, pin a release version and verify its checksum.
CLOUDFLARED_VERSION="<pinned-version>"
curl -LO "https://github.com/cloudflare/cloudflared/releases/download/${CLOUDFLARED_VERSION}/cloudflared-linux-arm64"
# Verify the checksum/signature from Cloudflare's release notes before installing.
sudo mv cloudflared-linux-arm64 /usr/local/bin/cloudflared
sudo chmod +x /usr/local/bin/cloudflared

# Create cloudflared config
sudo mkdir -p /etc/cloudflared
sudo tee /etc/cloudflared/config.yml << EOF
proxy-dns: true
proxy-dns-port: 5053
proxy-dns-upstream:
  - https://1.1.1.1/dns-query
  - https://1.0.0.1/dns-query
EOF

# Create systemd service
sudo cloudflared service install
sudo systemctl start cloudflared
sudo systemctl enable cloudflared

# Now point Pi-hole at the local DoH proxy:
#   Pi-hole admin → Settings → DNS → Custom upstream DNS
#   Set to: 127.0.0.1#5053
#   Uncheck all other upstream resolvers
```

## Registros DNS locales

Haz que tus servicios sean accesibles por nombre (por ejemplo `nas.home.lan`, `grafana.home.lan`).

> **Nota sobre el dominio:** `.home.lan` se usa mucho en homelabs y funciona en la práctica.
> El sufijo reservado por el IETF para uso local es `.home.arpa` (RFC 8375) — úsalo para seguir el estándar. Evita `.local` para registros DNS de Pi-hole porque entra en conflicto con mDNS/Bonjour.

```text
# Pi-hole admin → Local DNS → DNS Records

  Domain              IP
  nas.home.lan        192.168.30.10
  pi.home.lan         192.168.30.2
  grafana.home.lan    192.168.30.3
  proxmox.home.lan    192.168.30.4

# From any device on your network:
  ping nas.home.lan        → 192.168.30.10
  http://grafana.home.lan  → your Grafana dashboard

# For subdomains, add a CNAME:
  Pi-hole admin → Local DNS → CNAME Records
  Domain: portainer.home.lan → Target: pi.home.lan
```

## Solución de problemas

```bash
# Pi-hole bloquea algo que no debería
pihole -q example.com          # Comprueba si el dominio está bloqueado y por qué lista
pihole -w example.com          # Añade a whitelist inmediatamente

# DNS no resuelve nada
pihole status                  # Comprueba si pihole-FTL está ejecutándose
dig @192.168.3.2 google.com   # Prueba DNS directamente contra Pi-hole

# Reiniciar el DNS de Pi-hole
pihole restartdns

# Revisar logs de consultas para un dispositivo específico
pihole -t                      # Seguimiento en vivo de todas las consultas
# O filtra por cliente en el Query Log de la web admin

# Actualización de gravity de Pi-hole (refresca blocklists)
pihole -g
```

## Antipatrónes

```
# MALO: Depender de un solo Pi-hole sin vía de recuperación
# Si Pi-hole falla o el Pi pierde energía, DNS puede dejar de funcionar
# BUENO: Mantén un fallback documentado del router para rollback durante la configuración
# MEJOR: Ejecuta dos instancias de Pi-hole para redundancia; evita DNS público de respaldo si necesitas bloqueo estricto

# MALO: Instalar Pi-hole sin una IP estática
# Si el Pi obtiene una nueva IP DHCP, todos los dispositivos pierden DNS
# BUENO: Define primero una IP estática y luego instala Pi-hole

# MALO: Habilitar DHCP en Pi-hole sin desactivar antes el DHCP del router
# Dos servidores DHCP en la misma red entregan IPs en conflicto
# BUENO: Desactiva DHCP del router y luego habilita DHCP de Pi-hole

# MALO: No actualizar nunca gravity (blocklists)
# Se acumulan dominios nuevos de anuncios y malware — las listas obsoletas no los detectan
# BUENO: Programa una actualización semanal de gravity: pihole -g (o actívala en Settings → API)
```

## Buenas prácticas

- Asigna al Pi una IP estática o una reserva DHCP antes de instalar Pi-hole
- Usa Pi-hole como DNS primario; para redundancia, añade un segundo Pi-hole en lugar de un resolvedor público si necesitas bloqueo estricto
- Activa DoH (DNS-over-HTTPS) con cloudflared para consultas upstream cifradas
- Define `home.lan` como tu dominio local y crea registros DNS para todos tus servicios
- Revisa el Query Log de vez en cuando — las consultas bloqueadas te muestran qué hacen los dispositivos

## Skills relacionadas

- homelab-network-setup
- homelab-vlan-segmentation
- homelab-wireguard-vpn
