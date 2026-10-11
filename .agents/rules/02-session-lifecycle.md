# 02 — Regla Invariante: Ciclo de Vida de Sesión y Memoria Persistente (RAM)

> [!IMPORTANT]
> **PROTOCOLO DE CONTINUIDAD**: Para garantizar que un proyecto pueda pausarse hoy y retomarse mañana o en un mes sin perder contexto ni rumbo, el archivo `active_context.md` en la raíz del proyecto actúa como la memoria RAM del sistema.

---

## 1. Onboarding al Inicio de Sesión (Lectura Obligatoria)

Al comenzar una nueva conversación o recibir la primera instrucción en un proyecto:

1. **Inspeccionar `active_context.md`**:
   - Leer el archivo para sincronizarse con:
     - El objetivo del sprint activo.
     - El último estado conocido y los logros alcanzados.
     - Los bloqueadores o errores pendientes.
     - La deuda técnica documentada.
     - El siguiente paso planificado.
2. **Si el archivo no existe o está vacío**:
   - Inicializarlo inmediatamente utilizando la plantilla oficial de `active_context.md` antes de escribir código.

---

## 2. Cierre y Actualización de Tarea (Escritura Obligatoria)

Antes de considerar una tarea o sprint como "terminado", el agente DEBE actualizar `active_context.md`:

1. **Sección `Last Updated`**:
   - Actualizar fecha y timestamp actual.
2. **Sección `Completed in This Session`**:
   - Resumir de forma concisa y atómica lo que se construyó, probó o corrigió.
3. **Sección `Current Sprint Objective`**:
   - Marcar el progreso del objetivo o actualizar al siguiente si se ha completado.
4. **Sección `Active Technical Debt`**:
   - Registrar cualquier concesión o limitación conocida identificada con el formato `[DEBT-XXX]`. No ocultar atajos técnicos.
5. **Sección `Next Actions`**:
   - Dejar una lista numerada, clara y directa con los siguientes pasos inmediatos para que cualquier agente o humano sepa exactamente qué hacer a continuación.

---

## 3. Principio de Limpieza de la Memoria (Sin Basura Histórica)

- `active_context.md` **NO es un log de chat ni un historial infinito**.
- Solo almacena el **estado actual**.
- Las tareas y errores ya superados y verificados se limpian o condensan; la deuda técnica resuelta se elimina de la tabla activa. Mantener el archivo siempre ligero y de lectura rápida (menos de 150 líneas).
