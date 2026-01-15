# Roadmap del proyecto CRM Django

Este documento define el plan de desarrollo del proyecto CRM Django,
estructurado en fases y basado en un enfoque incremental. El objetivo del
roadmap es establecer un orden coherente de implementación que permita
validar el sistema de forma progresiva, reduciendo la necesidad de
refactorizaciones y garantizando la estabilidad del proyecto a lo largo
de su evolución.

El roadmap ha sido diseñado teniendo en cuenta el contexto académico del
proyecto como Trabajo de Fin de Grado (TFG), priorizando la claridad en las
decisiones, la mantenibilidad del código y la coherencia de la arquitectura
propuesta.

---

## Enfoque general del desarrollo

El desarrollo del proyecto se plantea de forma incremental, dividiendo el
trabajo en fases bien definidas. Cada fase tiene un objetivo claro y aporta
valor por sí misma, permitiendo probar y validar el sistema antes de avanzar
a la siguiente etapa.

Este enfoque facilita:
- La detección temprana de errores de diseño.
- La validación progresiva del dominio.
- La incorporación controlada de complejidad.
- La justificación clara del alcance del proyecto.

---

## Fase 1 — Base del proyecto

### Objetivo

Establecer una base sólida y estable del proyecto sobre la que se apoyará
todo el desarrollo posterior.

### Alcance

Durante esta fase se abordarán los siguientes aspectos:

- Creación del proyecto Django y estructura inicial (`config`).
- Configuración del entorno de desarrollo.
- Conexión a la base de datos PostgreSQL.
- Definición de la configuración global del sistema.
- Creación de la aplicación de gestión de usuarios (`accounts`).
- Definición de un modelo de usuario personalizado.
- Definición de la entidad Empresa.
- Configuración del panel de administración.
- Creación y aplicación de migraciones iniciales.
- Verificación del correcto arranque del sistema.

### Resultado esperado

Al finalizar esta fase, el proyecto debe ser arrancable, estable y permitir
la gestión básica de usuarios y empresas desde el panel de administración.

---

## Fase 2 — Núcleo CRM

### Objetivo

Implementar el núcleo funcional del CRM, reflejando el dominio definido en la
documentación de diseño de datos.

### Alcance

En esta fase se desarrollarán los elementos principales del CRM:

- Creación de la aplicación principal del CRM.
- Definición de las entidades Cliente, Contacto e Interacción.
- Implementación de las relaciones entre entidades.
- Integración de las entidades con la empresa correspondiente.
- Configuración del panel de administración para la gestión de datos.
- Creación de endpoints básicos de la API (CRUD).
- Validación funcional del dominio mediante pruebas manuales.

### Resultado esperado

Al finalizar esta fase, el sistema debe permitir gestionar clientes,
contactos e interacciones de forma coherente, reflejando el modelo de datos
definido y validando el núcleo del CRM.

---

## Fase 3 — Seguridad y permisos

### Objetivo

Incorporar mecanismos de seguridad y control de acceso que garanticen un uso
seguro y controlado del sistema.

### Alcance

Durante esta fase se abordarán los siguientes aspectos:

- Implementación de mecanismos de autenticación.
- Definición de permisos y roles de usuario.
- Restricción de acceso a los recursos según el usuario y la empresa.
- Protección de los endpoints de la API.
- Validación del aislamiento de datos entre empresas.

### Resultado esperado

Al finalizar esta fase, el sistema debe asegurar que los usuarios solo puedan
acceder a los recursos que les corresponden, reforzando la seguridad y la
integridad de los datos.

---

## Fase 4 — Extensiones y mejoras (opcional)

### Objetivo

Incorporar funcionalidades avanzadas y mejoras adicionales que aporten valor
al sistema, sin comprometer el alcance principal del proyecto.

### Alcance

Esta fase contempla de forma opcional:

- Implementación avanzada del enfoque multitenant.
- Incorporación de funcionalidades de correo electrónico (SMTP), como
  notificaciones o recuperación de credenciales.
- Registro de auditoría y trazabilidad de acciones.
- Optimización de la estructura y consultas de datos.
- Ajustes y mejoras según la evolución del proyecto.

### Resultado esperado

Al finalizar esta fase, el proyecto contará con funcionalidades adicionales
orientadas a mejorar su escalabilidad, seguridad y experiencia de uso.

---

## Fase 5 — Integración con frontend

### Objetivo

Validar el consumo del backend mediante un cliente frontend desacoplado,
desarrollado en paralelo al backend.

### Alcance

Durante esta fase se abordarán los siguientes aspectos:

- Coordinación entre el desarrollo backend y frontend.
- Diseño y ajuste de endpoints orientados al consumo desde interfaz web.
- Configuración de CORS y aspectos de comunicación entre cliente y API.
- Pruebas de integración entre frontend y backend.
- Ajustes derivados del uso real de la API.

El desarrollo del frontend se llevará a cabo de forma paralela por un 
miembro del equipo, mientras que esta fase se orienta a asegurar una
integración correcta y coherente entre el cliente frontend y el backend
del sistema.

### Resultado esperado

Al finalizar esta fase, el backend deberá ser consumible de forma efectiva
por un cliente frontend, validando la arquitectura API-first del proyecto.

---

## Consideraciones finales

El roadmap define una guía de desarrollo flexible. Las fases podrán ajustarse
en función del progreso del proyecto y de las necesidades detectadas durante
su desarrollo.

El enfoque incremental permite justificar de forma clara el alcance del
proyecto y las decisiones adoptadas en cada etapa.
