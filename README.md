# AI-Native Development Playbook 🚀

> **Un sistema operativo de gobernanza y desarrollo para agentes de IA en Google Antigravity.**  
> Transforma un agente de IA genérico en un equipo de ingeniería de élite con **memoria persistente**, **estándares de seguridad Zero-Trust** y **criterios de calidad visual 10/10**.

---

## 💡 ¿Por Qué Existe Este Playbook?

Al trabajar con asistentes y agentes de IA en proyectos de software surgen tres problemas universales:

1. **Amnesia entre sesiones:** Cierras el IDE hoy, regresas en un mes o una semana, y la IA no tiene idea de qué se estaba haciendo, qué decisiones se tomaron ni cuál era el siguiente paso.
2. **Fugas de credenciales y seguridad:** Sin directrices estrictas, los modelos tienden a hardcodear claves API (`sk-...`, `AIza...`), tokens o credenciales directamente en el código o en commits de Git.
3. **Código genérico o plano:** La IA tiende a entregar soluciones mínimas viables con estilos básicos, componentes desordenados y placeholders del tipo `// TODO: implementar aquí`.

Este Playbook resuelve estos tres problemas estructurando tu repositorio bajo la convención nativa de **Google Antigravity (`.agents/`)**.

---

## 🏛️ Arquitectura del Sistema: Las Tres Capas

```text
[Tu Proyecto]/
├── active_context.md          ← 🧠 RAM y Memoria Persistente (Estado vivo del proyecto)
├── .gitignore                 ← 🛡️ Protección de secrets (.env*)
│
└── .agents/
    ├── rules/                 ← ⚖️ LEYES INVARIANTES (Siempre activas)
    │   ├── 01-security-zero-trust.md   ← Cero API keys en código, blindaje de .env
    │   ├── 02-session-lifecycle.md     ← Lectura/Escritura obligatoria de active_context.md
    │   └── 03-code-standards.md        ← DRY, KISS, Conventional Commits, sin placeholders
    │
    └── skills/                ← 🛠️ HABILIDADES BAJO DEMANDA (Progressive Disclosure)
        ├── web-frontend-craft/         ← UI WOW, glassmorphism, micro-animaciones, 60 FPS
        ├── secret-auditor/             ← Escaneo regex de credenciales antes de commits
        ├── project-planner/            ← PRD, 3-Path Analysis, ADRs de arquitectura
        ├── documentation-writer/       ← User Guide, Developer Guide y Team Reference
        ├── backend-api-design/         ← APIs REST, contratos OpenAPI, validación en el borde
        ├── visual-qa-testing/          ← Pirámide de pruebas, responsive checks y accesibilidad
        ├── seo-cwv-optimizer/          ← Core Web Vitals (LCP/CLS/INP), Schema.org y metadatos
        ├── devops-infra/               ← Dockerfiles multi-stage, docker-compose y despliegues
        └── cms-plugin-extension/       ← Plugins/temas WordPress/CMS limpios sin tocar el core
```

---

## ⚖️ 1. Las Reglas Invariantes (`.agents/rules/`)

Las reglas son restricciones que el agente **nunca puede violar**. Están activas de forma permanente en cada interacción:

| Regla | Propósito |
| :--- | :--- |
| [`01-security-zero-trust.md`](.agents/rules/01-security-zero-trust.md) | **Tolerancia Cero** a claves API hardcodeadas. Todo secreto va a `.env` (ignorado en Git). Verificación de regex previa. |
| [`02-session-lifecycle.md`](.agents/rules/02-session-lifecycle.md) | **Memoria Obligatoria**: Al iniciar, lee `active_context.md`. Al cerrar una tarea, actualiza el estado y los próximos pasos. |
| [`03-code-standards.md`](.agents/rules/03-code-standards.md) | **Calidad 10/10**: Prohibidos bloques de código con `// TODO: implementar`. Conventional Commits y arquitectura limpia. |

---

## 🛠️ 2. Las Habilidades Bajo Demanda (`.agents/skills/`)

