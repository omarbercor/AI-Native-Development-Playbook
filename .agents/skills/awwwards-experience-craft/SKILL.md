---
name: awwwards-experience-craft
description: >-
  High-impact creative frontend engineering and art direction for Awwwards-level experience websites.
  Use this skill when building, designing, or refactoring immersive brand sites, portfolios, launches,
  or editorial experiences requiring monumental typography, intentional negative space, brutalist/minimalist
  tension, organic animated backgrounds, magnetic cursors, 60fps micro-interactions (GSAP/Lenis/CSS),
  and purposeful 3D/WebGL (Three.js) across Astro, WordPress/CMS, React, or Vanilla stacks.
---

# Awwwards Experience Craft — Sitios de Experiencia de Alto Impacto (10/10)

Esta habilidad transforma al agente en un **Creative Technologist y Director de Arte Digital**. Su objetivo es construir experiencias web memorables, inmersivas y de nivel **Awwwards Site of the Day**, combinando **tipografía monumental**, **espacio negativo intencional**, **física orgánica (60 FPS)** y **código estrictamente modular (< 200 líneas por archivo)**.

> [!IMPORTANT]
> **Cuándo usar este skill vs. `web-frontend-craft`:**
> - Usa **`awwwards-experience-craft`** para sitios de experiencia, narrativa de marca, portafolios, lanzamientos y landings creativas donde la interfaz *es* el espectáculo.
> - Usa **`web-frontend-craft`** para interfaces funcionales, paneles SaaS, dashboards o vistas transaccionales donde la interfaz debe ser silenciosa y utilitaria.

---

## 1. Manifiesto Anti-Slop (Prohibiciones Absolutas)

Los modelos de IA tienden a generar el "promedio" de internet. En un sitio de experiencia queda **estrictamente prohibido**:

1. **El Layout Cliché de IA**: Hero centrado en 3 líneas + pildorita arriba + fila de 3 tarjetas iguales con íconos dentro de cuadritos redondeados.
2. **Tipografía Sin Carácter**: Usar *Inter*, *Roboto* o *Arial* para titulares de impacto. En sitios de experiencia, **la tipografía es arquitectura**.
3. **Colores "SaaS Genéricos"**: Gradientes morado-azul saturados sobre fondo `#000000` o `#FFFFFF` puro sin matiz, o texto gris apagado sobre fondos de color.
4. **Tarjetas Anidadas ("Card-in-Card")**: Abusar de cajas con bordes y sombras dentro de otras cajas. Reemplázalas por **espacio negativo**, asimetría o líneas divisorias arquitectónicas (`1px`).
5. **3D o Animación Gratuita**: Meter modelos 3D pesados sin propósito narrativo o usar `transition: all 0.3s linear` / `ease-in` que hacen sentir la interfaz lenta y torpe.

---

## 2. Los 4 Pilares de Dirección de Arte (Tu Estilo Base)

Cada sitio de experiencia se diseña sobre la combinación equilibrada de estos cuatro pilares:

### A. Tipografía como Factor de Impacto y Estructura
- **Escala Monumental**: Los titulares principales (`h1`, displays de sección) actúan como elementos gráficos dominantes usando `clamp(3rem, 9vw + 1rem, 11rem)`.
- **Ajuste Óptico (Tracking & Leading)**: A mayor tamaño de letra, menor interlínea (`line-height: 0.88` a `0.96`) y menor espaciado entre letras (`letter-spacing: -0.03em` a `-0.06em`).
- **Contraste de Voces Tipográficas**: Combina una fuente Display expresiva (Grotesca de carácter o Serif editorial) con una **Monospace técnica** pequeña (`0.75rem`, uppercase, tracking `+0.08em`) para metadatos, coordenadas e índices (`[ 01 / 04 ]`).

