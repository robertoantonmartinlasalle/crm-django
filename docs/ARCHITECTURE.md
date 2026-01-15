# Arquitectura del proyecto CRM Django

Este documento describe la arquitectura general del proyecto CRM Django,
así como las decisiones de diseño adoptadas en las fases iniciales del desarrollo.

---

## Autoría y participantes del proyecto

El desarrollo del proyecto CRM Django se realiza de forma colaborativa
por los siguientes participantes, con una distribución de responsabilidades
orientada a las distintas áreas del sistema:

- **Roberto Antón** — Desarrollo backend y definición de la arquitectura del sistema.
- **María Sugasaga** — Desarrollo del frontend de la aplicación e integración con la API.
- **Enrique Merinero** — Desarrollo backend y apoyo en el diseño técnico del sistema.

Este proyecto se desarrolla en el contexto de un **Trabajo de Fin de Grado (TFG)**,
siguiendo buenas prácticas de diseño, documentación y desarrollo software.


---

## Visión general

El proyecto CRM Django es una aplicación backend orientada a la gestión de
usuarios y datos empresariales, diseñada como una API REST que podrá ser
consumida por clientes externos.

El sistema está pensado para ser escalable, mantenible y fácilmente extensible.

---

## Tipo de arquitectura

La aplicación sigue una arquitectura **API-first**, donde el backend expone
una serie de endpoints HTTP que devuelven datos en formato JSON.

El frontend se considera desacoplado del backend y podrá desarrollarse de
forma independiente (por ejemplo, con React).

---

## Estructura del proyecto

El proyecto se organiza siguiendo la estructura estándar de Django:

- Un proyecto principal (`config`) encargado de la configuración global.
- Varias aplicaciones Django que encapsulan la lógica de negocio por dominio.
- Separación clara entre configuración, lógica y presentación de datos.

---

## Organización por aplicaciones

La funcionalidad se divide en aplicaciones Django independientes, cada una
responsable de un área concreta del sistema (por ejemplo, gestión de usuarios,
clientes, permisos, etc.).

Esta organización permite mejorar la mantenibilidad y facilitar la evolución
del proyecto.

---

## Separación de responsabilidades

Se adopta una separación clara de responsabilidades:

- **Models**: definición de datos y relaciones.
- **Serializers**: transformación de datos para la API.
- **Views / ViewSets**: gestión de peticiones HTTP.
- **Services**: lógica de negocio (cuando sea necesario).
- **Permissions**: control de acceso a los recursos.

---

## Decisiones de diseño

Las principales decisiones de diseño adoptadas son:

- Uso de Django como framework backend por su robustez y madurez.
- Uso de Django REST Framework para la creación de la API.
- Uso de PostgreSQL como base de datos relacional.
- Arquitectura desacoplada backend/frontend.

---

## Evolución prevista

En fases posteriores del proyecto se contempla la incorporación progresiva
de nuevas funcionalidades orientadas a mejorar la escalabilidad, seguridad
y experiencia de uso del sistema.

Entre estas mejoras se incluyen:

- Implementación de autenticación basada en tokens.
- Definición avanzada de roles y permisos.
- Soporte para arquitectura multiempresa (multitenant).
- Integración con un frontend web desacoplado.
- Incorporación de funcionalidades de comunicación por correo electrónico
  (SMTP), como notificaciones y recuperación de credenciales.
- Mejora de la validación de datos y control de acceso a los recursos.

Estas mejoras se abordarán de forma incremental conforme avance el desarrollo
del proyecto y en función de su evolución.