Las habilidades no saturan la ventana de contexto (*tokens*). Antigravity solo lee su cabecera y abre el contenido detallado **únicamente cuando la tarea lo requiere**:

- 🎨 **`web-frontend-craft`**: Especialista en crear interfaces que sorprendan. Modo oscuro calibrado, paletas HSL, glassmorphism (`backdrop-filter`), animaciones fluidas con GPU (`transform`, `opacity`) y accesibilidad WCAG AA.
- 🛡️ **`secret-auditor`**: Escanea cambios activos y archivos modificados con expresiones regulares para atrapar claves de Google Cloud, OpenAI, Anthropic, Stripe, AWS o llaves privadas antes del commit.
- 📐 **`project-planner`**: Divide problemas complejos en tareas atómicas y evalúa el **Análisis de 3 Caminos** (Prototipo Rápido vs. Arquitectura Escalable vs. Mínimo Mantenimiento).
- ✍️ **`documentation-writer`**: Redacta documentación especializada para tres audiencias con plantillas listas: usuarios finales, desarrolladores externos y equipo interno.
- ⚙️ **`backend-api-design`**: Estructura APIs predecibles, esquemas de error unificados y validación de tipos en el borde.
- 🧪 **`visual-qa-testing`**: Checklist de pruebas en múltiples dispositivos (375px a 1920px), estados límite (listas vacías, textos largos) y determinismo de pruebas.
- 🔍 **`seo-cwv-optimizer`**: Optimización de Core Web Vitals (< 2.5s LCP, < 0.1 CLS), marcado JSON-LD y etiquetas OpenGraph.
- 🐳 **`devops-infra`**: Contenedores Docker seguros sin usuario root, configuraciones de reverse proxy y despliegues reproducibles.

---

## 🧠 3. La Memoria Persistente (`active_context.md`)

Este archivo es la **RAM** del proyecto. Permite cerrar el IDE hoy y reanudar el trabajo dentro de 30 días con total claridad:

1. **Al iniciar una sesión:** El agente lee `active_context.md` y sabe al instante cuál es el objetivo actual, qué se terminó y qué sigue.
2. **Al cerrar una tarea:** El agente actualiza la sección de logros, registra deuda técnica asumida (`[DEBT-XXX]`) y define las próximas acciones inmediatas.

---

## 🚀 Quickstart: Usar este Playbook en un Proyecto Nuevo

### Paso 1: Copiar la carpeta `.agents/` y los archivos base
Copia `.agents/`, `active_context.md` y `.gitignore` a tu nuevo proyecto:

```bash
# Desde la raíz de tu nuevo proyecto:
cp -r /ruta/a/AI-Native-Development-Playbook/.agents ./
cp /ruta/a/AI-Native-Development-Playbook/active_context.md ./
cp /ruta/a/AI-Native-Development-Playbook/.gitignore ./
```

*(O puedes instalar las reglas y habilidades de forma global en `~/.gemini/config/` para que apliquen a todos tus proyectos en la máquina).*

### Paso 2: Definir el primer sprint en `active_context.md`
Abre `active_context.md` y escribe el nombre del proyecto y el objetivo de la primera sesión:

```markdown
## 🎯 Objetivo del Sprint Actual
Construir una landing page para [Producto] con modo oscuro, formulario interactivo y diseño premium.
```

### Paso 3: Interactuar con el Agente
Pide lo que necesitas con naturalidad:

> *"Iniciemos el proyecto. Revisa el active_context y diseña la estructura inicial con diseño WOW y arquitectura modular."*

El agente automáticamente:
1. Leerá `active_context.md`.
2. Activará el skill `project-planner` y `web-frontend-craft`.
3. Verificará que las credenciales estén en `.env` antes de cualquier commit.
4. Actualizará la memoria al finalizar.

---

## 🔒 Manejo Seguro de Entornos

- **`.env.example`**: Siempre incluido en el repositorio con las variables requeridas pero sin valores sensibles.
- **`.env` / `.env.local`**: Protegidos por `.gitignore` y auditados automáticamente por `secret-auditor`.

---

## 📄 Licencia

Libre para usar, adaptar y distribuir en tus proyectos personales y comerciales.
