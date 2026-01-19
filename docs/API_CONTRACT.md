# Contrato de la API (API Contract)

Este documento define el contrato conceptual de la API REST del proyecto
CRM Django. Su objetivo es establecer las convenciones, estructuras y
principios que regirán la comunicación entre el backend y el frontend.

El contenido de este documento no constituye una especificación cerrada,
sino una guía de referencia destinada a garantizar coherencia, claridad
y alineación entre las distintas partes del sistema.

---

## Objetivo del API Contract

Los objetivos principales de este contrato son:

- Definir una estructura clara y consistente de endpoints.
- Facilitar la integración entre frontend y backend.
- Evitar decisiones improvisadas durante el desarrollo.
- Servir como referencia técnica dentro del proyecto y del TFG.
- Alinear la API con el dominio definido en la documentación de arquitectura,
  base de datos e interfaz de usuario.

---

## Enfoque general

La API del proyecto sigue un enfoque **RESTful** y está diseñada bajo el
principio **API-first**, de modo que el backend expone los recursos del
sistema mediante endpoints HTTP que devuelven datos en formato JSON.

El frontend consume esta API de forma desacoplada, sin conocimiento
directo de la implementación interna del backend.

---

## Convenciones generales

### Base URL

Todos los endpoints de la API estarán agrupados bajo un prefijo común:
```
/api/
```
---

## Autenticación (conceptual)

El acceso a la API estará protegido mediante mecanismos de autenticación.

- Los endpoints públicos se limitarán al proceso de autenticación.
- El resto de endpoints requerirán un usuario autenticado.
- La autenticación se gestionará mediante tokens (definidos en fases posteriores).

La implementación concreta del sistema de autenticación se abordará
en la fase correspondiente del roadmap.

---

## Multitenant y contexto de empresa

El sistema sigue un enfoque multiempresa (multitenant).

- Cada usuario pertenece a una única empresa.
- Todos los recursos del sistema están asociados a una empresa.
- El filtrado por empresa se gestiona en el backend y no se expone
  explícitamente en la API.
- El frontend no necesita enviar explícitamente el identificador de empresa.

Desde el punto de vista del frontend, los endpoints devuelven únicamente
los datos pertenecientes a la empresa del usuario autenticado.

---

## Recursos principales de la API

A continuación se describen los recursos principales expuestos por la API
y las operaciones básicas previstas para cada uno de ellos.

---

### Recurso: Usuarios

Gestión de los usuarios del sistema.

Endpoints previstos:
```
GET    /api/usuarios/
GET    /api/usuarios/{id}/
POST   /api/usuarios/
PUT    /api/usuarios/{id}/
DELETE /api/usuarios/{id}/
```

---

### Recurso: Empresas

Gestión de la entidad empresa.

Endpoints previstos:
```
GET    /api/empresas/
GET    /api/empresas/{id}/
POST   /api/empresas/
PUT    /api/empresas/{id}/
DELETE /api/empresas/{id}/
```

Filtros previstos:

- Búsqueda por nombre.

Nota: La eliminación de empresas podrá restringirse o ajustarse en fases
posteriores para preservar la integridad histórica del sistema.
---

### Recurso: Clientes

Gestión de los clientes del CRM.
Este recurso constituye el núcleo funcional del sistema.

Endpoints previstos:
```
GET    /api/clientes/
GET    /api/clientes/{id}/
POST   /api/clientes/
PUT    /api/clientes/{id}/
DELETE /api/clientes/{id}/
```
Filtros previstos:

- Búsqueda por nombre.

- Filtrado por tipo de cliente (persona / empresa).

---

### Recurso: Contactos

Gestión de los contactos asociados a clientes.

Endpoints previstos:
```
GET    /api/contactos/
GET    /api/contactos/{id}/
POST   /api/contactos/
PUT    /api/contactos/{id}/
DELETE /api/contactos/{id}/
```


Filtros previstos:

- Filtrado por cliente.

---

### Recurso: Interacciones

Gestión del historial de interacciones con clientes.

Endpoints previstos:
```
GET    /api/interacciones/
GET    /api/interacciones/{id}/
POST   /api/interacciones/
PUT    /api/interacciones/{id}/
DELETE /api/interacciones/{id}/
```

Filtros previstos:

- Filtrado por cliente.
- Filtrado por tipo de interacción.
- Filtrado por fecha.

---

## Códigos de respuesta HTTP

La API utilizará los códigos de estado HTTP estándar:

- `200 OK` — Petición correcta.
- `201 Created` — Recurso creado correctamente.
- `400 Bad Request` — Datos inválidos.
- `401 Unauthorized` — Usuario no autenticado.
- `403 Forbidden` — Acceso no permitido.
- `404 Not Found` — Recurso no encontrado.
- `500 Internal Server Error` — Error del servidor.

---

## Manejo de errores

Las respuestas de error se devolverán en formato JSON con un mensaje
descriptivo que permita al frontend gestionar adecuadamente la situación.

Ejemplo conceptual:
```json
{
  "error": "Mensaje descriptivo del error"
}
```
