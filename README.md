# AVILA Enterprise IT Infrastructure

Proyecto personal de infraestructura IT orientado al diseño, implementación y administración de un entorno empresarial simulado.

El proyecto evoluciona progresivamente desde una LAN con servicios de dominio hasta una infraestructura empresarial con routing, seguridad de red, monitorización, administración de sistemas, copias de seguridad, conectividad entre sedes y servicios cloud.

El objetivo no es únicamente desplegar tecnologías, sino comprender su funcionamiento, documentar las decisiones técnicas y validar cada componente mediante pruebas reales.

---

## Situación actual

Se ha construido una infraestructura LAN funcional basada en Windows Server y Active Directory. Posteriormente, se ha ampliado con un firewall Linux que proporciona conectividad entre la red interna e Internet y con una plataforma centralizada de monitorización.

La infraestructura permite:

- Gestionar usuarios y grupos mediante Active Directory.
- Proporcionar resolución DNS y asignación de direcciones IP mediante DHCP.
- Unir equipos Windows al dominio y aplicar políticas mediante GPO.
- Gestionar permisos de acceso mediante grupos de seguridad, NTFS y recursos SMB.
- Separar los servicios de dominio del servidor de archivos.
- Proporcionar conectividad a Internet mediante un firewall Linux con routing y NAT.
- Centralizar la monitorización de los servidores Windows mediante Zabbix.
- Consultar métricas y gráficas de rendimiento de los servidores monitorizados.

La infraestructura se encuentra en una fase funcional de sistemas, red y monitorización. Las siguientes etapas ampliarán las capacidades de seguridad, recuperación, automatización y conectividad.

## Dominio y red

- **Dominio:** `avila-tech.local`
- **Red interna:** `AVILA-LAB — 192.168.10.0/24`
- **Entorno de virtualización:** Oracle VirtualBox
- **Red interna aislada:** `AVILA-LAB`
- **Salida a Internet:** mediante FW01 y su interfaz WAN conectada a la red NAT de VirtualBox.

---

## Arquitectura actual

```text
                         INTERNET
                             |
                      VirtualBox NAT
                             |
                    +------------------+
                    |       FW01       |
                    |  Ubuntu Server   |
                    | Routing / NAT   |
                    | LAN: 192.168.10.1|
                    +------------------+
                             |
                       AVILA-LAB
                     192.168.10.0/24
                             |
             +---------------+---------------+
             |               |               |
          +------+        +--------+      +---------+
          | DC01 |        | FILE01 |      | WIN10-01|
          +------+        +--------+      +---------+
          .10              .20           DHCP: .100
             |               |
        AD DS / DNS       SMB / NTFS
           DHCP          File Server
             |
       avila-tech.local


          +-------------------------+
          |        ZABBIX01         |
          |      Ubuntu Server      |
          | Zabbix Server / Web UI  |
          | PostgreSQL / Agent 2    |
          |     192.168.10.30       |
          +-------------------------+
                      |
             Monitorización de
              DC01 y FILE01
```

*El esquema representa la arquitectura lógica. Todos los servidores y el cliente pertenecen a la red interna `AVILA-LAB`; ZABBIX01 se comunica con los equipos monitorizados a través de esa red.*

## Servidores y equipos

| Equipo | Sistema operativo | Dirección IP | Función |
|---|---|---|---|
| FW01 | Ubuntu Server 24.04.5 LTS | `192.168.10.1` en LAN | Firewall, routing y NAT |
| DC01 | Windows Server 2025 | `192.168.10.10` | AD DS, DNS y DHCP |
| FILE01 | Windows Server 2025 | `192.168.10.20` | Servidor de archivos, SMB y NTFS |
| ZABBIX01 | Ubuntu Server 24.04.2 LTS | `192.168.10.30` | Monitorización centralizada |
| WIN10-01 | Windows 10 | DHCP, `192.168.10.100` | Cliente unido al dominio |

Las direcciones indicadas para los servidores corresponden a sus interfaces LAN. FW01 dispone además de una interfaz WAN conectada a VirtualBox NAT.

---

## Infraestructura implementada

### 1. Virtualización y red

- Creación y administración de máquinas virtuales en VirtualBox.
- Configuración de una red interna aislada.
- Direccionamiento IPv4.
- Configuración de interfaces de red.
- Comprobación de conectividad entre máquinas virtuales.
- Configuración de conectividad hacia Internet mediante FW01.

### 2. Active Directory y servicios de dominio

- Dominio `avila-tech.local`.
- Active Directory Domain Services.
- Unidades Organizativas (OU).
- Creación y administración de usuarios.
- Grupos de seguridad.
- Organización de usuarios y equipos.
- DNS integrado en el entorno de dominio.
- DHCP para asignación automática de direcciones IP.
- Unión de equipos Windows al dominio.

