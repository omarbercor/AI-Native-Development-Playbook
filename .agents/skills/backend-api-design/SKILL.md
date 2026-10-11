---
name: backend-api-design
description: >-
  Backend architecture and REST API design specialist. Use this skill when defining
  REST endpoints, database models, OpenAPI contracts, authentication flows, error schemas,
  and input boundary validation.
---

# Backend API Design — Arquitectura y Diseño de APIs REST

Esta habilidad asegura que cualquier backend o API construida bajo este sistema sea robusta, predecible, segura y fácil de consumir por el frontend.

---

## 1. Principio API-First y Contratos de Datos

1. **Diseñar el contrato antes de programar**:
   - Definir la estructura exacta de la petición (Headers, Query, Body) y la respuesta esperada antes de implementar la lógica interna.
2. **Validación en el Borde**:
   - Todo dato que entra por un endpoint se valida estrictamente mediante un esquema (Zod, Joi, Pydantic, etc.).
   - Rechazar de inmediato cualquier petición con tipos incorrectos o campos maliciosos con código `400 Bad Request`.

---

## 2. Formato Estándar de Errores (Error Handling Contract)

Todas las respuestas de error de la API deben retornar un objeto con estructura consistente:

```json
{
  "success": false,
  "status": 400,
  "error_code": "INVALID_INPUT_DATA",
  "message": "El campo email no tiene un formato válido.",
  "trace_id": "req-987213a8-b4c1",
  "timestamp": "2026-10-10T21:30:00Z"
}
```

- **`status`**: Código HTTP numérico (400, 401, 403, 404, 409, 429, 500).
- **`error_code`**: Identificador interno en `SCREAMING_SNAKE_CASE` para que el frontend pueda manejarlo programáticamente.
- **`message`**: Explicación segura y amigable (sin detalles internos de base de datos ni stacktraces expuestos).

---

## 3. Idempotencia y Métodos HTTP

- **GET**: Idempotente y seguro. Nunca modifica estado.
- **POST**: Creación de recursos o acciones no idempotentes. Si se manejan pagos o transacciones críticas, exigir `Idempotency-Key` en headers.
- **PUT**: Reemplazo total de un recurso.
- **PATCH**: Actualización parcial de campos.
- **DELETE**: Eliminación de recurso (debe ser idempotente).

---

## 4. Seguridad de Backend (Reglas Invariantes)

1. **Hash de Contraseñas**: Usar `Argon2id` o `Bcrypt` con salt adecuado. Prohibido SHA256 o MD5 para contraseñas.
2. **Consultas a Base de Datos**: Obligatorio el uso de consultas parametrizadas o un ORM/Query Builder seguro. Prohibida la concatenación directa de strings en queries SQL.
3. **Control de Rate Limiting**: Todo endpoint público (especialmente auth, login y envío de correos) debe tener limitación de tasa por IP / usuario (`429 Too Many Requests`).
