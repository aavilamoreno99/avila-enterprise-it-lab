# HOW-TO — AVILA Enterprise IT Infrastructure

Este documento recoge la implementación técnica de la infraestructura actual.

El objetivo es mostrar cómo se ha construido el entorno, las decisiones tomadas durante el despliegue y las pruebas realizadas para comprobar que los servicios funcionan correctamente.

---

## 1. Diseño de la infraestructura

La infraestructura se ha planteado como una pequeña red empresarial aislada, utilizando máquinas virtuales independientes para separar las funciones principales.

La red interna utiliza:

```text
Red:        192.168.10.0/24
Gateway:    192.168.10.1
DNS:        192.168.10.10
Dominio:    avila-tech.local
```

### Equipos

| Equipo   | IP            | Función                      |
| -------- | ------------- | ---------------------------- |
| DC01     | 192.168.10.10 | Active Directory, DNS y DHCP |
| FILE01   | 192.168.10.20 | File Server                  |
| WIN10-01 | DHCP          | Cliente Windows              |

La separación entre `DC01` y `FILE01` permite mantener los servicios de dominio separados del almacenamiento y los recursos compartidos.

---

# 2. Red interna y VirtualBox

La primera parte de la implementación consiste en crear una red interna aislada para que las máquinas virtuales puedan comunicarse entre ellas sin depender de la red física del equipo.

En VirtualBox se crea la red interna:

```text
AVILA-LAB
```

Todas las máquinas que forman parte del laboratorio utilizan esta red.

**Configuración utilizada:**

```text
Red interna: AVILA-LAB
Red IP:      192.168.10.0/24
```

### Configuración inicial de DC01

`DC01` utiliza una dirección IP estática:

```text
IP:        192.168.10.10
Máscara:   255.255.255.0
Gateway:   192.168.10.1
DNS:       192.168.10.10
```

La configuración puede comprobarse mediante:

```powershell
ipconfig
```

![Configuración IP de DC01](../screenshots/02-dc01-ip.png)

### Comprobación de conectividad

Antes de desplegar los servicios se comprueba que los equipos pueden comunicarse dentro de la red.

```powershell
ping 192.168.10.10
```

![Prueba de conectividad](../screenshots/03-ping-dc01.png)

---

# 3. Servicios de infraestructura en DC01

`DC01` centraliza los principales servicios necesarios para que el entorno funcione como una infraestructura de dominio.

Se instalaron:

* Active Directory Domain Services.
* DNS.
* DHCP.

---

## 3.1 DHCP

El servidor DHCP proporciona automáticamente la configuración de red a los clientes.

Se creó el ámbito:

```text
AVILA-LAB
192.168.10.0/24
```

Con un rango destinado a los equipos cliente:

```text
192.168.10.100 - 192.168.10.200
```

![Ámbito DHCP](../screenshots/04-dhcp-scope.png)

Las opciones principales entregadas a los clientes son:

```text
Gateway:      192.168.10.1
DNS:          192.168.10.10
Dominio DNS:  avila-tech.local
```

![Opciones DHCP](../screenshots/05-dhcp-options.png)

De esta forma, los clientes reciben automáticamente la configuración necesaria para comunicarse con el dominio.

---

## 3.2 DNS

DNS es necesario para la resolución de nombres dentro del dominio.

Durante la configuración de Active Directory se creó la zona:

```text
avila-tech.local
```

![Zona DNS](../screenshots/06-dns-zone.png)

La resolución DNS se utiliza posteriormente para localizar servicios y equipos del dominio.

---

## 3.3 Active Directory Domain Services

Se instaló el rol **Active Directory Domain Services** y se promovió `DC01` como controlador de dominio.

El dominio utilizado es:

```text
avila-tech.local
```

![Active Directory](../screenshots/07-active-directory.png)

Una vez completada la instalación se realizó una comprobación del estado del controlador:

```powershell
dcdiag
```

![Comprobación DC01](../screenshots/08-dcdiag.png)

El objetivo de esta comprobación es verificar que los principales componentes del controlador de dominio funcionan correctamente.

---

# 4. Organización de Active Directory

Una vez disponible el dominio, se creó una estructura de OUs para organizar usuarios y equipos según su función.

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

![Usuarios y OUs](../screenshots/09-ad-users-ou.png)

### Usuarios

Se crearon las cuentas necesarias para simular diferentes departamentos:

```text
Alejandro → IT
Manuel    → Soporte
Belen     → Administracion
Valeriano → Usuarios
```

Estas cuentas se utilizan posteriormente para comprobar las políticas y permisos.

### Grupos

Se crearon grupos de seguridad asociados a los departamentos:

```text
GG-IT
GG-Soporte
GG-Administracion
```

La pertenencia de usuarios se utiliza posteriormente para controlar el acceso a los recursos.

![Grupo de IT](../screenshots/10-group-it.png)

---

# 5. Administración mediante GPO

Para comprobar la administración centralizada se creó una directiva de grupo:

```text
GPO-Soporte-Configuracion
```

