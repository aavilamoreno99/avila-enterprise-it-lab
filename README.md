# AVILA Enterprise IT Lab

Laboratorio personal de infraestructura IT creado para practicar, documentar y simular un entorno corporativo utilizando máquinas virtuales, Windows Server, Active Directory, redes, servicios de infraestructura, seguridad y monitorización.

El proyecto se desarrolla por fases y cada fase queda documentada con configuraciones, pruebas y capturas de pantalla.

**Objetivo:** construir progresivamente una infraestructura IT completa y documentar técnicamente cada componente para poder consultar el proceso, solucionar incidencias y demostrar los conocimientos adquiridos.

---

## 🏗️ Arquitectura del laboratorio

Actualmente la infraestructura utiliza una red virtual aislada:

```text
Red física
192.168.0.0/24
       │
       │
   HOST Windows
   192.168.0.14
       │
       │ VirtualBox
       │
       ▼
┌───────────────────────────────┐
│          AVILA-LAB            │
│      192.168.10.0/24          │
│                               │
│  ┌─────────────┐              │
│  │    DC01     │              │
│  │ .10         │              │
│  │             │              │
│  │ AD DS       │              │
│  │ DNS         │              │
│  │ DHCP        │              │
│  └──────┬──────┘              │
│         │                     │
│  ┌──────▼──────┐              │
│  │   FILE01    │              │
│  │ .20         │              │
│  │             │              │
│  │ File Server │              │
│  │ SMB / NTFS  │              │
│  └──────┬──────┘              │
│         │                     │
│  ┌──────▼──────┐              │
│  │  WIN10-01   │              │
│  │ DHCP        │              │
│  │             │              │
│  │ Cliente     │              │
│  └─────────────┘              │
│                               │
└───────────────────────────────┘
```

### Direccionamiento

| Equipo     | IP              | Función            |
| ---------- | --------------- | ------------------ |
| `DC01`     | `192.168.10.10` | AD DS / DNS / DHCP |
| `FILE01`   | `192.168.10.20` | File Server / SMB  |
| `LINUX01`  | `192.168.10.30` | Linux / Servicios  |
| `MON01`    | `192.168.10.40` | Monitorización     |
| `SEC01`    | `192.168.10.50` | Seguridad          |
| `WIN10-01` | DHCP            | Cliente Windows    |

Algunas máquinas forman parte de la planificación futura y todavía no están desplegadas.

---

# 📋 Estado del proyecto

### 🟢 Completado

*[x] Configuración de red virtual con VirtualBox
* [x] Red interna `AVILA-LAB`
* [x] Configuración IP de servidores
* [x] Instalación y configuración de Windows Server
* [x] Active Directory Domain Services
* [x] DNS
* [x] DHCP
* [x] Creación y organización de OUs
* [x] Usuarios y grupos de seguridad
* [x] Group Policy
* [x] Unión de equipos Windows al dominio
* [x] File Server
* [x] Recursos compartidos SMB
* [x] Permisos NTFS
* [x] Pruebas de acceso mediante grupos de seguridad

### 🟡 Próximamente

* [ ] Mapeo automático de unidades de red mediante GPO
* [ ] Gestión avanzada de GPO
* [ ] Automatización con PowerShell
* [ ] Administración remota de servidores
* [ ] Políticas de seguridad de Windows
* [ ] Auditoría de accesos a recursos
* [ ] Gestión de usuarios y equipos mediante PowerShell
* [ ] Scripts de administración y mantenimiento

### 🔵 Planificado

* [ ] Despliegue de `LINUX01`
* [ ] Administración de Linux
* [ ] DNS/DHCP y servicios Linux
* [ ] Servidor web
* [ ] Monitorización con `MON01`
* [ ] Logs y alertas
* [ ] Backup y recuperación
* [ ] Implementación de `SEC01`
* [ ] Herramientas de análisis y seguridad
* [ ] Firewall/router con pfSense
* [ ] VLANs
* [ ] Segmentación de red
* [ ] VPN
* [ ] Hardening de servidores
* [ ] Análisis de tráfico con Wireshark
* [ ] Escaneo y análisis con Nmap
* [ ] Integración con servicios cloud
* [ ] Azure / Microsoft Entra
* [ ] Documentación de troubleshooting
* [ ] Simulación de incidencias reales

---

# 📚 Documentación

Cada fase del laboratorio se documenta individualmente:

| Documento                                                                | Contenido                           | Estado |
| ------------------------------------------------------------------------ | ----------------------------------- | ------ |
| [01 - Network and Virtualization](docs/01-network-and-virtualization.md) | Red virtual y configuración inicial | 🟢     |
| [02 - DHCP, DNS & AD](docs/02-dhcp-dns-ad.md)                            | DHCP, DNS y Active Directory        | 🟢     |
| [03 - Active Directory](docs/03-active-directory.md)                     | OUs, usuarios y grupos              | 🟢     |
| [04 - Group Policy](docs/04-group-policy.md)                             | Creación y aplicación de GPO        | 🟢     |
| [05 - Domain Join](docs/05-domain-join.md)                               | Unión de Windows al dominio         | 🟢     |
| [06 - File Server](docs/06-file-server.md)                               | SMB, NTFS y per                     |        |
