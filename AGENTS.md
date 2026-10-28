# AGENTS.md — Gobernanza Global del Proyecto (AI-Native Playbook)

Este proyecto opera bajo una arquitectura estricta de **Memoria Persistente**, **Seguridad Zero-Trust** y **Artesanía Modular**. Todo agente de IA (Google Antigravity, Gemini CLI, Cursor, Claude Code, Codex) debe cumplir de forma invariante estas directrices en cada sesión.

## 1. Protocolo de Memoria Persistente (`active_context.md`)
- **Al iniciar sesión**: Lee obligatoriamente `active_context.md` en la raíz antes de proponer planes o escribir código (si no existe, inicialízalo desde `active_context.example.md`).
- **Al finalizar una tarea o sprint**: Actualiza `active_context.md` con los logros verificados, deuda técnica (`[DEBT-XXX]`) y próximas acciones, manteniéndolo limpio (`< 150 líneas`).
- Detalle completo: [`.agents/rules/02-session-lifecycle.md`](./.agents/rules/02-session-lifecycle.md).

## 2. Seguridad Zero-Trust Innegociable
- **Cero secretos hardcodeados**: Prohibido escribir API keys reales (`AIza...`, `sk-...`, `ghp_...`, `AKIA...`), contraseñas o tokens en código o commits.
- Todo secreto vive exclusivamente en `.env` (ignorado en `.gitignore`) y se documenta vacío en `.env.example`.
- Valida todo input externo en el borde y ejecuta el skill `secret-auditor` antes de cualquier `git commit`.
- Detalle completo: [`.agents/rules/01-security-zero-trust.md`](./.agents/rules/01-security-zero-trust.md).

## 3. Arquitectura Modular y Estándares de Código (10/10)
- **Límite estricto de tamaño**: Archivos atómicos y reutilizables de **máximo 150–200 líneas** y funciones pequeñas (`< 25–30 líneas`). Si un componente, escena 3D, controlador o vista crece más, divídelo en submódulos independientes.
- **Cero Placeholders**: Prohibido entregar bloques con `// TODO: implementar aquí` o código incompleto.
- **Core Inviolable**: En WordPress o CMS, jamás modifiques archivos del núcleo (`wp-admin/`, `wp-includes/`, `vendor/`).
- **Commits**: Usa siempre *Conventional Commits* (`feat:`, `fix:`, `refactor:`, `perf:`, `docs:`, `chore:`, `test:`).
- Detalle completo: [`.agents/rules/03-code-standards.md`](./.agents/rules/03-code-standards.md).

## 4. Selección de Perfil Frontend (Experiencia vs. Funcional)
Antes de diseñar o maquetar cualquier interfaz, elige el skill adecuado en `.agents/skills/`:
- **`awwwards-experience-craft`**: Para **sitios de experiencia, marca, portafolios y lanzamientos creativos** (tipografía monumental, espacio negativo activo, cursores magnéticos, GSAP/Lenis, texturas orgánicas y Three.js/WebGL a 60 FPS).
- **`web-frontend-craft`**: Para **interfaces funcionales, SaaS, dashboards, e-commerce y paneles CMS** donde priman la claridad operativa, velocidad de tarea y consistencia de componentes.

