# Referencia de Tokens CSS y Snippets UI Premium

Este documento contiene plantillas CSS listas para copiar y usar en proyectos web modernos.

---

## 1. Plantilla de Tokens Base (Dark Mode Elegante)

```css
:root {
  /* Color Palette - HSL Tailored Dark Theme */
  --bg-canvas: hsl(222, 47%, 7%);
  --bg-surface: hsl(222, 47%, 11%);
  --bg-surface-elevated: hsl(222, 40%, 15%);
  --bg-surface-glass: hsla(222, 47%, 14%, 0.65);

  --border-subtle: hsla(220, 20%, 90%, 0.08);
  --border-highlight: hsla(220, 20%, 90%, 0.16);
  --border-accent: hsla(250, 84%, 60%, 0.4);

  --text-primary: hsl(210, 40%, 98%);
  --text-secondary: hsl(215, 20%, 75%);
  --text-muted: hsl(215, 16%, 50%);

  --accent-primary: hsl(250, 84%, 64%);
  --accent-primary-hover: hsl(250, 84%, 70%);
  --accent-glow: hsla(250, 84%, 64%, 0.25);

  --accent-gradient: linear-gradient(135deg, hsl(250, 84%, 64%), hsl(290, 84%, 60%));

  /* Fluid Typography */
  --font-sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  --font-display: 'Outfit', var(--font-sans);

  --text-xs: clamp(0.75rem, 0.7rem + 0.25vw, 0.8125rem);
  --text-sm: clamp(0.875rem, 0.825rem + 0.25vw, 0.9375rem);
  --text-base: clamp(1rem, 0.95rem + 0.25vw, 1.0625rem);
  --text-lg: clamp(1.125rem, 1.05rem + 0.35vw, 1.25rem);
  --text-xl: clamp(1.25rem, 1.15rem + 0.5vw, 1.5rem);
  --text-2xl: clamp(1.5rem, 1.35rem + 0.75vw, 2rem);
  --text-3xl: clamp(2rem, 1.75rem + 1.25vw, 2.75rem);
  --text-4xl: clamp(2.5rem, 2.1rem + 2vw, 3.75rem);

  /* Spacing Scale */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-12: 3rem;
  --space-16: 4rem;

  /* Border Radii */
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 16px;
  --radius-full: 9999px;

  /* Transitions & Shadows */
  --transition-fast: 150ms cubic-bezier(0.16, 1, 0.3, 1);
  --transition-normal: 250ms cubic-bezier(0.16, 1, 0.3, 1);
  --transition-slow: 400ms cubic-bezier(0.16, 1, 0.3, 1);

  --shadow-sm: 0 2px 8px rgba(0, 0, 0, 0.2);
  --shadow-md: 0 8px 24px -4px rgba(0, 0, 0, 0.35);
  --shadow-glow: 0 0 30px var(--accent-glow);
}
```

---

## 2. Componente de Tarjeta Glassmorphic

```css
.glass-card {
  background: var(--bg-surface-glass);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-md);
  transition: transform var(--transition-normal), border-color var(--transition-normal), box-shadow var(--transition-normal);
}

.glass-card:hover {
  transform: translateY(-4px);
  border-color: var(--border-highlight);
  box-shadow: var(--shadow-md), var(--shadow-glow);
}
```

---

## 3. Botón de Acción Principal (Efecto Micro-Interactivo)

```css
.btn-primary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
  padding: var(--space-3) var(--space-6);
  font-family: var(--font-sans);
  font-size: var(--text-sm);
  font-weight: 600;
  color: #ffffff;
  background: var(--accent-gradient);
  border: 1px solid hsla(0, 0%, 100%, 0.15);
  border-radius: var(--radius-md);
  cursor: pointer;
  box-shadow: var(--shadow-sm), 0 0 15px var(--accent-glow);
  transition: transform var(--transition-fast), filter var(--transition-fast), box-shadow var(--transition-fast);
}

.btn-primary:hover {
  transform: scale(1.02);
  filter: brightness(1.1);
  box-shadow: var(--shadow-md), 0 0 25px var(--accent-glow);
}

.btn-primary:active {
  transform: scale(0.98);
}
```
