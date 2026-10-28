---
name: seo-cwv-optimizer
description: >-
  Search engine optimization (SEO), Generative Engine Optimization (GEO / llms.txt), and
  Core Web Vitals (CWV) specialist for Astro, WordPress, and animated experience sites.
  Use this skill when optimizing LCP/CLS/INP, webfont loading, Astro islands hydration,
  Schema.org JSON-LD, OpenGraph, sitemaps, and ensuring GSAP/Three.js animations never hurt SEO.
---

# SEO, GEO & Core Web Vitals Optimizer — Visibilidad y Velocidad Extrema (10/10)

Esta habilidad asegura que tanto un sitio funcional/CMS como un sitio de experiencia inmersiva (**Astro, WordPress, GSAP, Three.js**) alcance puntuaciones **95–100 en Lighthouse**, cumpla los umbrales reales de Core Web Vitals y sea indexable tanto por Google como por buscadores de IA.

---

## 1. Umbrales Innegociables y Estrategias Reales (Core Web Vitals)

| Métrica | Meta | Causa #1 de Fallo en IA | Solución Obligatoria |
| :--- | :---: | :--- | :--- |
| **LCP** | `< 2.2s` | Poner `loading="lazy"` en el Hero o bloquear con JS/fuentes | En la imagen LCP usar `loading="eager"`, `fetchpriority="high"`, formato AVIF/WebP (`<Picture />` en Astro) y nunca ocultarla tras JS lento. |
| **CLS** | `< 0.05` | Fuentes tardías, imágenes sin dimensiones o skeletons dinámicos | `width` y `height` o `aspect-ratio` explícito en todo media; fuentes `.woff2` locales con `font-display: swap`. |
| **INP** | `< 150ms` | Hidratar todo el sitio con `client:load` o listeners pesados | En Astro usar `client:visible` o `client:idle` para componentes interactivos/3D; delegar eventos y usar `{ passive: true }` en scroll/pointer. |

---

## 2. Reglas Críticas para Fuentes y Sitios con Animación (GSAP / 3D)

1. **Carga de Fuentes sin Bloqueo**:
   - Evita `@import url('https://fonts.googleapis.com/...')` en CSS (crea cascadas bloqueantes). Auto-hospeda archivos `.woff2` (o usa `@fontsource` / `astro:assets` fonts) y haz `<link rel="preload" as="font" type="font/woff2" crossorigin>` **únicamente** de las 1–2 variantes visibles en el primer pantallazo.
2. **Animaciones que No Destruyen el SEO ni el LCP**:
   - **Prohibido poner `opacity: 0` estático en el CSS base del `<h1>` o texto principal**: si el bundle JS falla, tarda en red móvil o entra un bot, la página queda en blanco y el LCP se dispara. Oculta/anima desde JS progresivo o usa `noscript` de respaldo.
   - **Accesibilidad en Split-Text**: Cuando dividas un `<h1>` en múltiples `<span>` para animar palabras o letras, añade `aria-label="Texto completo original"` al `<h1>` y `aria-hidden="true"` a los `<span>` internos para no romper lectores de pantalla ni semántica.
   - **Diferir Three.js / WebGL**: Carga el módulo 3D de forma asíncrona (`requestIdleCallback` o `import()` dinámico tras el primer pintado) para que el texto y layout aparezcan al instante.

---

## 3. Arquitectura On-Page SEO + GEO (`llms.txt`)

1. **Semántica Estricta**:
   - Exactamente **un `<h1>` por URL** con la intención principal. Jerarquía limpia `h1 → h2 → h3` sin usar niveles de encabezado solo por tamaño visual (para eso usa clases CSS como `.title-monumental`).
2. **Componente `<SEOHead />` Centralizado**:
   - `<title>` único (`50–60` caracteres), `<meta name="description">` (`140–160` caracteres), `<link rel="canonical">` absoluto, `og:title`, `og:description`, `og:image` (`1200x630` WebP/JPG con URL absoluta) y `twitter:card="summary_large_image"`.
3. **Datos Estructurados (Schema.org JSON-LD Real)**:
   - Inyectar el esquema específico del contenido (`Organization` / `LocalBusiness`, `Article` / `BlogPosting`, `Product` con `offers`, `FAQPage` o `BreadcrumbList`) validando que coincida con el texto visible en pantalla.
4. **Indexación Clásica y Generativa (GEO)**:
   - Configurar siempre `robots.txt` apuntando al `sitemap-index.xml` generado automáticamente (`@astrojs/sitemap` o sitemap limpio en WP).
   - En sitios de marca, documentación o servicios, exponer un archivo **`/llms.txt`** conciso en Markdown en la raíz pública resumiendo qué ofrece la marca, URLs canónicas clave y datos de contacto verificables para motores de respuesta con IA.
