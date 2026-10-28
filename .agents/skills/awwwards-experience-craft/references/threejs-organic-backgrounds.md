# Fondos Orgánicos Interactivos y 3D con Three.js / Shaders (60 FPS)

Guía de arquitectura para integrar **Three.js / WebGL** y fondos orgánicos interactivos en sitios de experiencia sin sacrificar el rendimiento, la batería móvil ni el límite de `< 200 líneas` por archivo.

---

## 1. Matriz de Justificación 3D (¿Cuándo usar WebGL?)

Antes de instalar o instanciar Three.js, evalúa qué técnica logra el mayor impacto con el menor costo:

| Objetivo Visual | Técnica Recomendada | Costo GPU |
| :--- | :--- | :--- |
| Grano cinematográfico + atmósfera sutil | Overlay SVG `feTurbulence` + CSS radial-gradient animado | Muy Bajo |
| Fondo orgánico líquido / topografía reactiva al mouse | **Plano 2D en Three.js / WebGL con Fragment Shader (Simplex Noise)** | Bajo–Medio (60fps estables) |
| Objeto central escultórico que reacciona al scroll/mouse | **Escena Three.js con geometría procedural o GLTF comprimido (Draco)** | Medio |
| Distorsión orgánica de fotografías al hacer scroll/hover | **Plano Three.js con textura de imagen + desplazamiento UV por velocidad** | Medio |

> [!WARNING]
> **Regla Invariante de Rendimiento 3D**:
> 1. Limita siempre el pixel ratio: `renderer.setPixelRatio(Math.min(window.devicePixelRatio, 1.75))` (`1.25` en móviles).
> 2. **Pausa el loop de `requestAnimationFrame`** cuando el canvas no esté en el viewport usando `IntersectionObserver` y cuando la pestaña esté inactiva (`document.hidden`).
> 3. **Limpia la memoria GPU** (`geometry.dispose()`, `material.dispose()`, `renderer.dispose()`) al cambiar de ruta.

---

## 2. Módulo Reutilizable: Fondo Orgánico Reactivo con Shader (`src/canvas/organic-bg.js`)

Este módulo (`< 130 líneas`) crea una superficie orgánica fluida en **Three.js** que responde suavemente a la posición del cursor (con LERP) y al tiempo, ideal para detrás de tipografía monumental en **Astro**, **WordPress** o **React**:

```javascript
import * as THREE from 'three';

const VERTEX_SHADER = `
  varying vec2 vUv;
  void main() {
    vUv = uv;
    gl_Position = vec4(position, 1.0);
  }
`;

const FRAGMENT_SHADER = `
  uniform float uTime;
  uniform vec2 uMouse;
  uniform vec2 uResolution;
  varying vec2 vUv;

  // Ruido orgánico ligero sin texturas externas
  float hash(vec2 p) { return fract(sin(dot(p, vec2(127.1, 311.7))) * 43758.5453123); }
  float noise(vec2 p) {
    vec2 i = floor(p), f = fract(p);
    vec2 u = f * f * (3.0 - 2.0 * f);
    return mix(mix(hash(i), hash(i + vec2(1.0, 0.0)), u.x),
               mix(hash(i + vec2(0.0, 1.0)), hash(i + vec2(1.0, 1.0)), u.x), u.y);
  }

  void main() {
    vec2 aspect = vec2(uResolution.x / uResolution.y, 1.0);
    vec2 uv = vUv * aspect;
    vec2 mouse = uMouse * aspect;

    float dist = length(uv - mouse);
    float interaction = smoothstep(0.65, 0.0, dist) * 0.35;

    float n1 = noise(uv * 2.8 + uTime * 0.12 + interaction);
    float n2 = noise(uv * 5.5 - uTime * 0.08 + n1 * 1.4);
    float pattern = smoothstep(0.2, 0.85, n2);

    vec3 baseDark = vec3(0.043, 0.043, 0.039);   // #0b0b0a
    vec3 warmTone = vec3(0.12, 0.115, 0.105);    // Tono piedra orgánica
    vec3 accentGlow = vec3(0.91, 1.0, 0.0);      // Pulso sutil en proximidad del cursor

    vec3 color = mix(baseDark, warmTone, pattern);
    color += accentGlow * interaction * 0.08;

    gl_FragColor = vec4(color, 1.0);
  }
