---
name: project-planner
description: >-
  Project planning, architectural strategy, and task decomposition. Use this skill
  when starting a new feature or project, defining a PRD, conducting a 3-Path Analysis,
  or writing Architecture Decision Records (ADRs).
---

# Project Planner — Planificación Estratégica y Arquitectura

Esta habilidad guía la descomposición de requerimientos en planes de ejecución atómicos y claros antes de escribir código, evitando reescrituras y complejidad innecesaria.

---

## 1. El Análisis de 3 Caminos (3-Path Analysis)

Antes de comprometerse con una implementación técnica compleja, evalúa tres alternativas y preséntalas al usuario con sus respectivos pros y contras:

| Camino | Enfoque | Cuándo Conviene |
| :--- | :--- | :--- |
| **Camino A: Prototipo Rápido** | Velocidad máxima de entrega. Código mínimo viable. Acepta deuda técnica controlada. | Para validar ideas rápido o crear pruebas de concepto. |
| **Camino B: Arquitectura Escalable** | Modular, desacoplada, tipado estricto, preparada para alta concurrencia y crecimiento. | Para productos principales y sistemas core en producción. |
| **Camino C: Mínimo Mantenimiento** | Uso de servicios gestionados o librerías nativas probadas. Menos código custom. | Para proyectos donde se busca cero fricción operativa a largo plazo. |

---

## 2. Creación del PRD (Product Requirements Document)

Para funcionalidades de tamaño medio o grande, define un PRD estructurado utilizando la plantilla:
👉 [prd_template.md](./references/prd_template.md)

Puntos indispensables:
1. **Problema de Negocio / Usuario**: Qué dolor resuelve.
2. **Historias de Usuario**: Flujo de éxito (*happy path*) y flujos de error.
3. **Criterios de Aceptación Verificables**: Checklist inequívoco de "Hecho" (*Definition of Done*).
4. **Degradación Elegante**: Qué ocurre si un servicio de terceros o la base de datos no responde.

---

## 3. Registro de Decisiones de Arquitectura (ADR)

Si se toma una decisión técnica trascendental (cambio de base de datos, framework, estrategia de autenticación o arquitectura de estado), documéntala usando:
👉 [adr_template.md](./references/adr_template.md)
