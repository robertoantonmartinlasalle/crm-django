# Diseño de la base de datos

Este documento describe el diseño conceptual de la base de datos del proyecto
CRM Django. El objetivo es definir las entidades principales del dominio, sus
relaciones y los criterios de diseño adoptados, sin entrar todavía en la
implementación concreta mediante modelos Django.

El diseño se plantea desde el inicio para ser escalable, mantenible y compatible
con una arquitectura multiempresa (multitenant).

---

## Objetivo del diseño de datos

La base de datos del CRM tiene como finalidad almacenar y gestionar información
relacionada con la actividad comercial y organizativa de distintas empresas,
permitiendo:

- Gestionar usuarios que acceden al sistema.
- Gestionar clientes y sus relaciones.
- Registrar interacciones y actividades a lo largo del tiempo.
- Garantizar el aislamiento de datos entre empresas.

El diseño prioriza la claridad del dominio y la separación de responsabilidades
entre las distintas entidades.

---

## Enfoque multiempresa (multitenant)

El sistema está diseñado para soportar un enfoque multiempresa, en el que una
misma instancia de la aplicación pueda ser utilizada por múltiples organizaciones.

Cada empresa actúa como un **tenant**, y todos los datos relevantes del sistema
deben estar asociados a una empresa concreta. Los usuarios solo podrán acceder
a los datos pertenecientes a su propia empresa.

Aunque la implementación técnica del multitenant podrá abordarse en fases
posteriores, el diseño de la base de datos se define desde el inicio para
permitir este enfoque sin refactorizaciones futuras.

---

## Relación Usuario–Empresa

En esta fase del proyecto, se adopta un modelo en el que cada usuario
pertenece a una única empresa.

Esta decisión simplifica el aislamiento de datos, reduce la complejidad
del dominio y facilita la implementación de controles de acceso seguros.

La posibilidad de que un usuario pueda pertenecer a múltiples empresas
se considera fuera del alcance actual y podría evaluarse en fases futuras
si el proyecto lo requiere.

---

## Entidades principales del dominio

A continuación se describen las entidades conceptuales principales del sistema.
Estas entidades representan el dominio del problema y no deben interpretarse
todavía como modelos definitivos de implementación.

---

### Empresa

La entidad **Empresa** representa a una organización que utiliza el CRM y actúa
como raíz del dominio.

Responsabilidades conceptuales:
- Identificar de forma única a cada organización.
- Agrupar usuarios, clientes e información asociada.
- Servir como base para el aislamiento de datos (multitenant).

Ninguna entidad de negocio relevante debe existir sin estar asociada a una empresa.

---

### Usuario

La entidad **Usuario** representa a una persona que accede al sistema.

Características conceptuales:
- Un usuario pertenece a una única empresa.
- Los usuarios se autentican para acceder a la aplicación.
- Un usuario puede tener distintos roles o permisos dentro de su empresa.

Los usuarios representan a los operadores del sistema y no deben confundirse
con los clientes gestionados por el CRM.

---

### Cliente

La entidad **Cliente** representa a una persona física o a una organización
gestionada por una empresa dentro del CRM.

Decisiones de diseño:
- Un cliente puede representar tanto a una **persona** como a una **empresa**.
- El tipo de cliente se diferenciará mediante una categoría o tipo.
- Un cliente pertenece siempre a una empresa.
- Los clientes no tienen acceso directo al sistema.

Esta entidad constituye el núcleo del CRM y representa la relación que la empresa
mantiene con terceros.

---

### Contacto

La entidad **Contacto** representa a una persona asociada a un cliente,
generalmente cuando el cliente es una organización.

Características conceptuales:
- Un cliente puede tener uno o varios contactos.
- Un contacto pertenece a un cliente y a una empresa.
- Los contactos permiten modelar relaciones personales dentro de un cliente
  empresarial.

Esta entidad evita sobrecargar la información del cliente y permite una
representación más realista de las relaciones comerciales.

---

### Interacción / Actividad

La entidad **Interacción** (o Actividad) representa cualquier acción o
comunicación realizada en el contexto de un cliente.

Ejemplos conceptuales:
- Llamadas
- Correos electrónicos
- Reuniones
- Tareas
- Notas de seguimiento

Decisiones de diseño:
- Toda interacción pertenece obligatoriamente a un cliente.
- Una interacción puede estar asociada opcionalmente a un contacto concreto.
- Las interacciones permiten construir un historial temporal de la relación
  con el cliente.

Esta entidad es clave para dotar al CRM de contexto y trazabilidad.

---

### Roles y permisos (conceptual)

El sistema contempla la existencia de mecanismos de control de acceso basados
en roles y permisos.

Inicialmente, este control podrá apoyarse en los mecanismos estándar del
framework, con la posibilidad de ampliarlo o personalizarlo en fases posteriores
del proyecto.

---

## Relaciones entre entidades

A nivel conceptual, las relaciones principales del sistema son:

- Una **Empresa** puede tener múltiples **Usuarios**.
- Una **Empresa** puede gestionar múltiples **Clientes**.
- Un **Cliente** pertenece a una única **Empresa**.
- Un **Cliente** puede tener múltiples **Contactos**.
- Una **Interacción** pertenece a un **Cliente**.
- Una **Interacción** puede estar asociada a un **Contacto** de forma opcional.

Estas relaciones refuerzan el aislamiento de datos entre empresas y permiten una
gestión flexible del dominio.

---

## Aislamiento de datos

El aislamiento de datos entre empresas es un principio fundamental del diseño.

Todas las entidades relevantes del sistema deben estar asociadas a una empresa,
de forma que cualquier consulta o acceso a datos pueda filtrarse por el tenant
correspondiente.

Este enfoque reduce el riesgo de accesos indebidos y simplifica la aplicación
de políticas de seguridad.

---

## Decisiones de diseño adoptadas

En esta fase del proyecto se adoptan las siguientes decisiones clave:

- Uso de una base de datos relacional (PostgreSQL).
- Diseño orientado a multitenant desde el inicio.
- Relación uno a muchos entre empresa y usuarios.
- Separación clara entre usuarios del sistema y clientes gestionados.
- Diferenciación entre cliente y contacto.
- Registro explícito de interacciones como historial de negocio.

---

## Evolución prevista

En fases posteriores del proyecto se contempla:

- Ampliar las entidades relacionadas con el CRM (estados, etiquetas, notas).
- Incorporar auditoría de acciones y trazabilidad.
- Añadir configuraciones específicas por empresa.
- Optimizar la estructura de datos según el crecimiento del sistema.

Estas decisiones se abordarán de forma incremental conforme avance el desarrollo
y se validen las necesidades reales del dominio.
