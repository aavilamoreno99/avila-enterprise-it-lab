# 02 - DHCP, DNS y Active Directory

## Descripción

En esta fase se han desplegado los principales servicios de infraestructura de Windows Server para proporcionar direccionamiento IP automático, resolución DNS y servicios de directorio.

El servidor `DC01` actúa como:

· Servidor DHCP
· Servidor DNS
· Controlador de dominio
· Servidor de Active Directory Domain Services (AD DS)

## Configuración de DC01

| Parámetro    | Valor              |
| ------------ | ------------------ |
| Nombre       | `DC01`             |
| Dirección IP | `192.168.10.10`    |
| Máscara      | `255.255.255.0`    |
| DNS          | `192.168.10.10`    |
| Dominio      | `avila-tech.local` |

Se utiliza una dirección IP estática para DC01 debido a que los servicios de infraestructura, especialmente DNS y Active Directory, necesitan una dirección conocida y estable dentro de la red.

---
![Configuración IP de DC01](../screenshots/02-dc01-ip.png)

## DHCP

Se instaló el rol **DHCP Server** en Windows Server.

Se creó un ámbito denominado:

```text
AVILA-LAB
```

correspondiente a la red:

```text
192.168.10.0/24
```

### Rango de direcciones

El rango configurado para los clientes DHCP es:

```text
192.168.10.100 - 192.168.10.200
```

Las direcciones inferiores se mantienen disponibles para servidores y dispositivos de infraestructura con direcciones estáticas.

![Configuración del ámbito DHCP AVILA-LAB](../screenshots/04-dhcp-scope.png)

### Opciones DHCP

Se configuraron las siguientes opciones:

| Opción           | Configuración      |
| ---------------- | ------------------ |
| Router / Gateway | `192.168.10.1`     |
| Servidor DNS     | `192.168.10.10`    |
| Dominio DNS      | `avila-tech.local` |

El gateway `192.168.10.1` corresponde al firewall/router que se incorporará posteriormente al laboratorio mediante pfSense.

Por este motivo, durante esta fase los clientes pueden comunicarse dentro de la red del laboratorio, pero todavía no disponen de salida a Internet a través de un gateway funcional.

![Opciones del ámbito DHCP](../screenshots/05-dhcp-options.png)

### Prueba DHCP

WIN10-01 se configuró para obtener automáticamente su dirección IP y configuración DNS.

La configuración se comprobó mediante:

```cmd
ipconfig /all
```

El cliente obtuvo correctamente una dirección dentro del rango DHCP y utilizó `192.168.10.10` como servidor DNS.

---

## DNS

Durante la instalación de Active Directory se desplegó también el servicio DNS.

La zona DNS principal del laboratorio es:

```text
avila-tech.local
```
![Zona DNS del dominio avila-tech.local](../screenshots/06-dns-zone.png)

DC01 actúa como servidor DNS para los equipos del dominio.

Se realizaron pruebas de resolución mediante:

```cmd
nslookup avila-tech.local
```

y:

```cmd
nslookup DC01.avila-tech.local
```

Las consultas fueron resueltas utilizando el servidor DNS `192.168.10.10`.

El correcto funcionamiento de DNS es fundamental para Active Directory, ya que los equipos del dominio utilizan registros DNS para localizar los servicios y controladores de dominio.

---

## Active Directory Domain Services

Se instaló el rol:

**Active Directory Domain Services (AD DS)**

Posteriormente, DC01 fue promocionado como controlador de dominio creando un nuevo bosque.

### Dominio

```text
avila-tech.local
```

![Estructura de Active Directory](../screenshots/07-active-directory.png)

### Configuración principal

* Nuevo bosque: `avila-tech.local`
* Controlador de dominio: `DC01`
* Servidor DNS: habilitado
* Catálogo global: habilitado
* Controlador de dominio de solo lectura: deshabilitado

Después de la promoción, el servidor se reinició y pasó a funcionar como controlador de dominio.

---

## Comprobaciones posteriores

Después de la promoción se verificó el correcto funcionamiento de los servicios principales.

### Servicios

Se comprobaron:

· DNS
· Active Directory Domain Services
· Netlogon
· DHCP Server

mediante:

```powershell
Get-Service DNS,NTDS,Netlogon,DHCPServer
```

Los servicios aparecieron en estado `Running`.

### Recursos compartidos de Active Directory

Se comprobó la existencia de los recursos:

```text
NETLOGON
SYSVOL
```

mediante:

```cmd
net share
```

### Diagnóstico de Active Directory

Se ejecutó:

```cmd
dcdiag
```

para comprobar el estado del controlador de dominio.

Las comprobaciones se completaron correctamente sin errores críticos.

---

## Resultado

DC01 quedó configurado como la infraestructura principal del laboratorio, proporcionando:

```text
                 DC01
           192.168.10.10
                  │
       ┌──────────┼──────────┐
       │          │          │
      DHCP       DNS       AD DS
       │          │          │
       └──────────┼──────────┘
                  │
             WIN10-01
```

Con esta infraestructura preparada, el laboratorio dispone de una base funcional para administrar equipos y usuarios mediante Active Directory y aplicar posteriormente políticas de grupo, permisos y servicios de red.