`;

export function mountOrganicBackground(container) {
  if (!container || window.matchMedia('(prefers-reduced-motion: reduce)').matches) return () => {};

  const scene = new THREE.Scene();
  const camera = new THREE.OrthographicCamera(-1, 1, 1, -1, 0, 1);
  const renderer = new THREE.WebGLRenderer({ alpha: false, antialias: false, powerPreference: 'high-performance' });

  const isMobile = window.innerWidth < 768;
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, isMobile ? 1.25 : 1.75));
  renderer.setSize(container.clientWidth, container.clientHeight);
  container.appendChild(renderer.domElement);

  const targetMouse = new THREE.Vector2(0.5, 0.5);
  const currentMouse = new THREE.Vector2(0.5, 0.5);

  const uniforms = {
    uTime: { value: 0 },
    uMouse: { value: currentMouse },
    uResolution: { value: new THREE.Vector2(container.clientWidth, container.clientHeight) },
  };

  const geometry = new THREE.PlaneGeometry(2, 2);
  const material = new THREE.ShaderMaterial({ vertexShader: VERTEX_SHADER, fragmentShader: FRAGMENT_SHADER, uniforms });
  scene.add(new THREE.Mesh(geometry, material));

  let isVisible = true;
  let rafId = null;
  const clock = new THREE.Clock();

  const renderLoop = () => {
    if (!isVisible || document.hidden) { rafId = null; return; }
    uniforms.uTime.value = clock.getElapsedTime();
    currentMouse.lerp(targetMouse, 0.06); // Amortiguación orgánica LERP
    renderer.render(scene, camera);
    rafId = requestAnimationFrame(renderLoop);
  };

  const observer = new IntersectionObserver(([entry]) => {
    isVisible = entry.isIntersecting;
    if (isVisible && !rafId) rafId = requestAnimationFrame(renderLoop);
  });
  observer.observe(container);

  const onPointerMove = (e) => {
    const rect = container.getBoundingClientRect();
    targetMouse.set((e.clientX - rect.left) / rect.width, 1.0 - (e.clientY - rect.top) / rect.height);
  };

  const onResize = () => {
    renderer.setSize(container.clientWidth, container.clientHeight);
    uniforms.uResolution.value.set(container.clientWidth, container.clientHeight);
  };

  window.addEventListener('pointermove', onPointerMove, { passive: true });
  window.addEventListener('resize', onResize, { passive: true });
  rafId = requestAnimationFrame(renderLoop);

  return () => {
    if (rafId) cancelAnimationFrame(rafId);
    observer.disconnect();
    window.removeEventListener('pointermove', onPointerMove);
    window.removeEventListener('resize', onResize);
    geometry.dispose();
    material.dispose();
    renderer.dispose();
    renderer.domElement.remove();
  };
}
```

---

## 3. Integración Limpia en Astro o WordPress (`< 30 líneas`)

En un componente `.astro` o script de tema WordPress, monta y limpia el canvas sin ensuciar el HTML:

```html
<section class="hero-experience" style="position: relative; min-height: 100dvh; overflow: hidden;">
  <div id="organic-canvas" style="position: absolute; inset: 0; z-index: 0;" aria-hidden="true"></div>
  <div class="experience-grid" style="position: relative; z-index: 1;">
    <span class="meta-index">[ 01 — EXPERIENCIA DIGITAL ]</span>
    <h1 class="title-monumental" data-reveal="mask">ARQUITECTURA VIVA</h1>
  </div>
</section>

<script>
  import { mountOrganicBackground } from '../canvas/organic-bg.js';
  let cleanup = mountOrganicBackground(document.getElementById('organic-canvas'));
  document.addEventListener('astro:before-swap', () => cleanup?.(), { once: true });
</script>
```