### B. Espacio Negativo Activo (Minimalismo + Tensión Brutalista)
- **El vacío aporta valor**: Al menos el **35%–45% del viewport** debe ser espacio abierto que deje respirar a la tipografía gigante y al elemento interactivo focal.
- **Asimetría Editorial**: Rompe la simetría central. Alinea titulares en columnas desplazadas (ej. columnas 1–8 de una grilla de 12) y coloca notas o micro-copys en los extremos (columnas 10–12).
- **Estructura Cruda (Brutalismo Sutil)**: Cuando se requiera orden, usa grillas expuestas de `1px solid` con baja opacidad (`rgba(255,255,255,0.1)` o `rgba(0,0,0,0.12)`), numeración visible y contrastes nítidos.

### C. Texturas de Fondo Orgánicas e Interactivas
- Ningún fondo debe sentirse como un plástico plano digital.
- Aplica **grano cinematográfico sutil** (SVG noise / canvas), mallas de luz orgánicas que reaccionan suavemente al cursor (LERP), o shaders fluidos de bajo costo en GPU que aporten atmósfera sin competir con la legibilidad del texto.

### D. 3D Justificado e Inmersión Espacial
- **Regla de Oro del 3D**: Solo se incluye Three.js / WebGL si **amplifica la idea central del proyecto** (un objeto hero interactivo, distorsión de imágenes por shader al hacer scroll, o partículas espaciales reactivas).
- Si el 3D no aporta a la narrativa o penaliza el tiempo de carga, se descarta a favor de tipografía cinética + shaders 2D/CSS de alto impacto.

---

## 3. Física Orgánica y Micro-Interacciones (Estándar 60 FPS)

Inspirado en la ingeniería de diseño de Emil Kowalski y sitios premiados en Awwwards:

1. **Interpolación Lineal (LERP) y Resortes**:
   - Nada se mueve de forma robótica. Los cursores magnéticos, seguimientos de mouse y paralajes usan amortiguación suave (`lerp(current, target, 0.08)` en `requestAnimationFrame` o `gsap.quickTo`).
2. **Curvas de Easing con Personalidad**:
   - **Prohibido `ease-in`** en elementos que entran.
   - Curva estándar de micro-interacción UI (`< 250ms`): `cubic-bezier(0.22, 1, 0.36, 1)`.
   - Curva dramática de revelado editorial (`600ms–1000ms` en GSAP): `power3.out` o `expo.out`.
3. **Cursores Contextuales y Botones Magnéticos**:
   - Los botones y enlaces clave atraen sutilmente el cursor y su etiqueta interna a distinta velocidad (efecto magnético de doble capa).
   - El cursor cambia de estado o muestra verbos cortos (`[ VER ]`, `[ EXPLORAR ]`, `[ ARRASTRAR ]`) al pasar sobre áreas interactivas o galerías.
4. **Revelado de Texto por Máscara (Split-Text)**:
   - Los titulares grandes emergen desde abajo dentro de contenedores con `overflow: hidden` (`translateY(110%)` -> `0%`, con rotación sutil de `2deg` a `0deg` y `stagger: 0.04s`), nunca con un simple fade-in aburrido.

---

## 4. Mobile-Native y Resiliencia (`break-ui`)

Un sitio Awwwards que se rompe o congela en el teléfono es un fracaso de ingeniería:

1. **Detección de Puntero Fino**: Todo cursor custom, efecto magnético o listener de `mousemove` **debe** desactivarse automáticamente en pantallas táctiles mediante `@media (hover: hover) and (pointer: fine)` y `window.matchMedia('(pointer: fine)')`.
2. **Viewport Real**: Usar siempre `100dvh` / `100svh` en lugar de `100vh` para evitar saltos con la barra de direcciones en iOS/Android.
3. **Degradación Inteligente de 3D/Shaders**: En móviles, limitar `renderer.setPixelRatio(Math.min(window.devicePixelRatio, 1.5))`, reducir geometrías o alternar a una versión ligera sin perder la estética.
4. **Prueba de Estrés (`break-ui`)**: Verificar que titulares en español (palabras largas) no generen scroll horizontal en `375px` usando `overflow-wrap: break-word` y escalas `clamp()` bien calibradas. Respetar siempre `prefers-reduced-motion: reduce`.

