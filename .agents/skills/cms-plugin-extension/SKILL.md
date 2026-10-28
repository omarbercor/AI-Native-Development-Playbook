---
name: cms-plugin-extension
description: >-
  Clean extension, custom theme, Gutenberg block, and Headless CMS specialist (WordPress + Astro,
  Shopify, etc.). Use this skill when developing custom plugins, child/block themes, REST/WPGraphQL
  endpoints, or headless CMS integrations ensuring zero core modifications, strict security nonces,
  conditional asset loading, and modular files (< 150 lines).
---

# CMS & Plugin Extension — WordPress Moderno, Bloques y Headless con Astro (10/10)

Esta habilidad garantiza que cualquier desarrollo sobre **WordPress u otro CMS** (ya sea monolítico con tema custom/bloques o **Headless conectado a Astro**) sea **100% modular, rápido, seguro y compatible con futuras actualizaciones**, sin tocar jamás el núcleo.

---

## 1. La Ley Inviolable: Cero Modificaciones al Core

> [!CAUTION]
> **PROHIBICIÓN ABSOLUTA**: Jamás edites ni añadas código en `wp-admin/`, `wp-includes/`, archivos raíz del core ni `vendor/`. Todo vive en `wp-content/plugins/[mi-plugin]/`, `wp-content/themes/[mi-tema]/` o `wp-content/mu-plugins/`.

---

## 2. Arquitectura Modular (< 150 Líneas por Archivo)

Prohibido el clásico `functions.php` o `mi-plugin.php` de 1,500 líneas. Usa el archivo raíz únicamente como **orquestador de carga** y divide cada responsabilidad:

```text
mi-plugin-o-tema/
├── functions.php (o mi-plugin.php)  ← Bootstrap limpio y constantes (< 70 líneas)
├── inc/
│   ├── PostTypes/                   ← Registro de CPTs y taxonomías (`show_in_rest: true`)
│   ├── Api/                         ← Endpoints custom `register_rest_route` y webhooks
│   ├── Blocks/                      ← Registro de bloques nativos (`block.json`) o ACF Blocks
│   ├── Assets/                      ← Encolado condicional (`wp_enqueue_scripts`)
│   └── Security/                    ← Nonces, capabilities y sanitización
├── blocks/
│   └── hero-experience/             ← `block.json` + `render.php` + script modular aislado
└── uninstall.php                    ← Limpieza explícita al borrar el plugin
```

- **Namespaces y Prefijos Obligatorios**: Toda clase PHP usa `namespace MiMarca\Modulo;` y toda opción/meta/hook global lleva prefijo único (`mimarca_`).

---

## 3. Desarrollo Moderno: Bloques Nativos y Puente Astro ↔ WordPress

1. **Cero Page-Builders Pesados cuando se busca Rendimiento**:
   - Prioriza **Bloques Nativos (`block.json` API v3)** o **ACF Blocks** ligeros con plantillas `render.php` limpias que produzcan HTML semántico compatible con nuestros perfiles `awwwards-experience-craft` o `web-frontend-craft`.
2. **Carga Condicional de Scripts Pesados (GSAP / Three.js)**:
   - **Nunca** encoles GSAP, Lenis o Three.js en todas las páginas del sitio si solo se usan en bloques o plantillas específicas. Decláralos en `block.json` (`viewScriptModule`) o verifica con `has_block()` / `is_page_template()` antes de encolar.
3. **WordPress como Headless CMS para Astro**:
   - Al exponer CPTs o campos personalizados hacia Astro (vía **WP REST API** o **WPGraphQL**), filtra la respuesta para exponer **únicamente los campos públicos necesarios** (nunca expongas emails de autores, IDs internos sensibles ni borradores).
   - Configura un hook en `save_post` / `transition_post_status` con debounce para disparar el webhook de rebuild o invalidación de caché de Astro en CloudPanel solo cuando el estado cambie a `publish`.

---

## 4. Seguridad Estricta de CMS (Zero-Trust)

1. **Permisos y Nonces en Cada Entrada**:
   - Formularios/AJAX: `check_admin_referer('mimarca_nonce', 'security')` + `current_user_can('...')`.
   - Endpoints REST (`register_rest_route`): **Prohibido usar `'permission_callback' => '__return_true'`** en rutas que muten datos o expongan información privada. Define siempre validación de permisos y `args` con `sanitize_callback` y `validate_callback`.
2. **Entrada Sanitizada, Salida Escapada, SQL Preparado**:
   - **Input**: `sanitize_text_field()`, `sanitize_email()`, `absint()`, `wp_kses_post()`.
   - **Output tardío (*Late Escaping*)**: `esc_html()`, `esc_attr()`, `esc_url()`, `wp_json_encode()`.
   - **Base de datos**: Siempre `$wpdb->prepare("SELECT ... WHERE id = %d", $id)`. Prohibida la interpolación de variables en SQL.
3. **Rendimiento y Caché**:
   - Prohibido `query_posts()`. Usa `WP_Query` con `'no_found_rows' => true` cuando no haya paginación y `'fields' => 'ids'` cuando solo necesites identificadores; apóyate en Transients / Redis Object Cache para consultas costosas.
