# 05 - Unión de Windows al dominio

## Descripción

En esta fase se ha unido el equipo cliente `WIN10-01` al dominio `avila-tech.local`.

El objetivo es comprobar la comunicación entre el cliente Windows y el controlador de dominio, así como validar el inicio de sesión utilizando una cuenta de Active Directory.

---

## Configuración del cliente

WIN10-01 obtiene su configuración de red mediante DHCP.

La configuración proporcionada por el servidor incluye:

* Dirección IP dentro del rango DHCP.
* Servidor DNS: `192.168.10.10`
* Dominio DNS: `avila-tech.local`

DC01 proporciona los servicios DHCP y DNS necesarios para que el cliente pueda localizar el controlador de dominio.

---

## Unión al dominio

WIN10-01 se incorporó al dominio:

```text id="o3n9n6"
avila-tech.local
```

La pertenencia al dominio se comprobó desde las propiedades del sistema del equipo cliente.

![WIN10-01 unido al dominio avila-tech.local](../screenshots/13-domain-join.png)

---

## Inicio de sesión con usuario de dominio

Una vez unido el equipo al dominio, se realizó el inicio de sesión utilizando una cuenta creada en Active Directory:

```text id="q4s8bc"
avila-tech\Manuel
```

Para comprobar que la sesión estaba utilizando correctamente la cuenta del dominio se ejecutó:

```cmd id="v4x5cg"
whoami
```

El resultado confirmó:

```text id="l6b3w8"
avila-tech\manuel
```

![Comprobación del usuario de dominio mediante whoami](../screenshots/14-domain-user.png)

---

## Comprobación de directivas

Después de iniciar sesión con el usuario de dominio se actualizaban las directivas mediante:

```cmd id="4x6v9d"
gpupdate /force
```

Posteriormente se utilizó:

```cmd id="j9c2zr"
gpresult /r
```

para comprobar las directivas aplicadas al usuario y al equipo.

Esto permitió verificar la aplicación de la GPO configurada anteriormente para el departamento de Soporte.

---

## Resultado

WIN10-01 se ha unido correctamente al dominio `avila-tech.local`.

Se ha comprobado:

* Obtención de configuración de red mediante DHCP.
* Resolución DNS mediante DC01.
* Unión del equipo al dominio.
* Inicio de sesión con un usuario de Active Directory.
* Aplicación de directivas de grupo.

Con esta fase queda completada la configuración básica del entorno Windows y Active Directory.

El siguiente objetivo del laboratorio será incorporar un servidor de archivos `FILE01` y configurar recursos compartidos, permisos NTFS y permisos de acceso mediante grupos de Active Directory.

