---
name: web-frontend-craft
description: >-
  High-precision functional UI/UX engineering for web apps, SaaS, dashboards, e-commerce,
  corporate portals, and CMS sites (WordPress, Astro, React). Use this skill when building
  or refactoring task-oriented interfaces, design systems, forms, tables, editorial/blog
  layouts, mobile-native web experiences, and accessible components (WCAG 2.2 AA).
---

# Web Frontend Craft — Ingeniería de Interfaces Funcionales, Producto y CMS (10/10)

Esta habilidad guía la construcción de **interfaces funcionales de precisión quirúrgica** (inspirada en Linear, Stripe, Apple HIG, Emil Kowalski, Taste Skill `app-screens` e Impeccable). Aquí la interfaz **no compite con la tarea del usuario**: elimina la fricción, transmite confianza inmediata y se siente tan fluida como una app nativa.

> [!IMPORTANT]
> **Cuándo usar este skill vs. `awwwards-experience-craft`:**
> - Usa **`web-frontend-craft`** para **sitios funcionales, portales/blogs en WordPress o Astro, e-commerce, SaaS, formularios, paneles y dashboards** donde mandan la claridad, conversión, lectura y velocidad operativa.
> - Usa **`awwwards-experience-craft`** cuando construyas **sitios de experiencia inmersiva, portafolios o lanzamientos creativos** con tipografía monumental, cursores magnéticos y 3D/WebGL.

---

## 1. Reglas Anti-Slop para Interfaces Funcionales

Los LLMs suelen disfrazar interfaces mediocres con adornos genéricos. Queda **prohibido**:

1. **El Síndrome "AI SaaS"**: Abusar de gradientes morado/neón, resplandores (`glow`) innecesarios o *glassmorphism* pesado detrás de tablas, formularios o textos largos.
2. **Íconos en Cuadritos de Colores**: Poner un ícono dentro de un cuadrado redondeado de color pastel sobre cada bloque de texto. Si el ícono no comunica estado o acción única, elimínalo.
3. **Tarjetas dentro de Tarjetas (*Card-in-Card*)**: Anidar cajas con bordes y sombras dentro de otras cajas. Usa **espaciado, alineación y divisores sutiles de `1px`** antes de encerrar todo en tarjetas.
4. **Bordes Sólidos Toscos y Negro Puro**: Reemplaza bordes grises pesados por sombras perimetrales semitransparentes (`0 0 0 1px rgba(...)`) que se adaptan a cualquier fondo, y usa neutros ligeramente matizados en lugar de `#000000` puro.
5. **Color como Decoración**: En dashboards, tiendas y portales, **el color es semántico**. Resérvalo para la acción primaria (CTA), indicadores de estado (éxito, alerta, error) y series de datos.

---

## 2. Jerarquía Visual, Tipografía Funcional y Contenido CMS

1. **Tipografía de Lectura y Datos**:
   - **Números Tabulares Obligatorios**: Aplica siempre `font-variant-numeric: tabular-nums` en precios, carritos, tablas, fechas, contadores y KPIs para que las columnas nunca "bailen" al actualizarse.
   - **Balance y Ritmo**: Usa `text-wrap: balance` en títulos (`h1`–`h3`) para evitar líneas huérfanas de una sola palabra, y `text-wrap: pretty` con ancho máximo de `65ch` en artículos/contenido de CMS.
2. **Estilos de Contenido Rico (`.prose` para WordPress / Markdown)**:
   - En sitios conectados a un CMS, el editor inyectará `<h2>`, listas, citas, tablas e imágenes de cualquier proporción. Define siempre un contenedor `.cms-prose` aislado que estilice etiquetas nativas con márgenes verticales consistentes y `aspect-ratio` seguro.
3. **Elevación Silenciosa (3 Niveles Máximo)**:
   - `Canvas` (fondo base) → `Surface` (panel o contenedor principal) → `Overlay` (dropdown, modal o popover flotante).

---

## 3. Micro-Interacciones por Frecuencia de Uso (Estándar Emil Kowalski)

En interfaces funcionales, animar de más degrada la productividad. Decide según cuántas veces al día ocurre la acción:

| Frecuencia de Uso | Ejemplos | Duración / Regla |
| :--- | :--- | :--- |
| **100+ veces/día** | Atajos de teclado, cambio de pestañas, filtros rápidos, command palette | **`0ms` (Instantáneo)** o `< 100ms`. Prohibido animar entradas lentas. |
| **10–50 veces/día** | Botones (`:active`), checkboxes, switches, filas de tabla | **`100ms–160ms`**. Feedback inmediato (`scale(0.97)` al presionar). |
| **Ocasional** | Dropdowns, popovers, modales, drawers móviles, toasts | **`160ms–240ms`** con `cubic-bezier(0.23, 1, 0.32, 1)` (`ease-out` fuerte). |

