# 03 - Active Directory

## Descripción

En esta fase se ha organizado el dominio `avila-tech.local` mediante unidades organizativas (OU), usuarios y grupos de seguridad.

El objetivo es crear una estructura que permita administrar los usuarios y equipos de forma organizada y aplicar posteriormente permisos y políticas de grupo según el departamento o función.

---

## Estructura de unidades organizativas

La estructura creada en Active Directory es:

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

Esta organización permite separar los usuarios y equipos según su función dentro del entorno.

La separación mediante OUs también facilita la aplicación de políticas de grupo específicas a determinados usuarios o equipos.

---

## Usuarios

Se han creado los siguientes usuarios para realizar las diferentes pruebas del laboratorio:

| Usuario   | OU                        | Función            |
| --------- | ------------------------- | ------------------ |
| Alejandro | `Usuarios\IT`             | IT                 |
| Manuel    | `Usuarios\Soporte`        | Soporte            |
| Belen     | `Usuarios\Administracion` | Administración     |
| Valeriano | `Usuarios`                | Usuario de pruebas |

Los usuarios se han trasladado desde el contenedor predeterminado `Users` a las OUs correspondientes para mantener una estructura organizada.

![Usuarios organizados en sus unidades organizativas](../screenshots/09-ad-users-ou.png)

---

## Grupos de seguridad

Se han creado grupos de seguridad globales para facilitar la gestión de permisos:

| Grupo               | Miembro   |
| ------------------- | --------- |
| `GG-IT`             | Alejandro |
| `GG-Soporte`        | Manuel    |
| `GG-Administracion` | Belen     |

El uso de grupos permite asignar permisos a un conjunto de usuarios sin tener que configurar los permisos individualmente para cada cuenta.

Por ejemplo, posteriormente se podrá utilizar `GG-IT` para proporcionar acceso a determinados recursos compartidos o servicios destinados al departamento de IT.

![Miembros del grupo de seguridad GG-IT](../screenshots/10-group-it.png)

---

## Organización de equipos

También se han creado unidades organizativas destinadas a los equipos:

```text
Equipos
├── Windows
└── Servidores
```

La OU `Windows` está destinada a los equipos cliente del laboratorio, mientras que `Servidores` se utilizará para organizar los servidores que se incorporen posteriormente.

Esta separación permitirá aplicar diferentes políticas de grupo dependiendo del tipo de equipo.

---

## Comprobaciones

Se verificó desde **Usuarios y equipos de Active Directory** que:

· Las unidades organizativas existen correctamente.
· Los usuarios se encuentran en las OUs correspondientes.
· Los grupos de seguridad están creados.
· Los usuarios pertenecen a los grupos correspondientes.
· La estructura del dominio se mantiene organizada.

## Resultado

Active Directory dispone ahora de una estructura básica de usuarios, grupos y equipos preparada para continuar con la administración del entorno.

Esta estructura se utilizará en las siguientes fases para implementar políticas de grupo, permisos sobre recursos compartidos y administración de equipos Windows.

