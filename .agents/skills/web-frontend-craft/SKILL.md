---
name: web-frontend-craft
description: >-
  Expert frontend craftsmanship for web applications and sites. Use this skill when
  designing, building, or refactoring UI/UX, CSS design systems, modern animations,
  responsive layouts, glassmorphism effects, 60fps micro-interactions, and accessible components (WCAG).
---

# Web Frontend Craft — Artesanía de Interfaz de Usuario (10/10)

Esta habilidad dota al agente de las directrices y estándares para construir experiencias web con **efecto WOW**, rendimiento extremo (60 FPS) y accesibilidad universal.

---

## 1. Principios de Diseño Visual "Efecto WOW"

Una interfaz profesional nunca se siente básica, genérica ni plana. Sigue estos estándares:

1. **Color y Gradientes Modernos**:
   - Evita colores primarios puros saturados (`#FF0000`, `#0000FF`).
   - Utiliza sistemas de color HSL o OKLCH con tonos calibrados para modo oscuro y claro.
   - Aplica gradientes sutiles y mallas de color con baja opacidad en fondos y acentos.
2. **Profundidad y Glassmorphism**:
   - Emplea capas translúcidas con `backdrop-filter: blur(12px)`.
   - Añade bordes ultrafinos y sutiles: `border: 1px solid rgba(255, 255, 255, 0.08)`.
   - Sombras difusas y elegantes con múltiples capas (`box-shadow: 0 10px 30px -10px rgba(0,0,0,0.3)`).
3. **Tipografía de Primera Clase**:
   - Usa fuentes modernas de Google Fonts: *Inter*, *Outfit*, *Plus Jakarta Sans* o *Space Grotesk*.
   - Corrige el interletreado (*letter-spacing*): `-0.02em` a `-0.04em` en títulos grandes para dar peso tipográfico premium.
   - Usa tipografía fluida con `clamp()` para que escale suavemente entre móviles y pantallas de escritorio.

---

## 2. Micro-Interacciones y Animación Fluida

Una web estática parece inanimada; una web con movimientos bruscos marea. El equilibrio es la fluidez:

1. **Propiedades de Alto Rendimiento**:
   - Animar **únicamente** `transform` y `opacity`. Estas propiedades corren en la GPU y no disparan repintados (*repaints*) ni cambios de layout (*reflows*).
2. **Easings Naturales**:
   - Prohibido el movimiento lineal (`transition: all 0.3s linear`).
   - Usar curvas cúbicas de desaceleración: `cubic-bezier(0.16, 1, 0.3, 1)` o resortes (*springs*).
3. **Feedback Inmediato**:
   - Todo botón o elemento clickeable debe reaccionar al hover, focus y active (escalado suave de `scale(1.02)` o elevación de sombra).

---

## 3. Arquitectura de Estilos y Tokens CSS

1. **Tokens Centralizados en `:root`**:
   - Toda la paleta, espaciado, radios de borde y transiciones deben residir en variables CSS en `index.css` o `tokens.css`.
   - Nomenclatura semántica: `--surface-card`, `--text-primary`, `--accent-brand`, en lugar de nombres de colores como `--azul-50`.
2. **Escala de Espaciado Modular**:
   - Basada en múltiplos de 4px u 8px (`0.25rem`, `0.5rem`, `1rem`, `1.5rem`, etc.).

---

## 4. Estructura y Responsive "Mobile-First"

1. **Flexbox y Grid Nativos**:
   - No usar anchos fijos en píxeles que causen desbordamiento horizontal.
   - Utilizar `minmax()`, `grid-template-columns: repeat(auto-fit, minmax(280px, 1fr))` para rejillas intrínsecamente adaptativas.
2. **Zonas Táctiles Adecuadas**:
   - Objetivos táctiles mínimos de 44x44px en dispositivos móviles.
3. **Accesibilidad (A11y)**:
   - Contraste mínimo de 4.5:1 para texto normal (WCAG AA).
   - Elementos interactivos con `focus-visible` claramente delineado para navegación por teclado.
   - Etiquetas semánticas: `<main>`, `<nav>`, `<article>`, `<section>`, `<header>`, `<footer>`.

---

## 5. Referencias y Snippets Rápidos

Para obtener snippets listos para usar de tokens, paletas de modo oscuro, y estilos de glassmorphism, consulta:
👉 [tokens_and_styles.md](./references/tokens_and_styles.md)