La GPO está vinculada a:

```text
Usuarios
└── Soporte
```

La política utilizada como prueba impide que los usuarios de soporte puedan modificar el fondo de escritorio.

![GPO vinculada](../screenshots/11-gpo-linked.png)

Configuración aplicada:

![Configuración GPO](../screenshots/12-gpo-setting.png)

La intención de esta configuración no es únicamente modificar una opción de Windows, sino comprobar el funcionamiento de la administración centralizada mediante Active Directory.

---

# 6. Incorporación de un cliente al dominio

Con los servicios de infraestructura funcionando, se configuró `WIN10-01` como equipo cliente.

El equipo obtiene su configuración de red mediante DHCP y utiliza `DC01` como servidor DNS.

Posteriormente se incorporó al dominio:

```text
avila-tech.local
```

![Unión al dominio](../screenshots/13-domain-join.png)

Una vez reiniciado el equipo se realizó una prueba iniciando sesión con un usuario del dominio.

![Usuario de dominio](../screenshots/14-domain-user.png)

También se utilizaron comandos de comprobación:

```powershell
whoami
gpupdate /force
gpresult /r
```

Estas pruebas permiten comprobar la identidad utilizada y verificar que las políticas de grupo se aplican correctamente.

---

# 7. Servidor de archivos FILE01

Para separar los servicios de dominio del almacenamiento se creó un segundo servidor:

```text
FILE01
192.168.10.20
```

`FILE01` ejecuta Windows Server 2025 y está unido al dominio:

```text
avila-tech.local
```

![Configuración IP de FILE01](../screenshots/15-file01-ip.png)

![FILE01 unido al dominio](../screenshots/16-file01-domain.png)

Se instaló el rol **File Server** para proporcionar almacenamiento compartido a los diferentes departamentos.

---

## 7.1 Estructura de carpetas

En `FILE01` se creó:

```text
C:\Departamentos
│
├── IT
├── Soporte
└── Administracion
```

![Estructura de carpetas](../screenshots/17-file-server-folders.png)

La separación permite aplicar permisos diferentes según el departamento.

---

## 7.2 Permisos NTFS

Los permisos se gestionan mediante los grupos de seguridad de Active Directory.

```text
IT
└── GG-IT → Modify

Soporte
└── GG-Soporte → Modify

Administracion
└── GG-Administracion → Modify
```

Se deshabilitó la herencia en las carpetas departamentales para evitar que permisos generales del directorio superior proporcionasen acceso no deseado.

Se conservaron las entradas necesarias para el funcionamiento del sistema y la administración del servidor.

![Permisos NTFS](../screenshots/18-file-server-permissions.png)

El control de acceso se basa en la pertenencia a grupos, evitando asignar permisos individualmente a cada usuario.

---

## 7.3 Recursos compartidos SMB

Las carpetas se publicaron mediante SMB:

```text
\\FILE01\IT
\\FILE01\Soporte
\\FILE01\Administracion
```

![Recursos SMB](../screenshots/19-file-server-shares.png)

Los permisos efectivos se controlan principalmente mediante NTFS, mientras que SMB proporciona el acceso a los recursos compartidos a través de la red.

---

# 8. Validación del acceso

La configuración se validó desde `WIN10-01` utilizando diferentes usuarios del dominio.

### Alejandro

Pertenece a:

```text
GG-IT
```

Resultado:

```text
IT              ✓
Soporte         ✗
Administracion  ✗
```

### Manuel

Pertenece a:

```text
GG-Soporte
```

Resultado:

```text
IT              ✗
Soporte         ✓
Administracion  ✗
```

### Belen

Pertenece a:

```text
GG-Administracion
```

Resultado:

```text
IT              ✗
Soporte         ✗
Administracion  ✓
```

Estas pruebas permiten comprobar que la estructura de grupos de Active Directory se está utilizando correctamente para controlar el acceso a los recursos del servidor.

---

# 9. Resultado actual

La infraestructura construida hasta este punto dispone de:

```text
                         AVILA-LAB
                      192.168.10.0/24
                             |
          +------------------+------------------+
          |                  |                  |
        DC01               FILE01           WIN10-01
     .10 /24             .20 /24               DHCP
          |                  |                  |
      AD / DNS            SMB / NTFS        Windows
        DHCP                 |                  |
          |                  +--------+---------+
          |                           |
          +------- avila-tech.local ---+
```

Actualmente se ha validado:

* Conectividad de red.
* DHCP.
* Resolución DNS.
* Funcionamiento de Active Directory.
* Organización mediante OUs.
* Usuarios y grupos.
* Aplicación de GPO.
* Unión de equipos al dominio.
* Autenticación de usuarios.
* Recursos SMB.
* Permisos NTFS.
* Control de acceso por departamento.

Esta infraestructura constituye la base sobre la que continuará evolucionando el proyecto.

Las siguientes ampliaciones se incorporarán sobre esta infraestructura, añadiendo nuevas necesidades de red, sistemas, seguridad, monitorización, conectividad entre sedes y servicios cloud.
