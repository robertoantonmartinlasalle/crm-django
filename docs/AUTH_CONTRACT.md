# Contrato de Autenticación y Autorización (AUTH Contract)

Este documento define el contrato conceptual de autenticación y
autorización del proyecto CRM Django.

Su objetivo es establecer las reglas, flujos y responsabilidades
relacionadas con el acceso al sistema, garantizando coherencia entre
backend, frontend y dominio funcional.

Este documento no describe una implementación técnica concreta, sino
los principios y comportamientos esperados del sistema en materia de
seguridad y control de acceso.

---

## Objetivo del AUTH Contract

Los objetivos principales de este contrato son:

- Definir cómo se autentican los usuarios en el sistema.
- Establecer qué significa que un usuario esté autenticado.
- Determinar qué recursos requieren autenticación.
- Alinear la autenticación con el modelo multiempresa (multitenant).
- Servir como referencia para la implementación de seguridad en fases posteriores.

---

## Enfoque general

El sistema sigue un modelo de autenticación **basado en sesión lógica
mediante tokens**, donde:

- El usuario se autentica una única vez mediante credenciales válidas.
- El backend devuelve un identificador de sesión o token.
- El frontend utiliza dicho token para autenticar las peticiones posteriores.
- El backend valida la autenticación en cada petición protegida.

El mecanismo concreto de tokens (JWT u otros) se definirá en la fase de
implementación correspondiente.

---

## Autenticación de usuarios

### Credenciales

Para autenticarse, el usuario deberá proporcionar:

- Identificador de usuario o correo electrónico.
- Contraseña.

Estas credenciales se validan exclusivamente en el backend.

---

### Proceso de autenticación (flujo conceptual)

1. El usuario envía sus credenciales al endpoint de autenticación.
2. El backend valida las credenciales.
3. Si las credenciales son válidas:
   - Se considera al usuario autenticado.
   - Se genera un token de autenticación.
4. El token se devuelve al frontend.
5. El frontend incluye el token en las peticiones posteriores a la API.

Si las credenciales no son válidas, el sistema devuelve un error de autenticación.

---

## Estado de usuario autenticado

Un usuario se considera **autenticado** cuando:

- Ha proporcionado credenciales válidas.
- Dispone de un token de autenticación válido.
- El token se incluye correctamente en las peticiones a la API.

El backend es el único responsable de validar dicho estado.

---

## Endpoints públicos y protegidos

### Endpoints públicos

Los siguientes endpoints no requieren autenticación:

- Autenticación (login).
- (Futuro) Recuperación de contraseña.

Estos endpoints no exponen información sensible del sistema.

---

### Endpoints protegidos

Todos los endpoints relacionados con:

- Usuarios
- Empresas
- Clientes
- Contactos
- Interacciones

requieren un usuario autenticado.

Cualquier petición sin autenticación válida será rechazada.

---

## Autorización y permisos (nivel conceptual)

Además de la autenticación, el sistema contempla mecanismos de
**autorización**, que determinan qué acciones puede realizar un usuario.

A nivel conceptual:

- Todos los usuarios pertenecen a una única empresa.
- Un usuario solo puede acceder a recursos de su empresa.
- Determinadas acciones podrán restringirse según rol o permisos.

La definición concreta de roles y permisos se detallará en documentos
específicos o fases posteriores del proyecto.

---

## Autenticación y multitenant

El sistema sigue un enfoque multiempresa (multitenant), por lo que:

- El usuario autenticado determina automáticamente el contexto de empresa.
- El frontend no envía explícitamente el identificador de empresa.
- El backend filtra los datos según la empresa asociada al usuario autenticado.

Este comportamiento es transparente para el cliente frontend.

---

## Gestión de errores de autenticación

En caso de error durante el proceso de autenticación, la API devolverá
respuestas claras y coherentes, por ejemplo:

- Credenciales inválidas.
- Usuario inexistente.
- Usuario inactivo o deshabilitado.
- Token ausente o inválido.

El formato de los errores seguirá las convenciones definidas en la
documentación de manejo de errores de la API.

---


## Evolución prevista

En fases posteriores del proyecto se contempla:

- Implementación técnica del sistema de tokens.
- Ampliación del control de acceso mediante roles y permisos.
- Integración con mecanismos de recuperación de credenciales.
- Refuerzo de medidas de seguridad según necesidades del sistema.

Estas mejoras se abordarán de forma incremental conforme avance el desarrollo.
