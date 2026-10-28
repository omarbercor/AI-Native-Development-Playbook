---
name: visual-qa-testing
description: >-
  Quality assurance, browser verification, automated testing (Vitest/Playwright), and
  UI stress-testing (`break-ui`) specialist. Use this skill before marking any frontend
  or full-stack task complete, when writing unit/integration/E2E tests, auditing responsive
  breakpoints (`375px` to `1920px`), and torturing UI components with worst-case edge data.
---

# Visual QA & Testing — Auditoría Visual, `break-ui` y Pruebas Automatizadas (10/10)

Ningún componente o vista está terminado porque "compila". Esta habilidad obliga al agente a **intentar romper su propia interfaz** y verificarla con rigor antes de dar cualquier entrega por cerrada.

---

## 1. Protocolo de Tortura `break-ui` (Inspirado en Emil Kowalski)

Antes de entregar una pantalla o componente, somételo a estos **6 escenarios hostiles**:

1. **Textos Extremos y Multilenguaje**:
   - Prueba nombres de 1 carácter (`"A"`) y de 85 caracteres sin espacios o palabras largas en español (`"Electroencefalografista Internacional"`). Nada debe desbordar su contenedor (`min-width: 0` en hijos Flex/Grid + `overflow-wrap: break-word` o `line-clamp`).
2. **Cantidades Límite**:
   - Prueba con `0` ítems (¿aparece un *Empty State* claro?), `1` ítem (¿se deforma la grilla?) y `9,999,999` en precios/contadores (¿se mantiene `tabular-nums` sin partir el botón?).
3. **Medios Rotos o Lentos**:
   - Simula una imagen `404` o muy vertical (`9:16` dentro de un card `16:9`): debe mantener `object-fit: cover` y `aspect-ratio` fijo sin colapsar ni provocar CLS.
4. **Doble Clic / Spam de Envío**:
   - Haz clic 5 veces seguidas rápido en un botón de formulario o compra: solo debe salir **1 petición de red** y el botón debe quedar en `aria-busy="true"` / `disabled`.
5. **Solo Teclado + Zoom 200%**:
   - Recorre la vista únicamente con `Tab`, `Shift + Tab`, `Enter` y `Escape`: el foco (`:focus-visible`) nunca debe perderse ni quedar atrapado fuera de un modal abierto.
6. **Limpieza de Ciclo de Vida (Sitios con Animación / 3D)**:
   - Navega de ida y vuelta 3 veces entre páginas: verifica que no queden instancias duplicadas de `ScrollTrigger`, cursores fantasma ni loops `requestAnimationFrame` consumiendo CPU.

---

## 2. Matriz de Inspección Responsive y Consola Limpia

Valida obligatoriamente estas 4 resoluciones reales:
- **`375 × 667` (Móvil compacto)**: Cero scroll horizontal (`document.documentElement.scrollWidth <= window.innerWidth`), inputs `>= 16px`, áreas táctiles `>= 44px` y tipografía `clamp()` legible.
- **`768 × 1024` (Tablet vertical)**: Grillas intermedias equilibradas (sin columnas ridículamente estrechas).
- **`1440 × 900` (Laptop estándar)**: Jerarquía visual y ritmo de sección óptimos.
- **`1920 × 1080+` (Monitor amplio)**: Contenedores con `max-width` controlado o escalado proporcional para que las líneas de texto nunca superen `70ch`.

**Criterio de Consola Cero-Errores**:
- `0` errores en consola JS (`ReferenceError`, hidratación fallida en Astro/React, promesas no capturadas).
- `0` recursos rotos (`404` en fuentes, scripts o imágenes) en la pestaña Network.

---

## 3. Estrategia Pragmática de Tests Automatizados

Escribe tests que aporten confianza real, sin inflar el proyecto con pruebas frágiles:

1. **Unitarios / Integración Rápida (`Vitest` / `PHPUnit`)**:
   - Obligatorios para lógica de negocio, cálculos de precios/impuestos, validadores de esquemas (Zod), transformadores de datos CMS y helpers puros.
   - 100% deterministas: **prohibido llamar APIs reales** (Stripe, SMTP, LLMs) en tests; usa *mocks/stubs* e inyección de dependencias.
2. **Flujos Críticos E2E (`Playwright`)**:
   - Cubrir los caminos donde perder un bug cuesta dinero o reputación: envío de formulario de contacto/lead, login/registro, carrito/checkout y renderizado correcto de la home en móvil y desktop.
