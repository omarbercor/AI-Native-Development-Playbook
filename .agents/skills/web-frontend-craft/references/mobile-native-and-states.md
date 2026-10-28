# Patrones `mobile-native`, Formularios Blindados y Estados de UI

Soluciones concretas a los fallos sutiles que separan una web amateur de una aplicación profesional en dispositivos móviles y escritorio.

---

## 1. Fixes Críticos `mobile-native` (`mobile.css`)

```css
/* 1. Evita el zoom automático de Safari iOS al enfocar un campo */
input, select, textarea {
  font-size: max(16px, 1rem);
  font-family: inherit;
  color: var(--text-primary);
  background: var(--bg-surface);
  border-radius: var(--radius-sm);
  box-shadow: var(--ring-hairline);
  border: none;
  padding: 0.625rem 0.75rem;
  width: 100%;
  transition: box-shadow var(--dur-ui) var(--ease-snappy);
}

@media (min-width: 768px) {
  input, select, textarea { font-size: 0.875rem; }
}

input:focus-visible, select:focus-visible, textarea:focus-visible {
  outline: none;
  box-shadow: 0 0 0 2px var(--focus-ring);
}

input[aria-invalid="true"] {
  box-shadow: 0 0 0 2px var(--status-danger);
}

/* 2. Expansión invisible de zona táctil a >=44px para botones de ícono pequeños */
.icon-hit-target {
  position: relative;
  display: inline-grid;
  place-items: center;
}
.icon-hit-target::after {
  content: '';
  position: absolute;
  inset: -8px;
}

/* 3. Barras inferiores y Drawers respetando barra gestual y teclado móvil */
.mobile-sticky-bar {
  position: sticky;
  bottom: 0;
  background: var(--bg-surface);
  box-shadow: 0 -1px 0 rgba(0, 0, 0, 0.08);
  padding: 0.75rem 1rem calc(0.75rem + env(safe-area-inset-bottom));
}

.fullscreen-drawer {
  min-height: 100dvh; /* Nunca 100vh: evita cortes con la barra del navegador */
}
```

---

## 2. Animación de Popovers y Modales con Origen Real

Nunca animes un menú desplegable desde `scale(0)` ni con `ease-in`. Escala desde `0.96` respetando el punto de origen:

```css
.popover-content {
  transform-origin: var(--popover-origin, top left);
  animation: popover-in 160ms var(--ease-snappy) forwards;
}

@keyframes popover-in {
  from {
    opacity: 0;
    transform: scale(0.96) translateY(-4px);
  }
  to {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}

@media (prefers-reduced-motion: reduce) {
  .popover-content { animation: none; }
}
```

---

## 3. Patrón de Formulario Accesible y Anti-Doble-Submit

Estructura HTML semántica obligatoria para cualquier formulario de contacto, checkout o login:

```html
<form id="checkout-form" novalidate>
  <div class="field-group">
    <label for="user-email">Correo electrónico</label>
    <input
      id="user-email"
      name="email"
      type="email"
      autocomplete="email"
      inputmode="email"
      required
      aria-describedby="email-error"
    />
    <p id="email-error" class="field-error" role="alert" hidden></p>
  </div>

  <button type="submit" class="btn" id="submit-btn">
    <span data-label>Confirmar pedido</span>
  </button>
</form>
```

```javascript
// Guardia reutilizable contra doble envío (< 25 líneas)
export function bindSafeSubmit(form, onSubmit) {
  let submitting = false;
  form.addEventListener('submit', async (e) => {
    e.preventDefault();
    if (submitting) return;
    const btn = form.querySelector('[type="submit"]');
    submitting = true;
    btn?.setAttribute('aria-busy', 'true');
    try {
      await onSubmit(new FormData(form));
    } finally {
      submitting = false;
      btn?.removeAttribute('aria-busy');
    }
  });
}
```

---

## 4. Checklist Rápido `break-ui` antes de Entregar

Prueba mentalmente (o en el navegador) estos 5 casos extremos en cada vista funcional:
1. **Texto extremo**: Un título o nombre de producto de 90 caracteres sin espacios o en alemán/español largo → usa `min-width: 0` en hijos de Flex/Grid + `overflow-wrap: break-word` o `line-clamp`.
2. **Lista vacía (0 ítems)**: Muestra estado vacío claro con CTA primario, nunca una tabla con cabeceras huérfanas.
3. **Volumen alto (500+ ítems)**: Paginación o carga incremental; nunca renderizar miles de nodos pesados de golpe.
4. **Imagen caída (`404`)**: Contenedor con `aspect-ratio` fijo y color `--bg-subtle` para que no colapse ni deforme la grilla.
5. **Red lenta (`3G`)**: Botones muestran estado `aria-busy` al instante para evitar clics desesperados.

