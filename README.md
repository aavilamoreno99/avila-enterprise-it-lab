# AVILA Enterprise IT Lab

Laboratorio personal de infraestructura IT creado para practicar, documentar y simular un entorno corporativo utilizando máquinas virtuales, Windows Server, Active Directory, redes, servicios de infraestructura, seguridad y monitorización.

El proyecto se desarrolla por fases y cada fase queda documentada con configuraciones, pruebas y capturas de pantalla.

> **Objetivo:** construir progresivamente una infraestructura IT completa y documentar técnicamente cada componente para poder consultar el proceso, solucionar incidencias y demostrar los conocimientos adquiridos.

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

> Algunas máquinas forman parte de la planificación futura y todavía no están desplegadas.

---

# 📋 Estado del proyecto

### 🟢 Completado

* [x] Configuración de red virtual con VirtualBox
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

| Documento                                                                | Contenido                             | Estado |
| ------------------------------------------------------------------------ | ------------------------------------- | ------ |
| [01 - Network and Virtualization](docs/01-network-and-virtualization.md) | Red virtual y configuración inicial   | 🟢     |
| [02 - DHCP, DNS & AD](docs/02-dhcp-dns-ad.md)                            | DHCP, DNS y Active Directory          | 🟢     |
| [03 - Active Directory](docs/03-active-directory.md)                     | OUs, usuarios y grupos                | 🟢     |
| [04 - Group Policy](docs/04-group-policy.md)                             | Creación y aplicación de GPO          | 🟢     |
| [05 - Domain Join](docs/05-domain-join.md)                               | Unión de Windows al dominio           | 🟢     |
| [06 - File Server](docs/06-file-server.md)                               | SMB, NTFS y permisos por departamento | 🟢     |

Las nuevas fases se irán incorporando a esta tabla a medida que avance el laboratorio.

---

# 🔐 Active Directory

Dominio utilizado:

```text
avila-tech.local
```

Estructura actual:

```text
avila-tech.local
│
├── Usuarios
│   ├── Administracion
│   ├── IT
│   └── Soporte
│
├── Equipos
│   ├── Windows
│   └── Servidores
│
├── Grupos
│
└── Domain Controllers
```

### Grupos de seguridad

```text
GG-IT
└── Alejandro

GG-Soporte
└── Manuel

GG-Administracion
└── Belen
```

---

# 🗄️ File Server

`FILE01` proporciona recursos compartidos SMB para diferentes departamentos:

```text
\\FILE01\IT
\\FILE01\Soporte
\\FILE01\Administracion
```

El acceso se controla mediante grupos de seguridad de Active Directory y permisos NTFS.

Ejemplo:

```text
GG-IT
└── \\FILE01\IT

GG-Soporte
└── \\FILE01\Soporte

GG-Administracion
└── \\FILE01\Administracion
```

Se han realizado pruebas de acceso utilizando diferentes usuarios del dominio para verificar que cada usuario únicamente puede acceder al recurso correspondiente.

---

# 🛠️ Tecnologías

### Infraestructura

* VirtualBox
* Windows Server 2025
* Windows 10
* Linux
* pfSense

### Microsoft

* Active Directory
* Group Policy
* Windows Server
* DNS
* DHCP
* SMB
* NTFS
* PowerShell
* Microsoft Entra
* Azure

### Redes

* TCP/IP
* DHCP
* DNS
* VLAN
* VPN
* Routing
* Firewall
* Wireshark
* Cisco / Packet Tracer

### Seguridad

* Hardening
* Gestión de permisos
* Auditoría
* Nmap
* Wireshark
* Análisis de logs
* Segmentación de red

### Monitorización

* Logs
* Alertas
* Monitorización de servidores
* Disponibilidad de servicios

---

# 🎯 Objetivos de aprendizaje

El laboratorio está orientado principalmente a practicar:

* Administración de sistemas Windows y Linux.
* Administración de Active Directory.
* Gestión de usuarios, grupos y permisos.
* Configuración de servicios de red.
* Administración de servidores.
* Redes y segmentación.
* Automatización mediante PowerShell.
* Monitorización e identificación de incidencias.
* Seguridad y hardening.
* Troubleshooting.
* Documentación técnica.

El objetivo no es únicamente desplegar servicios, sino **entender cómo funcionan, configurarlos, probarlos y documentar posibles incidencias y soluciones**.

---

# 🧪 Metodología

Cada nueva fase sigue, siempre que sea posible, este proceso:

```text
1. Diseñar
      ↓
2. Desplegar
      ↓
3. Configurar
      ↓
4. Probar
      ↓
5. Provocar / analizar incidencias
      ↓
6. Resolver
      ↓
7. Documentar
```

De esta forma, el laboratorio no se limita a una instalación de servicios, sino que también permite practicar tareas habituales de administración y soporte IT.

---

# 📸 Documentación visual

Las configuraciones importantes se acompañan de capturas de pantalla y ejemplos reales realizados dentro del laboratorio.

Las imágenes utilizadas en la documentación se encuentran en:

```text
screenshots/
```

Los diagramas de infraestructura se almacenan en:

```text
diagrams/
```

---

# 🚀 Evolución del proyecto

Este laboratorio es un proyecto en evolución. La infraestructura se irá ampliando progresivamente para incorporar nuevos servicios, tecnologías y escenarios de troubleshooting.

La planificación puede cambiar a medida que se incorporen nuevos objetivos o se detecten necesidades durante el desarrollo.

---

## 👤 Autor

**Alejandro Ávila Moreno**

Proyecto personal de laboratorio de infraestructura IT orientado al aprendizaje práctico, administración de sistemas, redes y seguridad.
