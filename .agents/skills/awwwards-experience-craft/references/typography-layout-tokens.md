# Tipografía Monumental, Layouts de Espacio Negativo y Texturas Orgánicas

Referencia técnica para construir la base visual de un sitio de experiencia nivel **Awwwards**: tipografía de impacto arquitectónico, grillas asimétricas con espacio negativo activo y texturas de fondo orgánicas.

---

## 1. Combinaciones Tipográficas con Carácter (Prohibido el "Default AI")

Evita usar *Inter* o *Roboto* para encabezados. Selecciona una dupla o tríada según la personalidad de la marca:

| Personalidad | Fuente Display (Impacto) | Fuente Cuerpo (Lectura) | Fuente Técnica (Índices / Meta) |
| :--- | :--- | :--- | :--- |
| **Neo-Brutalista / Suiza** | *Syne*, *Space Grotesk* o *Cabinet Grotesk* | *Plus Jakarta Sans* o *DM Sans* | *JetBrains Mono* o *Space Mono* |
| **Editorial / Monolítica** | *Instrument Serif*, *Playfair Display* o *Cormorant* | *General Sans* o *Outfit* | *IBM Plex Mono* |
| **Cinética / Vanguardia** | *Clash Display*, *Unbounded* o *Archivo Black* | *Manrope* o *Satoshi* | *Fira Code* |

---

## 2. Sistema de Tokens (`tokens.css` < 100 líneas)

Escala tipográfica extrema con `clamp()` para que un titular domine el viewport en escritorio (`10vw+`) sin romper una pantalla móvil de `375px`:

```css
:root {
  /* Paleta Base: Carbón Cálido + Marfil Crudo (Nunca #000 ni #FFF puros) */
  --bg-primary: #0b0b0a;
  --bg-elevated: #141412;
  --text-primary: #f2efe9;
  --text-muted: rgba(242, 239, 233, 0.54);
  --accent-raw: #e8ff00; /* O terracota #ff4d29 / cobalto #2b4eff según marca */
  --border-hairline: rgba(242, 239, 233, 0.12);

  /* Familias Tipográficas */
  --font-display: 'Syne', sans-serif;
  --font-sans: 'Plus Jakarta Sans', sans-serif;
  --font-mono: 'Space Mono', monospace;

  /* Escala Tipográfica Monumental (Fluid Clamp) */
  --text-mega: clamp(3.25rem, 10.5vw + 0.5rem, 11.5rem); /* Hero Display */
  --text-display: clamp(2.25rem, 5.5vw + 0.5rem, 6rem);  /* Títulos de Sección */
  --text-editorial: clamp(1.35rem, 2.2vw + 0.4rem, 2.5rem); /* Manifiesto / Lead */
  --text-body: clamp(1rem, 0.4vw + 0.9rem, 1.1875rem);
  --text-meta: 0.75rem; /* 12px fijo para etiquetas técnicas monospace */

  /* Espaciado de Espacio Negativo */
  --space-section: clamp(6rem, 16vh, 14rem);
  --gutter: clamp(1.25rem, 4vw, 4rem);

  /* Curvas Físicas (Estándar Emil Kowalski) */
  --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-out-quart: cubic-bezier(0.22, 1, 0.36, 1);
  --ease-in-out-cubic: cubic-bezier(0.77, 0, 0.175, 1);
}
```

---

## 3. Reglas de Composición Tipográfica y Espacio Negativo

```css
/* Titular Monumental: interlínea comprimida y tracking negativo */
.title-monumental {
  font-family: var(--font-display);
  font-size: var(--text-mega);
  font-weight: 800;
  line-height: 0.88;
  letter-spacing: -0.045em;
  text-transform: uppercase;
  color: var(--text-primary);
  overflow-wrap: break-word;
}

/* Índice Técnico Brutalista / Editorial: [ 01 — CONCEPTO ] */
.meta-index {
  font-family: var(--font-mono);
  font-size: var(--text-meta);
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text-muted);
  font-variant-numeric: tabular-nums;
}

/* Grilla Asimétrica de 12 Columnas con Espacio Negativo Activo */
.experience-grid {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr));
  gap: clamp(1rem, 2vw, 2rem);
  padding-inline: var(--gutter);
  padding-block: var(--space-section);
  border-bottom: 1px solid var(--border-hairline);
}

/* En móvil: colapsa limpiamente sin perder jerarquía */
@media (max-width: 768px) {
  .experience-grid {
    grid-template-columns: repeat(4, minmax(0, 1fr));
  }
}
```

### Patrón de Distribución Asimétrica (Anti-Slop)
En lugar de centrar el contenido, distribuye el peso visual para que el vacío respire:
- **Columna 1–2**: Índice técnico `.meta-index` (`[ 01 / ARQUITECTURA ]`) anclado arriba con `position: sticky; top: 2rem`.
- **Columna 3–5**: Vacío intencional (espacio negativo puro).
- **Columna 6–12**: Titular `.title-editorial` o bloque interactivo principal.

---

## 4. Textura de Fondo Orgánica (Grano Reactivo + Atmósfera)

Un overlay de ruido fractal SVG ultraligero que elimina el aspecto plano digital sin consumir GPU:

```css
/* Capa de grano orgánico fijo en toda la experiencia */
.organic-grain {
  position: fixed;
  inset: 0;
  width: 100%;
  height: 100dvh;
  pointer-events: none;
  z-index: 9998;
  opacity: 0.045;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
```