---

## 5. Arquitectura Modular (< 200 Líneas) y Agnóstica al Stack

Ya sea en **Astro**, **WordPress (PHP/Blocks/Headless)**, **Next.js/React** o **Vanilla**, está **prohibido crear archivos monolíticos**. Cumple estrictamente la regla de **máximo 150–200 líneas por archivo**:

```text
src/ (o directorio del tema/plugin)
├── styles/
│   ├── tokens.css             ← Variables CSS: tipografía clamp(), paleta, curvas (< 120 líneas)
│   └── base.css               ← Reset, grano orgánico, utilidades de grilla (< 120 líneas)
├── motion/ (Módulos ES puros y reutilizables, desacoplados por data-attributes)
│   ├── smooth-scroll.js       ← Instancia de Lenis + integración GSAP ScrollTrigger (< 60 líneas)
│   ├── magnetic-cursor.js     ← Cursor contextual + física magnética en [data-magnetic] (< 120 líneas)
│   └── text-reveal.js         ← Animaciones de entrada en [data-reveal] con cleanup (< 100 líneas)
├── canvas/ (Solo si aplica 3D / Shaders)
│   ├── organic-bg.js          ← Escena Three.js / Shader con IntersectionObserver (< 150 líneas)
│   └── shaders/               ← Vertex y Fragment shaders aislados
└── sections/ (o template-parts/ en WordPress / components/ en Astro)
    ├── HeroExperience.*       ← Vista limpia solo con marcado semántico (< 120 líneas)
    └── ShowcaseSection.*      ← Cada sección de la página en su propio archivo (< 150 líneas)
```

- **Acoplamiento por `data-*` attributes**: En lugar de atar la animación a un framework específico, usa atributos HTML (`data-magnetic="0.3"`, `data-reveal="lines"`, `data-cursor-text="VER"`). Así el mismo módulo JS de 80 líneas sirve en un componente `.astro`, en una plantilla `.php` de WordPress o en React.
- **Limpieza de Memoria Obligatoria**: Toda instancia de GSAP (`ctx.revert()`), Lenis (`lenis.destroy()`) y Three.js (`geometry.dispose()`, `renderer.dispose()`) debe limpiarse al desmontar o cambiar de página (`astro:before-swap`).

---

## 6. Comandos Mentales de Iteración (Vocabulario de Dirección)

Cuando el usuario pida ajustar el diseño con estos términos, aplica el protocolo correspondiente:
- **`bolder` (Más Brutal / Arriesgado)**: Aumenta el contraste tipográfico (+30% en displays), endurece la asimetría, expone bordes de `1px` y acentúa el movimiento.
- **`distill` (Minimalismo Radical)**: Elimina el 40% de los elementos decorativos, bordes redundantes o textos secundarios; deja solo tipografía monumental, espacio negativo y una interacción clave.
- **`overdrive` (Inmersión Técnica)**: Eleva el factor WOW integrando un fondo reactivo con shaders/Three.js, distorsión al scroll o física magnética multicapa.
- **`harden` (Blindaje de Producción)**: Audita rendimiento a 60 FPS, pausa canvases fuera de pantalla, corrige desbordes en `375px` y asegura accesibilidad de teclado/contraste.

---

## 7. Referencias y Blueprints Modulares

Consulta estos documentos especializados según la tarea (diseñados bajo el límite de < 200 líneas):
- 📐 **Tipografía Monumental, Tokens, Layouts y Texturas Orgánicas**: [typography-layout-tokens.md](./references/typography-layout-tokens.md)
- ⚡ **Micro-Interacciones, GSAP, Lenis y Cursores Magnéticos (Astro / WP / Vanilla)**: [motion-gsap-microinteractions.md](./references/motion-gsap-microinteractions.md)
- 🌌 **Fondos Orgánicos Interactivos y 3D con Three.js / Shaders (60 FPS)**: [threejs-organic-backgrounds.md](./references/threejs-organic-backgrounds.md)

