# Modelo de permisos y control de acceso (Permissions Model)

Este documento define el modelo conceptual de permisos y control de acceso
del proyecto **CRM Django**.

Su objetivo es establecer cómo se gobierna el acceso a los recursos y
operaciones del sistema, garantizando seguridad, coherencia funcional
y alineación con el modelo multiempresa (multitenant).

Este documento no describe una implementación técnica concreta, sino
las reglas, principios y comportamientos esperados del sistema en
materia de autorización.

---

## Objetivo del modelo de permisos

Los objetivos principales de este modelo son:

- Definir cómo se controla el acceso a los recursos del sistema.
- Establecer qué acciones puede realizar un usuario autenticado.
- Garantizar el aislamiento de datos entre empresas.
- Evitar accesos indebidos o inconsistencias de seguridad.
- Servir como referencia para la implementación de permisos en fases posteriores.

---

## Relación con la autenticación

El modelo de permisos se aplica siempre **después de la autenticación**.

- La autenticación determina quién es el usuario.
- La autorización determina qué puede hacer ese usuario.

Un usuario no autenticado nunca puede acceder a recursos protegidos,
independientemente de los permisos definidos.

---

## Principios generales de autorización

El sistema de permisos se rige por los siguientes principios:

- Todo acceso a recursos está restringido por defecto.
- Los permisos se evalúan siempre en el backend.
- El frontend no decide ni valida permisos.
- Un usuario solo puede acceder a datos de su propia empresa.
- La ausencia de permiso implica denegación de acceso.

---

## Modelo de permisos adoptado

El sistema adopta un modelo de permisos **basado en roles**, complementado
con reglas de negocio y el contexto de empresa.

A nivel conceptual:

- Cada usuario pertenece a una única empresa.
- Cada usuario puede tener uno o varios roles dentro de su empresa.
- Los roles agrupan permisos funcionales.
- Los permisos controlan acciones sobre recursos.

---

## Roles (conceptual)

En una primera fase, el sistema contempla roles conceptuales como:

- **Administrador**
- **Usuario estándar**
- **Usuario de solo lectura** (opcional)

Estos roles representan perfiles funcionales y no una implementación cerrada.
La definición exacta de roles podrá evolucionar según las necesidades del proyecto.

---

## Permisos (conceptual)

Los permisos se definen como **acciones permitidas sobre recursos**.

Ejemplos conceptuales de permisos:

- Ver clientes
- Crear clientes
- Editar clientes
- Eliminar clientes
- Registrar interacciones
- Gestionar contactos
- Gestionar usuarios
- Administrar empresa

Los permisos no se asignan directamente a recursos externos,
sino a operaciones internas del sistema.

---

## Control de acceso por recurso

El acceso a los recursos del sistema sigue estas reglas generales:

### Usuarios
- Acceso restringido según rol.
- Gestión de usuarios limitada a perfiles autorizados.

### Empresas
- Acceso limitado a la empresa del usuario autenticado.
- Operaciones sensibles (crear o eliminar) restringidas.

### Clientes
- Solo accesibles si pertenecen a la empresa del usuario.
- Acciones permitidas según permisos asignados.

### Contactos
- Acceso condicionado al cliente asociado.
- Sujeto a permisos de gestión de clientes.

### Interacciones
- Acceso limitado al contexto del cliente.
- Registro y edición según permisos.

---

## Permisos y multitenant

En un sistema multiempresa:

- El contexto de empresa se deriva del usuario autenticado.
- El frontend no envía identificadores de empresa.
- El backend filtra y valida todos los accesos por empresa.
- El acceso a recursos de otra empresa se considera un **error de autorización**.

Este enfoque refuerza el aislamiento de datos y la seguridad del sistema.

---

## Relación con el manejo de errores

Cuando un usuario no dispone de permisos suficientes:

- El backend debe responder con **403 Forbidden**.
- No se debe indicar si el recurso existe en otra empresa.
- El mensaje de error debe ser genérico y seguro.

El formato del error seguirá el contrato definido en `ERROR_HANDLING.md`.

---

## Evolución prevista

En fases posteriores del proyecto se contempla:

- Definición detallada de roles por empresa.
- Asignación dinámica de permisos.
- Permisos más granulares por operación.
- Integración con el sistema de permisos del framework.
- Auditoría de acciones según permisos.

Estas mejoras se abordarán de forma incremental conforme avance
el desarrollo del proyecto.

---

## Consideraciones finales

Este modelo de permisos establece una base sólida y extensible
para el control de acceso del sistema.

Su diseño prioriza la seguridad, la claridad conceptual y la
alineación con el enfoque multiempresa del proyecto CRM Django,
permitiendo evolucionar el sistema sin refactorizaciones críticas.