- **Prohibido `transition: all` y `ease-in`**: Especifica propiedades exactas (`transform`, `opacity`, `background-color`). Nunca uses `ease-in` al mostrar un elemento (se siente lento).
- **Origen Espacial Real**: Los popovers y menús desplegables escalan desde `scale(0.96)` (nunca desde `0`) con `transform-origin` anclado al botón que los abrió, no al centro de la pantalla.

---

## 4. Estándar `mobile-native` (Que la Web se Sienta App Nativa)

Toda interfaz funcional debe superar estos 5 puntos obligatorios en teléfonos reales:

1. **Cero Auto-Zoom en iOS**: Todo `<input>`, `<select>` y `<textarea>` debe tener `font-size: max(16px, 1rem)` en móvil (valores menores fuerzan a Safari iOS a hacer zoom y descolocar el layout).
2. **Guardia contra "Sticky Hover"**: Envuelve todos los efectos `:hover` en `@media (hover: hover) and (pointer: fine)` para que no queden pegados tras tocar la pantalla.
3. **Viewport Dinámico y Safe Areas**: Usa `100dvh` en cajones/modales móviles y `padding-bottom: max(1rem, env(safe-area-inset-bottom))` en barras fijas inferiores o botones de pago.
4. **Respuesta Táctil Limpia**: Aplica `touch-action: manipulation` y `-webkit-tap-highlight-color: transparent` en controles interactivos, garantizando un área táctil mínima de **`44x44px`**.
5. **Tablas y Filtros en `375px`**: Las tablas anchas usan contenedor con `overflow-x: auto` + primera columna fija o se transforman limpiamente en listas estructuradas, jamás rompen el ancho de página.

---

## 5. Los 5 Estados Obligatorios y Blindaje (`harden` / `break-ui`)

Ninguna pantalla dinámica, listado de productos o bloque CMS se entrega solo con el "camino feliz":

1. **Loading (Sin CLS)**: Skeletons con las dimensiones exactas del contenido final; prohibido que la página salte al terminar de cargar.
2. **Empty State Útil**: Si no hay resultados, pedidos o posts, explica por qué en 1 línea y ofrece la acción directa para empezar o limpiar filtros.
3. **Edge Data (`break-ui`)**: Soporta nombres de 80 caracteres (`truncate` / `line-clamp-2`), precios de 7 cifras, 0 comentarios e imágenes rotas (`onerror` / fallback limpio).
4. **Formularios Honestos**: `<label>` siempre visible, `autocomplete` correcto (`email`, `tel`, `current-password`), botón bloqueado durante el envío (`aria-busy="true"`) para impedir cobros o envíos duplicados, y errores específicos junto al campo.
5. **Accesibilidad Teclado (WCAG 2.2 AA)**: Navegación completa con `Tab`/`Escape`, contraste mínimo `4.5:1` y anillo `:focus-visible` nítido.

---

## 6. Arquitectura Modular (< 200 Líneas por Archivo)

Mantén el código limpio, reutilizable y desacoplado tanto en **Astro**, **WordPress** como **React**:

```text
src/ (o tema/plugin CMS)
├── styles/
│   ├── tokens.css           ← Paleta OKLCH/HSL (Light/Dark), escala 4px/8px, radios (< 120 líneas)
│   └── cms-prose.css        ← Estilos seguros para contenido dinámico de editores CMS (< 120 líneas)
├── components/ui/           ← Primitivos atómicos sin lógica de negocio (< 120 líneas c/u)
│   ├── Button.*             ← Variantes primary, secondary, ghost, danger + loading state
│   ├── Field.*              ← Label + Input (>=16px mobile) + Hint + Error accesible
│   └── Dialog.*             ← Modal/Drawer accesible con cierre Escape y focus trap
└── features/ (o template-parts/)
    └── [modulo]/            ← Vistas y controladores divididos por responsabilidad (< 180 líneas)
```

---

## 7. Referencias Listas para Producción

- 🎨 **Sistema de Tokens (Light/Dark), Bordes Sutiles y `.cms-prose`**: [tokens_and_styles.md](./references/tokens_and_styles.md)
- 📱 **Patrones `mobile-native`, Formularios Blindados y Estados UI**: [mobile-native-and-states.md](./references/mobile-native-and-states.md)
