---
name: cms-plugin-extension
description: >-
  Clean extension and plugin/theme development specialist for CMS platforms (WordPress, Shopify, etc.).
  Use this skill when developing plugins, custom modules, or child themes, ensuring zero core modifications,
  isolated namespaces, official hooks/filters, security nonces, and forward compatibility.
---

# CMS & Plugin Extension — Extensión Limpia sin Modificar el Core

Esta habilidad garantiza que cualquier desarrollo sobre un CMS o aplicación base (como WordPress, Drupal o plataformas headless) sea **100% modular, seguro y compatible hacia el futuro (*future-proof*)**, sin tocar jamás archivos del sistema base.

---

## 1. La Ley Inviolable: Cero Modificaciones al Core

> [!CAUTION]
> **PROHIBICIÓN ABSOLUTA**: Jamás se edita, renombra o añade código a archivos del núcleo de WordPress o del framework (`wp-admin/`, `wp-includes/`, `index.php`, `wp-config.php` base, o carpetas `vendor/`). Cualquier cambio realizado en el core se destruirá en la siguiente actualización del sistema.

### Ubicaciones Permitidas:
- Plugins propios: `wp-content/plugins/[mi-plugin-custom]/`
- Temas hijo (Child Themes): `wp-content/themes/[mi-tema-child]/`
- Must-Use Plugins (solo si aplica arquitectura global): `wp-content/mu-plugins/`

---

## 2. Principios de Extensibilidad Limpia

1. **Uso Exclusivo de Hooks Oficiales (*Actions & Filters*)**:
   - Para añadir funcionalidad o inyectar HTML/JS: `add_action('hook_name', 'callback')`.
   - Para modificar datos existentes antes de guardarse o renderizarse: `add_filter('hook_name', 'callback')`.
   - Nunca alterar templates del tema padre directamente; sobrescribirlos en el Child Theme o inyectar mediante hooks.

2. **Aislamiento Total de Nombres (Namespaces y Prefijos)**:
   - Toda función, constante o clase debe usar un prefijo de proyecto o namespace PHP para evitar colisiones con otros plugins:
     ```php
     // ✅ Correcto: Con prefijo único o namespace
     namespace MiProyecto\Billing;
     const MIPROJECT_VERSION = '1.0.0';
     function miproject_init() { ... }

     // ❌ Peligroso: Sin prefijo (colisionará con otros plugins)
     function init() { ... }
     ```

3. **Arquitectura Modular del Plugin (< 150 líneas por archivo)**:
   - Dividir el plugin en responsabilidades claras:
     ```text
     mi-plugin/
     ├── mi-plugin.php          ← Archivo principal de registro y activación (< 80 líneas)
     ├── includes/
     │   ├── class-api.php      ← Endpoints REST nativos
     │   ├── class-admin.php    ← Menús y vistas del panel de administración
     │   └── class-security.php ← Verificación de nonces y capabilities
     ├── templates/             ← Vistas HTML limpias
     └── assets/                ← CSS y JS encolados con wp_enqueue_scripts
     ```

---

## 3. Seguridad en Extensiones de CMS (WordPress Standards)

1. **Verificación de Nonces en Formularios y AJAX**:
   - Todo formulario o petición REST/AJAX debe validar un nonce para evitar ataques CSRF:
     ```php
     check_admin_referer('miproject_action_nonce', 'security');
     // o para REST:
     'permission_callback' => function() { return current_user_can('manage_options'); }
     ```

2. **Capacidades de Usuario (*Capabilities*)**:
   - Comprobar permisos antes de ejecutar acciones administrativas: `current_user_can('edit_posts')`.

3. **Sanitización y Escape Obligatorio**:
   - **Al recibir datos**: `sanitize_text_field($_POST['campo'])`, `absint()`, `sanitize_email()`.
   - **Al renderizar en pantalla**: `esc_html()`, `esc_attr()`, `esc_url()`.
   - **En base de datos**: Usar `$wpdb->prepare()` para cualquier consulta SQL personalizada. Prohibida la interpolación directa.

---

## 4. Compatibilidad Futura (*Future-Proof*)

1. **Evitar APIs y Funciones Deprecadas**:
   - Consultar la documentación oficial de la versión activa antes de usar funciones heredadas.
2. **Encolado Correcto de Assets**:
   - NUNCA escribir etiquetas `<script>` o `<link>` directo en el HTML. Usar `wp_enqueue_script()` y `wp_enqueue_style()` con dependencias y números de versión para evitar problemas de caché.
3. **Desactivación y Desinstalación Limpia**:
   - Incluir `uninstall.php` para eliminar transients, tablas temporales u opciones solo cuando el usuario borre explícitamente el plugin, sin dejar basura en la base de datos.
