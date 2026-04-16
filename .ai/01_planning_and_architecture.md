# 01 — Planeación y Arquitectura (PRD & ADR)

Reglas de negocio y arquitectura puras. Ningún sub-agente escribe código sin que el orquestador haya validado que existe un plan aprobado. La velocidad de implementación no justifica la ausencia de diseño previo.

---

## 1. Estándar de PRD (Product Requirements Document)

Todo feature de mediana o alta complejidad se documenta en un PRD antes de iniciar desarrollo. Un feature sin PRD aprobado no entra al sprint.

**Estructura mínima obligatoria de un PRD:**

```markdown
## Problema
¿Qué falla, falta o genera fricción hoy? Descripción en lenguaje de negocio, no técnico.

## Objetivo
¿Qué estado del mundo queremos lograr cuando este feature esté terminado?

## Usuarios Afectados
¿Quién usa esta funcionalidad? ¿Cuál es su nivel técnico y contexto de uso?

## Historias de Usuario (End-to-End)
- Como [rol], quiero [acción] para [beneficio].
- Incluir el flujo completo: inicio → decisiones → resultado exitoso → resultado de error.

## Decisiones Arquitectónicas (ADRs)
- ¿Qué tecnología/patrón se usa y por qué?
- ¿Qué alternativas se descartaron y con qué justificación?

## Criterios de Aceptación
Lista de condiciones verificables que determinan que el feature está "hecho".
(Estos se convierten en la Definition of Done del sprint.)

## Fallbacks y Degradación Graceful
¿Qué pasa si el servicio externo cae? ¿Si la BD no responde? ¿Si el usuario no tiene permisos?
El sistema siempre tiene un comportamiento definido para el caso de fallo.

## Métricas de Éxito
¿Cómo sabremos que este feature funciona bien en producción? (Latencia, tasa de error, adopción, etc.)
```

---

## 2. Abanico de Soluciones Obligatorio (3-Path Analysis)

La primera solución planteada rara vez es la óptima. Antes de comprometerse con una arquitectura, el orquestador exige que el sub-agente responsable presente tres caminos técnicos:

| Path | Nombre | Criterio |
|---|---|---|
| **A** | Prototipo Rápido | Velocidad de entrega máxima. Acepta deuda técnica controlada. Ideal para validar hipótesis. |
| **B** | Arquitectura Escalable | Diseñada para carga alta, multi-tenant o crecimiento futuro. Mayor inversión inicial. |
| **C** | Mínimo Costo de Mantenimiento | Usa servicios gestionados, menos infraestructura custom, menor overhead operacional. |

El orquestador presenta los tres caminos al usuario con sus **trade-offs** (tiempo, costo, complejidad, escalabilidad) antes de que se tome la decisión de implementación. No se asume el path A por defecto.

---

## 3. Decisiones Arquitectónicas (ADR — Architecture Decision Records)

Cada decisión técnica significativa que no es obvia queda registrada como un ADR. Un ADR documenta el contexto, la decisión y las consecuencias esperadas. Esto evita que el equipo "olvide" por qué se eligió una tecnología y la reemplace sin entender el costo.

**Cuándo escribir un ADR:**
- Al elegir un framework, librería o proveedor cloud.
- Al definir el esquema de autenticación/autorización.
- Al decidir entre SQL vs NoSQL, monolito vs microservicios, REST vs GraphQL.
- Al optar por una solución pragmática que introduce deuda técnica conocida.
- Al descartar una solución que parecía obvia.

**Formato mínimo de ADR:**
```markdown
## ADR-[N]: [Título de la Decisión]
**Fecha**: YYYY-MM-DD
**Estado**: Propuesto | Aceptado | Obsoleto | Reemplazado por ADR-[X]

### Contexto
¿Qué problema o restricción motivó esta decisión?

### Decisión
¿Qué se decidió exactamente?

### Consecuencias
- ✅ Beneficios esperados.
- ⚠️ Trade-offs o limitaciones aceptadas.
- 🔧 Deuda técnica que se asume conscientemente.
```

---

## 4. Jerarquía de Diseño (De lo Abstracto a lo Concreto)

El diseño sigue este orden. No se puede ir al paso N+1 si el paso N no está resuelto:

```
1. Dominio del Problema (¿Qué resolvemos?)
       ↓
2. Arquitectura del Sistema (¿Cómo se comunican las partes?)
       ↓
3. Contratos de API y Schemas de BD (¿Qué datos fluyen entre partes?)
       ↓
4. Diseño de Componentes (¿Cómo se implementa cada parte?)
       ↓
5. Implementación y Tests
       ↓
6. Observabilidad y Monitoreo
```

Saltarse el paso 3 (contratos de API) para ir directo al 4 y 5 es la causa más frecuente de re-trabajo entre frontend y backend.

---

## 5. Patrones Arquitectónicos de Referencia

El equipo tiene patrones pre-aprobados para casos de uso comunes. Usarlos reduce tiempo de diseño y alinea al equipo:

