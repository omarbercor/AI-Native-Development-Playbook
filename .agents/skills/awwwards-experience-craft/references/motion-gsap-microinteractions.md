# Micro-Interacciones, Física Orgánica, GSAP y Cursores Magnéticos

Módulos agnósticos (`< 120 líneas` cada uno) diseñados para funcionar idénticamente en **Astro**, **WordPress (PHP/Blocks)**, **Next.js** o **HTML Vanilla** mediante atributos `data-*`.

---

## 1. Tabla de Decisión de Tiempos y Curvas (Reglas de Emil Kowalski)

| Interacción | Duración | Curva / Easing | Regla Crítica |
| :--- | :--- | :--- | :--- |
| **Feedback táctil (`:active`)** | `100ms–150ms` | `cubic-bezier(0.22, 1, 0.36, 1)` | Escalar a `scale(0.97)`, nunca sin respuesta |
| **Hover en botones/links** | `180ms–240ms` | `cubic-bezier(0.22, 1, 0.36, 1)` | Solo en `@media (hover: hover)` |
| **Cursor magnético (LERP)** | Continuo (60fps) | `gsap.quickTo(..., { duration: 0.45, ease: "power3" })` | El texto interno se mueve al 50% del contenedor |
| **Revelado de Titular (Mask)** | `750ms–1000ms` | `expo.out` o `power4.out` | Emergencia desde `yPercent: 110` con `stagger: 0.04` |
| **Acciones repetitivas (+50/día)**| `0ms–120ms` | Instantáneo o `ease-out` | Nunca animar con lentitud menús de uso frecuente |

---

## 2. Módulo 1: Smooth Scroll + GSAP Sync (`src/motion/smooth-scroll.js`)

Sincroniza **Lenis** con el ticker de **GSAP ScrollTrigger** y limpia instancias al navegar (compatible con Astro View Transitions y WordPress):

```javascript
import Lenis from 'lenis';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

let lenisInstance = null;

export function initSmoothScroll() {
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return null;

  lenisInstance?.destroy();
  lenisInstance = new Lenis({
    lerp: 0.09,
    smoothWheel: true,
    wheelMultiplier: 0.95,
  });

  lenisInstance.on('scroll', ScrollTrigger.update);
  gsap.ticker.add((time) => lenisInstance?.raf(time * 1000));
  gsap.ticker.lagSmoothing(0);

  return lenisInstance;
}

export function destroySmoothScroll() {
  lenisInstance?.destroy();
  lenisInstance = null;
}
```

---

## 3. Módulo 2: Cursor Contextual y Botones Magnéticos (`src/motion/magnetic-cursor.js`)

Se activa **únicamente en dispositivos con puntero fino** (`pointer: fine`). Cualquier botón o link con `data-magnetic="0.35"` adquiere física de atracción magnética, y `data-cursor-label="EXPLORAR"` expande el cursor:

```javascript
import gsap from 'gsap';

export function initMagneticCursor() {
  if (!window.matchMedia('(hover: hover) and (pointer: fine)').matches) return () => {};

  const cursor = document.querySelector('[data-cursor]');
  const cursorLabel = document.querySelector('[data-cursor-text]');
  const cleanups = [];

  if (cursor) {
    const xTo = gsap.quickTo(cursor, 'x', { duration: 0.35, ease: 'power3.out' });
    const yTo = gsap.quickTo(cursor, 'y', { duration: 0.35, ease: 'power3.out' });
    const onMove = (e) => { xTo(e.clientX); yTo(e.clientY); };
    window.addEventListener('mousemove', onMove, { passive: true });
    cleanups.push(() => window.removeEventListener('mousemove', onMove));
  }

  document.querySelectorAll('[data-magnetic]').forEach((el) => {
    const strength = parseFloat(el.dataset.magnetic) || 0.3;
    const inner = el.querySelector('[data-magnetic-inner]');
    const xEl = gsap.quickTo(el, 'x', { duration: 0.5, ease: 'elastic.out(1, 0.4)' });
    const yEl = gsap.quickTo(el, 'y', { duration: 0.5, ease: 'elastic.out(1, 0.4)' });

    const onMove = (e) => {
      const rect = el.getBoundingClientRect();
      const dx = (e.clientX - (rect.left + rect.width / 2)) * strength;
      const dy = (e.clientY - (rect.top + rect.height / 2)) * strength;
      xEl(dx);
      yEl(dy);
      if (inner) gsap.to(inner, { x: dx * 0.4, y: dy * 0.4, duration: 0.3 });
    };

    const onEnter = () => {
      if (cursor && el.dataset.cursorLabel && cursorLabel) {
        cursorLabel.textContent = el.dataset.cursorLabel;
        cursor.classList.add('is-expanded');
      }
    };

    const onLeave = () => {
      xEl(0);
      yEl(0);
      if (inner) gsap.to(inner, { x: 0, y: 0, duration: 0.4, ease: 'power3.out' });
      cursor?.classList.remove('is-expanded');
    };

    el.addEventListener('mousemove', onMove);
    el.addEventListener('mouseenter', onEnter);
    el.addEventListener('mouseleave', onLeave);
    cleanups.push(() => {
      el.removeEventListener('mousemove', onMove);
      el.removeEventListener('mouseenter', onEnter);
      el.removeEventListener('mouseleave', onLeave);
    });
  });

  return () => cleanups.forEach((fn) => fn());
}
```

---

## 4. Módulo 3: Revelado Editorial por Máscara (`src/motion/text-reveal.js`)

Envuelve líneas o palabras marcadas con `data-reveal="mask"` en contenedores con `overflow: hidden` para lograr el revelado monumental clásico de Awwwards:

```javascript
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

export function initTextReveals(scope = document) {
  const ctx = gsap.context(() => {
    scope.querySelectorAll('[data-reveal="mask"]').forEach((heading) => {
      const words = heading.textContent.trim().split(/\s+/);
      heading.innerHTML = words
        .map((w) => `<span class="reveal-mask"><span class="reveal-word">${w}</span></span>`)
        .join(' ');

      gsap.from(heading.querySelectorAll('.reveal-word'), {
        yPercent: 115,
        rotateZ: 2,
        duration: 0.9,
        stagger: 0.035,
        ease: 'expo.out',
        scrollTrigger: {
          trigger: heading,
          start: 'top 85%',
          once: true,
        },
      });
    });
  }, scope);

  return () => ctx.revert();
}
```

```css
/* CSS requerido para el revelado por máscara */
.reveal-mask {
  display: inline-block;
  overflow: hidden;
  vertical-align: bottom;
  padding-bottom: 0.06em;
}
.reveal-word {
  display: inline-block;
  will-change: transform;
}
```

