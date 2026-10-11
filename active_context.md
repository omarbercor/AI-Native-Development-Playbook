> Last Updated: 2026-10-10 | Version: 2.0 | Sprint: Reestructuración Oficial .agents/

# Active Context — RAM y Memoria Persistente del Proyecto

> [!IMPORTANT]
> **REGLA PARA EL AGENTE IA**: Este archivo es la memoria viva (RAM) del proyecto.
> 1. Léelo **al inicio** de cada sesión antes de ejecutar cualquier tarea para saber en qué estado se quedó el desarrollo.
> 2. Actualízalo **al cierre** de cada tarea o sprint con los logros, el nuevo estado y los siguientes pasos.
> 3. Mantén este archivo limpio y directo (no es un log histórico infinito; lo resuelto se resume o elimina).

---

## 🗓️ Última Actualización

- **Fecha**: 2026-10-10
- **Sesión**: Migración de la arquitectura `.ai/` al sistema nativo `.agents/`
- **Estado**: ✅ Estructura `.agents/` completada con Rules y Skills funcionales.

---

## 🎯 Objetivo del Sprint Actual

Transformar el repositorio en una plantilla maestra (boilerplate) de gobernanza y desarrollo para agentes IA con Google Antigravity, lista para ser reutilizada en cualquier proyecto web.

---

## ✅ Completado en Esta Sesión

1. **Definición de Reglas Invariantes (`.agents/rules/`)**:
   - `01-security-zero-trust.md`: Prohibición absoluta de hardcoding de API keys/secretos y blindaje de `.env`.
   - `02-session-lifecycle.md`: Protocolo de inicio y cierre de sesión con memoria persistente.
   - `03-code-standards.md`: Principios DRY/KISS/YAGNI, calidad de código sin placeholders y Conventional Commits.
2. **Transformación de Roles en Skills Nativos (`.agents/skills/`)**:
   - `web-frontend-craft`: Diseño WOW, glassmorphism, micro-animaciones a 60 FPS, tokens y accesibilidad.
   - `secret-auditor`: Procedimiento de escaneo pre-commit con regex de firmas de API keys.
   - `project-planner`: PRD, 3-Path Analysis y plantillas de ADR.
   - `documentation-writer`: Generación de User Guide, Developer Guide y Team Reference.
   - `backend-api-design`: APIs REST, contratos OpenAPI y validación en el borde.
   - `visual-qa-testing`: Pirámide de testing y checklist visual responsive.
   - `seo-cwv-optimizer`: Core Web Vitals, Schema.org y metadatos SEO.
   - `devops-infra`: Dockerfiles multi-stage, compose y despliegue multi-entorno.
3. **Instalación de `active_context.md` en la raíz del proyecto**.

---

## 🏗️ Estado del Sistema de Agentes (`.agents/`)

| Componente | Tipo | Estado | Propósito |
| :--- | :--- | :--- | :--- |
| `01-security-zero-trust.md` | Rule (Invariante) | ✅ Activa | Bloquear subida de credenciales y API keys |
| `02-session-lifecycle.md` | Rule (Invariante) | ✅ Activa | Forzar lectura/escritura de `active_context.md` |
| `03-code-standards.md` | Rule (Invariante) | ✅ Activa | Calidad 10/10, Conventional Commits y sin TODOs |
| `web-frontend-craft` | Skill (On-Demand) | ✅ Lista | UI/UX de alto impacto y animación fluida |
| `secret-auditor` | Skill (On-Demand) | ✅ Lista | Escaneo de regex antes de commits |
| `project-planner` | Skill (On-Demand) | ✅ Lista | PRD, 3-path analysis y decisiones técnicas |
| `documentation-writer` | Skill (On-Demand) | ✅ Lista | Documentación para usuario, dev y equipo |
| `backend-api-design` | Skill (On-Demand) | ✅ Lista | Arquitectura REST y contratos de datos |
| `visual-qa-testing` | Skill (On-Demand) | ✅ Lista | Testing unitario, E2E y responsive |
| `seo-cwv-optimizer` | Skill (On-Demand) | ✅ Lista | Core Web Vitals y Schema.org |
| `devops-infra` | Skill (On-Demand) | ✅ Lista | Docker, despliegues y multi-entorno |

---

## ⚠️ Bloqueadores / Limitaciones Conocidas

- _Ninguno actualmente._

---

## 🔧 Deuda Técnica Activa

_Ninguna deuda técnica activa. El repositorio ha sido limpiado y estandarizado al 100%._

---

## 📋 Próximas Acciones

1. Usar esta plantilla como base para nuevos proyectos web clonando `.agents/`, `active_context.md`, `.gitignore` y `.env.example`.
2. (Opcional) Instalar globalmente en `~/.gemini/config/` si se desea que aplique a cualquier carpeta de la máquina.
3. Realizar commit inicial de la nueva estructura estandarizada.
