# Manejo de errores de la API (Error Handling)

Este documento define el contrato conceptual de manejo de errores
de la API REST del proyecto CRM Django.

Su objetivo es establecer un comportamiento consistente, predecible
y comprensible ante situaciones de error, garantizando una correcta
comunicación entre backend y frontend.

Este documento no describe una implementación técnica concreta, sino
las reglas y estructuras que deben cumplirse en la gestión de errores.

---

## Objetivo del manejo de errores

Los objetivos principales de este contrato son:

- Proporcionar respuestas de error claras y consistentes.
- Facilitar al frontend la correcta gestión de errores.
- Diferenciar errores de validación, autenticación y sistema.
- Evitar respuestas ambiguas o difíciles de interpretar.
- Alinear el comportamiento de errores con el contrato de la API.

---

## Principios generales

El sistema de manejo de errores se rige por los siguientes principios:

- Todas las respuestas de error se devuelven en formato JSON.
- Los errores utilizan códigos de estado HTTP estándar.
- El backend es la única fuente de verdad sobre el estado del error.
- El frontend no debe inferir errores a partir de datos incompletos.
- Los mensajes de error deben ser descriptivos pero no exponer información sensible.

---

## Estructura estándar de una respuesta de error

Todas las respuestas de error de la API seguirán una estructura común:

```json
{
  "error": {
    "code": "string",
    "message": "Descripción legible del error"
  }
}
```
---

### Campos definidos

- **code** 
    Identificador interno del tipo de error.
    Permite al frontend aplicar lógica específica si es necesario.

- **message**
    Mensaje descriptivo del error, pensado para ser mostrado o interpretado
    por el usuario final.

---

## Tipos de errores

### Errores de validación (400 Bad Request)

Se producen cuando los datos enviados por el cliente no son válidos
o no cumplen las reglas del dominio.

Ejemplos:

- Campos obligatorios ausentes.

- Formatos incorrectos.

- Valores fuera de rango.

Ejemplo conceptual:

```json
{
  "error": {
    "code": "validation_error",
    "message": "Los datos enviados no son válidos."
  }
}

```
---

### Errores de autenticación (401 Unauthorized)

Se producen cuando el usuario no está autenticado correctamente.

Ejemplos:

- Credenciales inválidas.

- Token ausente.

- Token expirado o inválido.

Ejemplo conceptual:

```json
{
  "error": {
    "code": "authentication_error",
    "message": "Autenticación requerida."
  }
}
```
---

### Errores de autorización (403 Forbidden)

Se producen cuando el usuario está autenticado, pero no tiene permiso
para realizar la acción solicitada.

Ejemplos:

- Acceso a recursos de otra empresa.

- Operaciones restringidas por rol o permisos.

Ejemplo conceptual:

```json
{
  "error": {
    "code": "permission_denied",
    "message": "No tiene permisos para realizar esta acción."
  }
}
```
---

### Errores de recurso no encontrado (404 Not Found)

Se producen cuando el recurso solicitado no existe o no es accesible
para el usuario autenticado.

Ejemplos:

- Cliente inexistente.

- Contacto no asociado a la empresa del usuario.

Ejemplo conceptual:
```json
{
  "error": {
    "code": "not_found",
    "message": "El recurso solicitado no existe."
  }
}
```
---

### Errores internos del sistema (500 Internal Server Error)

Se producen cuando ocurre un fallo inesperado en el backend.

Características:

- No deben exponer detalles técnicos.

- Se consideran errores no controlados.

- Deben registrarse internamente para diagnóstico.

Ejemplo conceptual:

```json
{
  "error": {
    "code": "internal_error",
    "message": "Se ha producido un error interno del sistema."
  }
}
```
---

## Relación con el modelo multitenant
En un sistema multiempresa:

El acceso a recursos de otra empresa se considera un error de autorización.

El backend no debe indicar explícitamente si un recurso existe en otra empresa.

El frontend recibe una respuesta coherente sin información sensible.

Este enfoque refuerza el aislamiento de datos y la seguridad del sistema.

---

## Responsabilidades del frontend

El frontend debe:

- Interpretar los códigos HTTP correctamente.

- Mostrar mensajes de error adecuados al usuario.

- No asumir estados incorrectos ante errores.

- Manejar de forma diferenciada errores de validación, autenticación y sistema.

---

## Evolución prevista

En fases posteriores del proyecto se contempla:

- Ampliación de códigos de error específicos por dominio.

- Soporte para errores de concurrencia o conflictos.

- Integración con sistemas de logging y monitorización.

- Ajustes según necesidades reales del frontend.

Estas mejoras se abordarán de forma incremental conforme avance el desarrollo.