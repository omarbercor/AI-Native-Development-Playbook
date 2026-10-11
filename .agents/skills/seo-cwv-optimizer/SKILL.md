---
name: seo-cwv-optimizer
description: >-
  Search engine optimization (SEO) and Core Web Vitals (CWV) specialist. Use this skill
  when optimizing page load speed, LCP/CLS/INP metrics, Schema.org JSON-LD, OpenGraph tags,
  semantic heading hierarchy, and robots/sitemap configurations.
---

# SEO & Core Web Vitals Optimizer — Especialista en Visibilidad y Rendimiento

Esta habilidad optimiza páginas y aplicaciones web para máxima visibilidad orgánica en buscadores y una experiencia de carga ultrarrápida medida por Google Core Web Vitals.

---

## 1. Métricas de Core Web Vitals (Objetivos Clave)

| Métrica | Objetivo Óptimo | Qué mide | Estrategia de optimización |
| :--- | :--- | :--- | :--- |
| **LCP** (Largest Contentful Paint) | `< 2.5s` | Velocidad de carga del elemento principal visible. | Priorizar carga de imágenes hero (`fetchpriority="high"`), comprimir en WebP/AVIF, eliminar CSS bloqueante. |
| **CLS** (Cumulative Layout Shift) | `< 0.1` | Estabilidad visual mientras carga la página. | Definir siempre `width` y `height` explícitos en imágenes y videos; reservar espacio con `min-height` para banners dinámicos. |
| **INP** (Interaction to Next Paint) | `< 200ms` | Capacidad de respuesta ante clicks o interacciones. | Evitar tareas largas de JavaScript que bloqueen el hilo principal; diferir scripts pesados. |

---

## 2. On-Page SEO y Jerarquía Semántica

1. **Un solo `<h1>` por página**:
   - Debe contener la propuesta de valor principal y la keyword de mayor relevancia.
2. **Jerarquía lógica de encabezados**:
   - `<h1>` seguido de `<h2>` para secciones principales, y `<h3>` para subsecciones. Nunca saltar niveles (ej. `h1` a `h4`).
3. **Metadatos Esenciales en el `<head>`**:
   ```html
   <title>[Título descriptivo de 50-60 caracteres | Marca]</title>
   <meta name="description" content="[Descripción atractiva de 140-160 caracteres con llamada a la acción]">
   <link rel="canonical" href="https://ejemplo.com/pagina-actual">
   
   <!-- OpenGraph para redes sociales -->
   <meta property="og:title" content="[Título]">
   <meta property="og:description" content="[Descripción]">
   <meta property="og:image" content="https://ejemplo.com/og-image.webp">
   <meta property="og:type" content="website">
   <meta name="twitter:card" content="summary_large_image">
   ```

---

## 3. Datos Estructurados (Schema.org / JSON-LD)

Implementar siempre marcado JSON-LD adecuado para que los motores de búsqueda muestren fragmentos enriquecidos (*Rich Snippets*):

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Nombre de la Aplicación",
  "applicationCategory": "BusinessApplication",
  "operatingSystem": "All",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD"
  }
}
</script>
```
