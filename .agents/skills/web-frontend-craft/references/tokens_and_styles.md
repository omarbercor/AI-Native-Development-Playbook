# Tokens Funcionales (Light/Dark), Superficies Silenciosas y Contenido CMS

Plantillas CSS modernas y libres de "AI Slop" para aplicaciones web, tiendas, portales corporativos y sitios gestionados con CMS (WordPress / Astro / Headless).

---

## 1. Sistema de Tokens Calibrado (`tokens.css` < 110 líneas)

Diseñado con neutros matizados, contraste WCAG AA verificado y soporte limpio para modo claro y oscuro:

```css
:root {
  /* Superficies Modo Claro (Limpio, tipo Linear / Stripe) */
  --bg-canvas: #fafafa;
  --bg-surface: #ffffff;
  --bg-subtle: #f4f4f5;

  /* Texto con jerarquía real (Nunca gris ilegible) */
  --text-primary: #111113;
  --text-secondary: #52525b;
  --text-muted: #71717a;

  /* Acción Primaria y Semántica (Color reservado a intención) */
  --brand-solid: #18181b;       /* Reemplazar por color de marca calibrado */
  --brand-solid-hover: #27272a;
  --brand-contrast: #ffffff;
  --focus-ring: #2563eb;

  --status-success: #15803d;
  --status-warning: #b45309;
  --status-danger: #b91c1c;

  /* Bordes por Sombra Perimetral (Se mezclan mejor que border sólido) */
  --ring-hairline: 0 0 0 1px rgba(17, 17, 19, 0.08);
  --ring-hover: 0 0 0 1px rgba(17, 17, 19, 0.16);
  --shadow-card: var(--ring-hairline), 0 1px 2px rgba(17, 17, 19, 0.04);
  --shadow-overlay: var(--ring-hairline), 0 12px 32px -8px rgba(17, 17, 19, 0.14);

  /* Tipografía Funcional */
  --font-sans: 'Plus Jakarta Sans', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --font-mono: 'JetBrains Mono', ui-monospace, SFMono-Regular, monospace;

  /* Escala Modular y Radios Coherentes (Outer radius = Inner + Padding) */
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 14px;

  /* Curvas Rápidas de Alta Respuesta (< 240ms) */
  --ease-snappy: cubic-bezier(0.23, 1, 0.32, 1);
  --dur-press: 120ms;
  --dur-ui: 180ms;
}

[data-theme="dark"] {
  --bg-canvas: #0c0c0e;
  --bg-surface: #141417;
  --bg-subtle: #1c1c21;

  --text-primary: #f4f4f6;
  --text-secondary: #a1a1aa;
  --text-muted: #71717a;

  --brand-solid: #f4f4f6;
  --brand-solid-hover: #e4e4e7;
  --brand-contrast: #0c0c0e;
  --focus-ring: #60a5fa;

  --ring-hairline: 0 0 0 1px rgba(255, 255, 255, 0.08);
  --ring-hover: 0 0 0 1px rgba(255, 255, 255, 0.16);
  --shadow-card: var(--ring-hairline), 0 2px 6px rgba(0, 0, 0, 0.35);
  --shadow-overlay: var(--ring-hairline), 0 16px 40px -8px rgba(0, 0, 0, 0.6);
}
```

---

## 2. Superficies, Datos Tabulares y Botones Táctiles

```css
/* Panel limpio sin "Card-in-Card" innecesario */
.surface-panel {
  background: var(--bg-surface);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-card);
  padding: 1.25rem;
}

/* Obligatorio para precios, métricas, cantidades y tablas */
.tabular-data {
  font-variant-numeric: tabular-nums;
  letter-spacing: -0.01em;
}

/* Botón funcional de respuesta inmediata */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  min-height: 2.5rem;
  padding: 0.5rem 1rem;
  font-family: var(--font-sans);
  font-size: 0.875rem;
  font-weight: 600;
  border-radius: var(--radius-sm);
  border: none;
  background: var(--brand-solid);
  color: var(--brand-contrast);
  cursor: pointer;
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
  transition: transform var(--dur-press) var(--ease-snappy),
              background-color var(--dur-ui) var(--ease-snappy);
}

@media (hover: hover) and (pointer: fine) {
  .btn:hover { background: var(--brand-solid-hover); }
}

.btn:active { transform: scale(0.97); }
.btn:focus-visible { outline: 2px solid var(--focus-ring); outline-offset: 2px; }
.btn[aria-busy="true"], .btn:disabled { opacity: 0.6; pointer-events: none; }
```

---

## 3. Contenedor Seguro para Contenido Dinámico CMS (`.cms-prose`)

Ideal para entradas de WordPress (`the_content()`), bloques Gutenberg o Markdown en Astro sin que imágenes o tablas desborden el layout:

```css
.cms-prose {
  max-width: 66ch;
  color: var(--text-primary);
  line-height: 1.7;
  text-wrap: pretty;
}

.cms-prose :is(h1, h2, h3, h4) {
  text-wrap: balance;
  letter-spacing: -0.025em;
  line-height: 1.2;
  margin-top: 2em;
  margin-bottom: 0.6em;
}

.cms-prose img, .cms-prose video, .cms-prose iframe {
  max-width: 100%;
  height: auto;
  border-radius: var(--radius-md);
  margin-block: 1.5rem;
}

.cms-prose table {
  display: block;
  width: 100%;
  overflow-x: auto;
  border-collapse: collapse;
  font-variant-numeric: tabular-nums;
}
```
