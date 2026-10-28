---
name: project-planner
description: >-
  Project planning, architectural strategy, frontend profile selection, and modular file
  decomposition (< 200 lines). Use this skill when starting a new feature or project,
  defining a PRD, conducting a 3-Path Analysis, planning Astro/WordPress/CloudPanel architectures,
  or writing Architecture Decision Records (ADRs).
---

# Project Planner — Planificación Estratégica y Arquitectura Modular (10/10)

Esta habilidad guía la descomposición de requerimientos en planes de ejecución atómicos **antes de escribir código**, evitando archivos monolíticos, retrabajos y sobreingeniería.

---

## 1. Decisiones Iniciales Obligatorias del Proyecto

Al planificar cualquier proyecto o módulo nuevo, define explícitamente estas 3 coordenadas:

1. **Perfil de Interfaz (Si incluye Frontend)**:
   - 🏆 **`awwwards-experience-craft`**: Sitios de experiencia, marca, portafolios o lanzamientos creativos (tipografía monumental, espacio negativo activo, GSAP/Lenis, Three.js).
   - 🎨 **`web-frontend-craft`**: Aplicaciones funcionales, SaaS, dashboards, e-commerce o portales CMS donde priman la claridad, formularios y `mobile-native`.
   - *(En proyectos híbridos, ej. Landing inmersiva + Dashboard interno, asigna el perfil correspondiente a cada ruta).*
2. **Mapa de Módulos Atómicos (`< 150–200 líneas` por archivo)**:
   - Diseña el árbol de carpetas y archivos **antes de programar**. Ninguna vista, escena 3D, endpoint o clase PHP planificada debe concentrar múltiples responsabilidades.
3. **Destino de Despliegue**:
   - Por defecto considera el flujo estándar en **CloudPanel + CI/CD con Local Runner** (definiendo si el entregable será Astro estático, Astro SSR con PM2 o tema/plugin WordPress aislado).

---

## 2. El Análisis de 3 Caminos (3-Path Analysis)

Ante decisiones técnicas con trade-offs importantes, presenta al usuario tres opciones claras:

| Camino | Enfoque | Cuándo Conviene |
| :--- | :--- | :--- |
| **Camino A: Prototipo Rápido** | Velocidad máxima de validación con deuda técnica documentada (`[DEBT-XXX]`). | Pruebas de concepto, demos comerciales o validación de idea. |
| **Camino B: Arquitectura Escalable** | Modularidad estricta, contratos tipados, caché y pruebas automatizadas. | Productos en producción, sitios de cliente final y sistemas core. |
| **Camino C: Mínimo Mantenimiento** | Estático/Jamstack (ej. Astro puro en Nginx CloudPanel) y cero dependencias innecesarias. | Sitios de marca, portafolios y proyectos que deben correr años sin fricción. |

---

## 3. Creación del PRD y Registro de Decisiones (ADR)

1. **PRD (Product Requirements Document)** — Para features medianos o proyectos nuevos usa 👉 [prd_template.md](./references/prd_template.md), asegurando siempre:
   - Problema real y objetivo medible.
   - Flujo principal (*happy path*) + los 5 estados límite (*loading, empty, error, edge data*).
   - Criterios de aceptación verificables y estrategia de degradación elegante.
2. **ADR (Architecture Decision Record)** — Documenta decisiones estructurales (elección de stack, headless vs. monolito, motor 3D, esquema de auth) usando 👉 [adr_template.md](./references/adr_template.md).
