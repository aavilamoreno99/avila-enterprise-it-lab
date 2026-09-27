# 04 - Group Policy

## Descripción

En esta fase se ha creado y aplicado una directiva de grupo (GPO) para controlar determinadas configuraciones de los usuarios del departamento de Soporte.

Las directivas de grupo permiten centralizar la configuración de usuarios y equipos dentro de un dominio Active Directory.

---

## GPO creada

Se ha creado la siguiente directiva:

```text id="yq1j83"
GPO-Soporte-Configuracion
```

La GPO está vinculada a la OU:

```text id="mb5t7n"
Usuarios
└── Soporte
```

De esta forma, la configuración se aplica a los usuarios que se encuentran dentro de esta unidad organizativa.

![GPO vinculada a la OU Soporte](../screenshots/11-gpo-linked.png)

---

## Configuración aplicada

Dentro de la GPO se ha configurado la siguiente directiva:

**Impedir cambiar el fondo de escritorio**

Ruta:

```text id="x9k5qz"
Configuración de usuario
└── Directivas
    └── Plantillas administrativas
        └── Panel de control
            └── Personalización
                └── Impedir cambiar el fondo de escritorio
```

La directiva se encuentra configurada como:

```text id="yjx3l5"
Habilitada
```

![Configuración de la directiva de grupo](../screenshots/12-gpo-setting.png)

---

## Aplicación de la política

Para comprobar la aplicación de la GPO en el equipo cliente se utilizó:

```cmd id="0x7c3m"
gpupdate /force
```

Este comando fuerza la actualización de las directivas de grupo en el equipo.

Posteriormente se puede comprobar el resultado mediante:

```cmd id="8xv4we"
gpresult /r
```

La comprobación permite verificar las directivas aplicadas al usuario y al equipo.

---

## Resultado

La GPO `GPO-Soporte-Configuracion` ha sido creada y vinculada correctamente a la OU `Usuarios\Soporte`.

La política configurada permite demostrar el funcionamiento de la administración centralizada mediante Group Policy dentro del dominio `avila-tech.local`.

Esta estructura servirá posteriormente para implementar políticas adicionales relacionadas con seguridad, restricciones de usuario, configuración de Windows y administración de equipos.

