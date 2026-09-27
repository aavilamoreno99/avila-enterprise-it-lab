# AVILA Enterprise IT Lab

Laboratorio personal de infraestructura IT creado para practicar y documentar la administración de sistemas, redes y servicios en un entorno similar al de una pequeña infraestructura corporativa.

El proyecto se desarrolla por fases y cada fase queda documentada con configuraciones, pruebas, capturas de pantalla y resolución de incidencias.

**Objetivo:** aprender haciendo. Desplegar servicios, configurarlos, realizar pruebas, detectar problemas, resolverlos y documentar el proceso.

---

# Arquitectura del laboratorio

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

## Direccionamiento

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

# Estado del proyecto

## Completado

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

## En desarrollo

* [ ] Mapeo automático de unidades de red mediante GPO
* [ ] Gestión avanzada de GPO
* [ ] Automatización con PowerShell
* [ ] Administración remota de servidores
* [ ] Políticas de seguridad de Windows
* [ ] Auditoría de accesos a recursos
* [ ] Gestión de usuarios y equipos mediante PowerShell
* [ ] Scripts de administración y mantenimiento

## Planificado

* [ ] Despliegue de `LINUX01`
* [ ] Administración de Linux
* [ ] Servicios de red en Linux
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

# Documentación

Cada fase del laboratorio se documenta individualmente:

| Documento                                                                | Contenido                             | Estado     |
| ------------------------------------------------------------------------ | ------------------------------------- | ---------- |
| [01 - Network and Virtualization](docs/01-network-and-virtualization.md) | Red virtual y configuración inicial   | Completado |
| [02 - DHCP, DNS & AD](docs/02-dhcp-dns-ad.md)                            | DHCP, DNS y Active Directory          | Completado |
| [03 - Active Directory](docs/03-active-directory.md)                     | OUs, usuarios y grupos                | Completado |
| [04 - Group Policy](docs/04-group-policy.md)                             | Creación y aplicación de GPO          | Completado |
| [05 - Domain Join](docs/05-domain-join.md)                               | Unión de Windows al dominio           | Completado |
| [06 - File Server](docs/06-file-server.md)                               | SMB, NTFS y permisos por departamento | Completado |

Las nuevas fases se irán incorporando a esta tabla a medida que avance el laboratorio.

---

# Active Directory

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

## Usuarios

Actualmente existen diferentes usuarios de prueba para simular distintos perfiles dentro de la infraestructura:

```text
Alejandro
Manuel
Belen
Valeriano
```

## Grupos de seguridad

```text
GG-IT
└── Alejandro

GG-Soporte
└── Manuel

GG-Administracion
└── Belen
```

Los usuarios y grupos se utilizan para realizar pruebas de permisos y acceso a los recursos compartidos del servidor.

---

# File Server

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

# Tecnologías

## Tecnologías utilizadas actualmente

### Infraestructura

* VirtualBox
* Windows Server 2025
* Windows 10

### Microsoft

* Active Directory
* Group Policy
* Windows Server
* DNS
* DHCP
* SMB
* NTFS

### Redes

* TCP/IP
* DHCP
* DNS
* Redes virtuales

### Administración

* PowerShell
* Gestión de usuarios y grupos
* Gestión de permisos
* Administración de recursos compartidos

## Tecnologías previstas

### Sistemas e infraestructura

* Linux
* pfSense
* Servidor web
* Monitorización
* Backup y recuperación

### Microsoft y Cloud

* Microsoft Entra
* Azure
* Automatización avanzada con PowerShell

### Redes

* VLAN
* VPN
* Routing
* Firewall
* Segmentación de red
* Cisco / Packet Tracer

### Seguridad

* Hardening
* Auditoría
* Nmap
* Wireshark
* Análisis de logs
* Simulación de incidencias

---

# Objetivos de aprendizaje

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

El objetivo no es únicamente desplegar servicios, sino entender cómo funcionan, configurarlos, probarlos y documentar posibles incidencias y soluciones.

---

# Metodología

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

# Documentación visual

Las configuraciones importantes se acompañan de capturas de pantalla y ejemplos realizados dentro del laboratorio.

Las imágenes utilizadas en la documentación se encuentran en:

```text
screenshots/
```

Los diagramas de infraestructura se almacenan en:

```text
diagrams/
```

---

# Evolución del proyecto

Este laboratorio es un proyecto en evolución.

La infraestructura se irá ampliando progresivamente para incorporar nuevos servicios, tecnologías y escenarios de troubleshooting.

La planificación puede cambiar a medida que se incorporen nuevos objetivos o se detecten nuevas necesidades durante el desarrollo.

Cada nueva fase se documentará para mantener un historial del proceso de configuración, las pruebas realizadas, los problemas encontrados y las soluciones aplicadas.

---

## Autor

**Alejandro Ávila Moreno**

Proyecto personal de laboratorio de infraestructura IT orientado al aprendizaje práctico, administración de sistemas, redes y seguridad.