### 3. Administración de sistemas Windows

- Creación y vinculación de GPO.
- Aplicación de políticas a usuarios y equipos.
- Administración de cuentas y grupos.
- Gestión de permisos.
- Comprobación de la aplicación de directivas.
- Validación del acceso con distintas cuentas de usuario.

### 4. Servidor de archivos

- Despliegue de un servidor independiente, FILE01.
- Estructura de carpetas por departamentos.
- Creación de recursos compartidos SMB.
- Configuración de permisos NTFS.
- Control de acceso mediante grupos de Active Directory.
- Pruebas de autenticación y acceso.
- Validación de restricciones entre departamentos.

### 5. Firewall y conectividad de red

- Despliegue de FW01 con Ubuntu Server.
- Configuración de interfaces WAN y LAN.
- Configuración de direccionamiento estático en la LAN.
- Activación del reenvío IPv4.
- Configuración de routing entre interfaces.
- Configuración de NAT mediante `iptables`.
- Definición de políticas y reglas de tráfico.
- Registro de eventos del firewall.
- Persistencia de las reglas para mantener la configuración después de reiniciar.

El firewall proporciona una salida controlada a Internet para la red interna. La segmentación avanzada mediante VLAN y las reglas específicas entre diferentes redes quedan pendientes.

### 6. Monitorización con Zabbix

Se ha desplegado una plataforma centralizada de monitorización sobre ZABBIX01.

Componentes instalados:

- Zabbix Server 7.2.15.
- PostgreSQL.
- Apache y PHP para la interfaz web.
- Zabbix Agent 2.

La interfaz web está disponible en:

`http://192.168.10.30/zabbix`

Se han incorporado y validado los siguientes equipos:

- **DC01:** monitorización mediante agente, con métricas y gráficas disponibles.
- **FILE01:** monitorización mediante agente, con métricas y gráficas disponibles.

La plataforma permite consultar información de rendimiento de los servidores Windows.

Queda pendiente profundizar en el uso operativo de Zabbix: configuración de alertas, iniciadores, umbrales, análisis de problemas y monitorización de servicios.

La instalación del agente en FW01 se pospone para evitar dedicar tiempo a resolver el problema actual de acceso al repositorio y DNS, ya que no es imprescindible para continuar el proyecto.

### 7. Validación técnica

Se han realizado comprobaciones de:

- Resolución DNS.
- Conectividad entre equipos.
- Unión al dominio.
- Aplicación de GPO.
- Autenticación de usuarios.
- Acceso a recursos compartidos.
- Restricciones de permisos.
- Routing y conectividad a Internet.
- Reglas NAT y configuración del firewall.
- Funcionamiento de Zabbix.
- Recepción de métricas de DC01 y FILE01.

---

## Documentación del proyecto

El procedimiento detallado de instalación, configuración y pruebas se recoge en:

**[HOW-TO](docs/HOW-TO.md)**

Este documento debe mantenerse como referencia técnica de la implementación, con comandos, configuraciones, comprobaciones y capturas de pantalla.

El README describe la arquitectura y el estado general; el HOW-TO debe explicar cómo se ha construido cada componente.

---

## Próximas fases del proyecto

La infraestructura seguirá evolucionando por etapas. Cada bloque debe quedar configurado, probado y documentado antes de avanzar al siguiente.

### Fase 1. Consolidación de la infraestructura actual

Objetivo: dejar una base estable y bien documentada.

- Revisar el estado de las máquinas virtuales y sus configuraciones.
- Completar las capturas de pantalla pendientes.
- Revisar la documentación de FW01 y ZABBIX01.
- Confirmar que las reglas del firewall persisten tras un reinicio controlado.
- Verificar la conectividad entre servidores, cliente e Internet.
- Mantener actualizado el inventario de equipos y direcciones IP.

### Fase 2. Copias de seguridad y recuperación

Objetivo: aprender a recuperar servicios y datos ante errores o pérdidas.

- Diseñar una estrategia básica de copias de seguridad.
- Estudiar copias de seguridad de archivos y configuraciones.
- Realizar una copia de los datos de FILE01.
- Practicar la restauración de archivos y comprobar sus permisos.
- Documentar procedimientos de recuperación.
- Evaluar la recuperación de servicios críticos y configuraciones de los servidores.

La fase debe demostrar tanto la creación de copias como la recuperación efectiva de los datos.

### Fase 3. Uso práctico de Zabbix

Objetivo: pasar de tener Zabbix instalado a utilizarlo como herramienta de administración y diagnóstico.

