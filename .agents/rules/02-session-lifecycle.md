---
trigger: always_on
description: Protocolo invariante de lectura y escritura de memoria persistente en active_context.md al iniciar y cerrar sesión.
---

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
   - Inicializarlo inmediatamente utilizando la plantilla oficial `active_context.example.md` (o estructura equivalente) antes de escribir código.

---

## 2. Cierre y Actualización de Tarea (Escritura Obligatoria)

Antes de considerar una tarea o sprint como "terminado", el agente DEBE actualizar `active_context.md` respetando sus nombres de sección canónicos:

1. **Cabecera `> Last Updated:` y sección `## 🗓️ Última Actualización`**:
   - Actualizar fecha, sesión y estado actual (`🟢 Estable`, `🟡 En desarrollo`, `🔴 Bloqueado`).
2. **Sección `## 🎯 Objetivo del Sprint Actual`**:
   - Marcar el progreso del objetivo o actualizar al siguiente si se ha completado.
3. **Sección `## ✅ Completado en Esta Sesión`**:
   - Resumir de forma concisa y atómica lo que se construyó, probó o corrigió.
4. **Sección `## 🔧 Deuda Técnica Activa`**:
   - Registrar cualquier concesión o limitación conocida identificada con el formato `[DEBT-XXX]`. No ocultar atajos técnicos.
5. **Sección `## 📋 Próximas Acciones`**:
   - Dejar una lista numerada, clara y directa con los siguientes pasos inmediatos para que cualquier agente o humano sepa exactamente qué hacer a continuación.

---

## 3. Principio de Limpieza de la Memoria (Sin Basura Histórica)

- `active_context.md` **NO es un log de chat ni un historial infinito**.
- Solo almacena el **estado actual**.
- Las tareas y errores ya superados y verificados se limpian o condensan; la deuda técnica resuelta se elimina de la tabla activa. Mantener el archivo siempre ligero y de lectura rápida (menos de 150 líneas).
