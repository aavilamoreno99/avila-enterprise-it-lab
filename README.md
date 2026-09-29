# AVILA Enterprise IT Infrastructure

Proyecto personal de infraestructura IT orientado al diseño, implementación y administración de un entorno empresarial simulado.

El proyecto parte de una red interna aislada y evoluciona progresivamente hacia una infraestructura corporativa con servicios de red, sistemas Windows y Linux, seguridad, monitorización, conectividad entre sedes y servicios cloud.

El objetivo no es únicamente desplegar tecnologías, sino construir una infraestructura funcional, documentar las decisiones técnicas y comprobar su funcionamiento mediante pruebas reales.

---

## Situación actual

Actualmente se ha construido una infraestructura LAN funcional basada en Windows Server y Active Directory.

La infraestructura permite:

* Gestionar usuarios y grupos mediante Active Directory.
* Proporcionar resolución DNS.
* Asignar direcciones IP mediante DHCP.
* Unir equipos Windows al dominio.
* Aplicar políticas mediante GPO.
* Gestionar permisos de acceso por departamentos.
* Proporcionar recursos compartidos mediante SMB.
* Separar los servicios de dominio del servidor de archivos.
* Validar el acceso utilizando diferentes cuentas de usuario.

### Dominio

`avila-tech.local`

### Red interna

`AVILA-LAB — 192.168.10.0/24`

---

## Arquitectura actual

```text
                         AVILA-LAB
                      192.168.10.0/24
                             |
        +--------------------+--------------------+
        |                    |                    |
      DC01                 FILE01             WIN10-01
  192.168.10.10        192.168.10.20          DHCP
        |                    |                    |
        |                    |              Cliente Windows
        |                    |
   AD DS / DNS          File Server
      DHCP                  SMB
        |                  NTFS
        |
        +---------- Dominio ----------+
                  avila-tech.local
```

### Servidores

| Equipo   | Sistema             | IP            | Función            |
| -------- | ------------------- | ------------- | ------------------ |
| DC01     | Windows Server 2025 | 192.168.10.10 | AD DS, DNS, DHCP   |
| FILE01   | Windows Server 2025 | 192.168.10.20 | File Server, SMB   |
| WIN10-01 | Windows 10          | DHCP          | Cliente de dominio |

---

## Infraestructura implementada

### Red y servicios

* Red interna aislada mediante VirtualBox.
* Direccionamiento IPv4.
* DHCP.
* DNS.
* Conectividad entre máquinas virtuales.

### Active Directory

* Dominio `avila-tech.local`.
* Unidades Organizativas.
* Usuarios.
* Grupos de seguridad.
* Organización de usuarios y equipos.

### Administración

* Group Policy.
* Políticas vinculadas a OUs.
* Gestión de equipos unidos al dominio.

### File Server

* Servidor independiente `FILE01`.
* Estructura de carpetas por departamentos.
* Permisos NTFS.
* Recursos compartidos SMB.
* Control de acceso mediante grupos de Active Directory.

### Validación

La infraestructura se ha probado utilizando diferentes usuarios y equipos para comprobar:

* Resolución DNS.
* Conectividad.
* Unión al dominio.
* Aplicación de GPO.
* Autenticación.
* Acceso a recursos compartidos.
* Restricciones de permisos por departamento.

---

## Cómo se ha construido

La implementación y las comprobaciones técnicas están documentadas en:

**[HOW-TO](docs/HOW-TO.md)**

El documento recoge el proceso de construcción de la infraestructura, las configuraciones realizadas, las decisiones técnicas y las pruebas utilizadas para validar cada componente.

---

## Evolución de la infraestructura

La infraestructura continuará creciendo sobre la misma base para aproximarla progresivamente a un entorno empresarial más completo.

### Seguridad e infraestructura de red

Se incorporarán progresivamente:

* Firewall y control del tráfico.
* Segmentación de red.
* VLAN.
* ACL.
* NAT.
* VPN.
* Hardening de servidores.
* Registro y análisis de eventos.

### Sistemas Linux

Se añadirá infraestructura Linux para ampliar el entorno:

* Administración de servidores Linux.
* Servicios de red.
* Gestión de usuarios y permisos.
* Servicios web.
* Integración con la infraestructura existente.
* Automatización de tareas.

### Monitorización

Se incorporará monitorización de servidores y servicios para poder detectar:

* Caídas de servicios.
* Problemas de conectividad.
* Consumo de recursos.
* Errores.
* Disponibilidad de sistemas.

### Conectividad entre sedes

La infraestructura evolucionará de una única LAN hacia un escenario con diferentes ubicaciones.

```text
                 INTERNET
                     |
              FIREWALL / VPN
                     |
          +----------+----------+
          |                     |
       SEDE A                 SEDE B
        LAN                    LAN
          |                     |
     Servidores             Clientes
     Usuarios               Servicios
```

Esto permitirá trabajar con conceptos como:

* WAN.
* Routing.
* VPN Site-to-Site.
* NAT.
* Firewalls.
* Comunicación segura entre redes.

### Microsoft Azure

Una parte posterior del proyecto incorporará servicios cloud mediante Microsoft Azure.

La infraestructura evolucionará hacia un escenario híbrido combinando recursos locales y cloud:

```text
                  INTERNET
                      |
              FIREWALL / VPN
                 /         \
                /           \
       INFRAESTRUCTURA       AZURE
          LOCAL                |
             |          Entra ID / VMs
       AD / FILE01            |
       Clientes        Servicios Cloud
```

Entre los componentes que se estudiarán dentro de esta evolución:

* Microsoft Azure.
* Microsoft Entra ID.
* Máquinas virtuales.
* Redes virtuales.
* Identidad y acceso.
* Integración entre infraestructura local y cloud.
* Seguridad en entornos híbridos.

El objetivo será comprobar cómo una infraestructura inicialmente local puede evolucionar hacia un modelo híbrido.

---

## Arquitectura objetivo

La evolución del proyecto busca llegar progresivamente a una infraestructura empresarial compuesta por:

```text
                         INTERNET
                             |
                    FIREWALL / VPN
                             |
              +--------------+--------------+
              |                             |
             WAN                          AZURE
              |                             |
       +------+------+                +-----+------+
       |             |                |            |
    SEDE A        SEDE B          Entra ID       VMs
       |             |                |            |
    LAN/VLAN      LAN/VLAN       Servicios     Servicios
       |             |
   AD / DNS       Clientes
   FILE01         Servicios
   Clientes
```

Esta arquitectura se irá construyendo progresivamente a partir de la infraestructura que ya está funcionando actualmente.


## Tecnologías actuales

* VirtualBox
* Windows Server 2025
* Windows 10
* Active Directory
* DNS
* DHCP
* Group Policy
* SMB
* NTFS
* IPv4
* PowerShell

## Tecnologías previstas

* Linux
* VLAN
* Routing
* Firewall
* VPN
* Monitorización
* Automatización
* Microsoft Azure
* Microsoft Entra ID
* Infraestructura híbrida

---

## Resultado

El proyecto cuenta actualmente con una infraestructura empresarial simulada funcional, con servicios de red, dominio, gestión de usuarios, políticas, servidor de archivos y control de acceso.

A partir de esta base se continuará ampliando la infraestructura para incorporar nuevas necesidades de red, sistemas, seguridad, conectividad y cloud.