### 5.1 Arquitectura de Capas (Proyectos Web/API Estándar)
```
[Cliente / Frontend]
        ↓ HTTPS
[API Gateway / Load Balancer]
        ↓
[Capa de Aplicación — Controllers / Handlers]
        ↓
[Capa de Negocio — Services / Use Cases]
        ↓
[Capa de Datos — Repositories / ORM]
        ↓
[Base de Datos]
```
- Cada capa solo habla con la capa inmediatamente adyacente.
- La lógica de negocio nunca vive en Controllers ni en la BD.

### 5.2 Background Jobs para Tareas Pesadas
Cualquier operación que tarda más de 300ms o tiene side-effects externos (emails, webhooks, procesamiento de archivos) se mueve a una cola:
```
[API] → 202 Accepted → [Queue (Redis/BullMQ)] → [Worker] → [Resultado]
```

### 5.3 Feature Flags para Despliegues Seguros
Features nuevos o de alto riesgo se despliegan detrás de un flag antes de activarlos para todos los usuarios. Permite rollback instantáneo sin revertir código.

---

## 6. Modelado de Datos (Principios Pre-Implementación)

Antes de que `backend-architect` escriba una sola migración:

- **Normalización por Defecto**: Esquemas en Tercera Forma Normal (3FN) como punto de partida. La desnormalización es una optimización deliberada y documentada, no el estado inicial.
- **Nomenclatura Consistente**: Tablas en `snake_case` plural (`user_sessions`), columnas en `snake_case` singular (`created_at`). Las inconsistencias en naming de BD son permanentes y costosas.
- **Campos de Auditoría Obligatorios**: Toda tabla de entidad de negocio incluye: `id`, `created_at`, `updated_at`, y `deleted_at` (soft-delete por defecto).
- **Migraciones Reversibles**: Toda migración tiene su operación de rollback definida (`up` y `down`). Una migración sin rollback es una operación destructiva unidireccional.
- **Índices Deliberados**: Los índices se definen según los patrones de consulta reales documentados en el PRD, no de forma especulativa.

---

## 7. Contratos de API (API-First Design)

El contrato de API se define ANTES de que frontend y backend implementen. Esta sincronización evita el ciclo de "yo esperaba X, tú enviaste Y":

- **Formato**: OpenAPI 3.x (YAML o JSON) versionado en el repositorio junto al código.
- **Versionado de URL**: `/api/v1/` desde el primer endpoint. Los breaking changes van en `/api/v2/`.
- **Contrato de Errores**: Los formatos de error son consistentes en todos los endpoints (ver `backend_architect.md` §6).
- **Mocking Temprano**: El contrato OpenAPI sirve como fuente para generar mocks que el frontend puede consumir mientras el backend implementa.

---

## 8. Observabilidad como Requisito de Arquitectura (No Afterthought)

La observabilidad no se añade después del deployment; se diseña desde el PRD:

- **Logs Estructurados**: Formato JSON con campos mínimos: `timestamp`, `level`, `service`, `trace_id`, `message`, `context`.
- **Trazabilidad Distribuida**: Cada request lleva un `trace_id` único que se propaga entre servicios para poder reconstruir el flujo completo en los logs.
- **Métricas de Negocio vs Técnicas**: Además de CPU/RAM, se definen métricas de negocio (ej. "pagos procesados por minuto", "tasa de abandono de onboarding").
- **Alertas con Umbral**: No se escala ni se depura lo que no se mide. Las alertas se definen junto con el feature, no cuando ya hay incidente.

---

## 9. Gestión del Backlog y Deuda Técnica

- **Backlog Priorizado**: Las tareas pendientes viven en `active_context.md`. No en la cabeza del desarrollador, no en un chat de Slack.
- **Deuda Técnica Explícita**: Cada workaround deliberado se etiqueta con `[DEUDA]` en el comentario del código y se registra en `active_context.md` con su impacto esperado si no se resuelve.
- **Regla del Boy Scout**: Cada vez que se entra a un módulo, se deja un poco más limpio de como se encontró. No como objetivo principal, pero sí como hábito.
- **Anti-Pattern: Backlog Infinito**: Si una tarea del backlog no ha sido tocada en dos sprints, se evalúa si sigue siendo relevante. Si no lo es, se elimina. Un backlog de 200 items es ruido, no planificación.

---

## 10. Documentación Técnica Orientada a Humanos

Cuando el proyecto alcanza una complejidad que requiere incorporar nuevos miembros o integraciones externas:

- **Tech Overview**: El rol de documentación técnica (o el orquestador en ausencia de un rol dedicado) publica `.ai/docs/tech_overview.md` con: diagrama de arquitectura, flujo de datos principal, endpoints clave y decisiones de diseño más importantes.
- **README Funcional**: El `README.md` del repositorio es la puerta de entrada. Debe incluir: propósito del proyecto, cómo levantar el entorno local en menos de 5 comandos, variables de entorno requeridas y links a documentación extendida.
- **Onboarding en < 30 Minutos**: Si un desarrollador nuevo no puede tener el entorno local corriendo en 30 minutos siguiendo el README, el README está incompleto.
- **Diagramas como Código**: Los diagramas de arquitectura se escriben en Mermaid o PlantUML dentro del repositorio, no en herramientas externas desincronizadas.
