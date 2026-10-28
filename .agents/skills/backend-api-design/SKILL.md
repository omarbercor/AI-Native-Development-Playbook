---
name: backend-api-design
description: >-
  Backend architecture and production-grade REST API design specialist. Use this skill
  when designing endpoints, database queries, authentication/RBAC, pagination, webhook
  HMAC verification, rate limiting, error contracts, and boundary validation.
---

# Backend API Design — Arquitectura de APIs Robustas y Seguras (10/10)

Esta habilidad corrige las omisiones típicas de código backend generado por IA, garantizando APIs **predecibles, blindadas en el borde, resistentes a picos de tráfico y libres de cuellos de botella**.

---

## 1. Validación Estricta en el Borde y Contratos Tipados

1. **Cero Confianza en el Cliente (*Parse, Don't Just Validate*)**:
   - Valida `body`, `query` y `params` en la primera línea del controlador usando esquemas estrictos (**Zod**, **Pydantic**, **Valibot** o validación nativa en WP REST) rechazando campos desconocidos (*strip/strict*) con `400 Bad Request`.
2. **DTOs de Salida Explícitos**:
   - **Jamás** devuelvas el objeto crudo de la base de datos (`return user` exponiendo `password_hash`, tokens internos o metadatos privados). Mapea explícitamente los campos permitidos de respuesta.

---

## 2. Contrato Unificado de Respuestas y Errores

Toda respuesta de error sigue una estructura invariante y segura (sin filtrar *stacktraces* ni nombres de tablas SQL en producción):

```json
{
  "success": false,
  "status": 422,
  "error_code": "VALIDATION_FAILED",
  "message": "Revisa los campos marcados antes de continuar.",
  "details": [{ "field": "email", "issue": "Formato de correo inválido" }],
  "trace_id": "req_01j9x8k2m4"
}
```

- Usa códigos HTTP semánticos precisos: `400` (payload malformado), `401` (no autenticado), `403` (autenticado sin permiso), `404` (recurso inexistente), `409` (conflicto/duplicado), `422` (regla de negocio/validación), `429` (rate limit), `500` (fallo interno registrado en logs privados con `trace_id`).

---

## 3. Reglas de Producción donde los LLMs Suelen Fallar

1. **Paginación Obligatoria en Listados**:
   - **Prohibido** hacer `SELECT *` o `findMany()` sin límite en un endpoint. Toda colección exige paginación (`cursor` o `page`/`limit`) con un tope duro en servidor (`Math.min(limit, 100)`).
2. **Prevención de Consultas N+1 e Índices**:
   - Prohibido ejecutar queries a base de datos dentro de un bucle `for` / `.map()`. Carga relaciones en lote (*batching / eager loading*) y exige índices en toda columna usada en `WHERE`, `JOIN` y `ORDER BY`.
3. **Timeouts en Llamadas Externas**:
   - Todo `fetch()` o cliente HTTP hacia APIs de terceros (pasarelas de pago, CRMs, CMS headless, LLMs) **debe** incluir `signal: AbortSignal.timeout(8000)` y manejo de error `502/504` para no colgar hilos de Node/PHP indefinidamente.
4. **Webhooks Seguros e Idempotentes**:
   - Al recibir webhooks (Stripe, GitHub, pasarelas, CMS), verifica primero la firma criptográfica **HMAC sobre el raw body** usando comparación en tiempo constante (`crypto.timingSafeEqual`).
   - Guarda el `event_id` procesado en base de datos/Redis para que reintentos del proveedor sean **idempotentes** y no dupliquen cobros, correos ni pedidos.

---

## 4. Seguridad Innegociable de API

1. **Contraseñas y Tokens**: Hashing exclusivo con `Argon2id` o `bcrypt` (`cost >= 12`). Tokens de sesión en cookies `HttpOnly; Secure; SameSite=Lax` (o JWT de vida corta + rotación).
2. **SQL Parametrizado**: Consultas preparadas u ORM seguro en el 100% de accesos a datos.
3. **Rate Limiting y CORS Estricto**:
   - Aplicar límite de peticiones por IP/cuenta en endpoints de login, recuperación de clave, formularios públicos y búsqueda (`429 Too Many Requests` + header `Retry-After`).
   - Configurar `Access-Control-Allow-Origin` con la lista blanca exacta de dominios permitidos; prohibido `*` en rutas autenticadas.
