# Debian 13 OpenVPN Debian ISO

ISO personalizada de **Debian 13 (Trixie)** que instala automáticamente un servidor OpenVPN completo con panel de administración web.

## Características

- Instalación casi desatendida — solo pide contraseña de root y configuración de red
- Asistente interactivo de primer arranque para configurar OpenVPN (PKI, IP pública, DNS, subred VPN, perfil gateway/server)
- Panel web para gestionar clientes VPN: crear, revocar, renovar y descargar perfiles `.ovpn`
- Soporte para proteger claves privadas de clientes con contraseña
- Caducidad de certificados configurable (1–120 meses)
- Boot dual BIOS + UEFI
- Sistema mínimo sin entorno gráfico

## Requisitos

- Máquina (física o virtual) con arquitectura amd64
- Mínimo 1 GB RAM, 10 GB disco
- Conexión a Internet durante la instalación

## Instalación

1. Grabar la ISO en un USB (`dd`, Rufus, balenaEtcher) o montarla en una VM
2. Arrancar desde la ISO
3. El instalador pedirá:
   - Contraseña de root
   - Configuración de red (DHCP o IP estática)
4. El resto es automático (particionado, paquetes, configuración de servicios)
5. Al reiniciar, se lanza automáticamente el asistente de configuración VPN

## Asistente de primer arranque

El wizard interactivo configura:

- **Datos regionales** — País, provincia, ciudad, organización, email (para los certificados PKI)
- **Red** — Perfil (gateway = túnel completo / server = túnel dividido), IP pública, subred LAN, DNS, subred VPN
- **Aplicación automática** — Genera la PKI, certificados, `server.conf`, reglas iptables y arranca OpenVPN

## Panel web

Accesible en `http://<IP_DEL_SERVIDOR>` tras completar el asistente.

**Credenciales por defecto:** `admin@admin.com` / `password` (cambiar tras el primer acceso)

Funciones: crear clientes, descargar `.ovpn`, revocar, renovar certificados, gestionar usuarios del panel.

## Comandos útiles (SSH)

```bash
openvpn-addclient <nombre> <email>    # Crear cliente por CLI
openvpn-revoke <nombre>               # Revocar cliente
vpn-manager-token                     # Ver token de la API interna
systemctl status openvpn-server@server # Estado del servicio
vpn-firstboot-config.sh               # Re-ejecutar el asistente
```

## Componentes incluidos

OpenVPN, EasyRSA 3, Nginx, PHP-FPM, Laravel (SQLite), Python 3, Fail2ban, iptables-persistent

## Licencia

GPLv3 — Ver [LICENSE](LICENSE)