- Interpretar métricas de CPU, memoria, disco y disponibilidad.
- Comprender la relación entre equipos, métricas, iniciadores y acciones.
- Configurar alertas ante situaciones controladas.
- Simular problemas de disponibilidad y comprobar su detección.
- Investigar el historial de métricas y eventos.
- Explorar la monitorización de servicios y conectividad.
- Documentar ejemplos de diagnóstico a partir de las alertas generadas.

Esta fase se realizará cuando retomemos el trabajo específico de monitorización.

### Fase 4. Segmentación y seguridad de red

Objetivo: evolucionar desde una LAN única hacia una red con diferentes zonas de seguridad.

- Estudiar VLAN y segmentación IP.
- Separar redes de servidores y clientes.
- Configurar routing entre redes.
- Definir reglas de firewall según las necesidades de comunicación.
- Aplicar el principio de mínimo privilegio al tráfico de red.
- Probar qué comunicaciones están permitidas y cuáles deben bloquearse.
- Documentar las reglas y los resultados de las pruebas.

La segmentación se introducirá progresivamente, preservando el funcionamiento de la infraestructura existente.

### Fase 5. Administración y automatización

Objetivo: reducir tareas manuales y mejorar la consistencia de la administración.

- Ampliar el uso de PowerShell para tareas de Windows.
- Practicar administración de usuarios, servicios y registros.
- Profundizar en la administración de Ubuntu Server.
- Trabajar con usuarios, permisos, servicios, paquetes y logs en Linux.
- Automatizar tareas repetitivas mediante scripts.
- Documentar y validar cada automatización.

### Fase 6. Conectividad remota y VPN

Objetivo: conectar redes de forma segura.

- Estudiar los fundamentos de VPN.
- Preparar una segunda red que represente una sede remota.
- Configurar una conexión entre las dos redes.
- Aplicar reglas de firewall y routing.
- Validar el acceso únicamente a los servicios necesarios.
- Comprobar la comunicación entre redes y documentar los resultados.

### Fase 7. Microsoft Azure e infraestructura híbrida

Objetivo: comprender cómo ampliar una infraestructura local mediante servicios cloud.

- Introducción a Microsoft Azure.
- Redes virtuales y máquinas virtuales.
- Microsoft Entra ID.
- Gestión de identidades y accesos.
- Conceptos de infraestructura híbrida.
- Estudio de las opciones de integración entre Active Directory local y servicios cloud.
- Pruebas de conectividad y control de acceso según el escenario elegido.

Esta fase se abordará cuando la infraestructura local esté suficientemente consolidada.

---

## Arquitectura objetivo

La evolución a largo plazo busca construir un entorno empresarial simulado con:

- Una sede principal y una sede remota.
- Firewall y conectividad segura entre redes.
- Segmentación mediante VLAN o redes independientes.
- Active Directory, DNS y DHCP.
- Servidores de archivos y clientes.
- Monitorización centralizada.
- Copias de seguridad y procedimientos de recuperación.
- Automatización de tareas de administración.
- Servicios de Azure y componentes de identidad cloud.
- Integración progresiva entre infraestructura local y cloud.

La arquitectura objetivo es una dirección de evolución, no un conjunto de componentes que deban desplegarse todos a la vez.

---

## Tecnologías utilizadas

### Implementadas

- Oracle VirtualBox.
- Windows Server 2025.
- Windows 10.
- Ubuntu Server 24.04 LTS.
- Active Directory Domain Services.
- DNS y DHCP.
- Group Policy.
- SMB y NTFS.
- IPv4.
- Routing.
- `iptables` y NAT.
- Zabbix Server 7.2.
- Zabbix Agent 2.
- PostgreSQL.
- Apache y PHP.
- PowerShell.

### Previstas o por profundizar

- Estrategias de backup y recuperación.
- Administración avanzada de Linux.
- Alertas e iniciadores de Zabbix.
- VLAN y segmentación de red.
- ACL y reglas de seguridad entre redes.
- VPN y conectividad entre sedes.
- Automatización de tareas.
- Microsoft Azure.
- Microsoft Entra ID.
- Infraestructura híbrida.

---

## Resultado actual

El proyecto dispone de una infraestructura empresarial simulada funcional que integra:

- Dominio y servicios de red.
- Administración centralizada de usuarios y equipos.
- Políticas de seguridad y acceso.
- Servidor de archivos independiente.
- Firewall Linux con routing y NAT.
- Plataforma centralizada de monitorización.
- Métricas y gráficas de dos servidores Windows.

El siguiente objetivo es consolidar y documentar la infraestructura existente, avanzar hacia las copias de seguridad y recuperación y, posteriormente, profundizar en el uso operativo de Zabbix, la segmentación de red, la automatización y la conectividad entre sedes.

La prioridad es **comprender, validar y documentar cada tecnología antes de añadir la siguiente**.
