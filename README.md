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
[Repositorio Plantilla / Tu Proyecto]/
├── AGENTS.md                  ← 🧭 Ancla Universal Always-On (Antigravity, Cursor, Claude, Codex)
├── active_context.example.md  ← 🧠 Plantilla limpia de RAM (se copia a active_context.md en cada proyecto)
├── .env.example               ← 🔑 Plantilla segura de variables de entorno
├── .gitignore                 ← 🛡️ Protección Zero-Trust (.env*, llaves, builds)
│
└── .agents/
    ├── rules/                 ← ⚖️ LEYES INVARIANTES (trigger: always_on)
    │   ├── 01-security-zero-trust.md   ← Cero API keys en código, blindaje de .env
    │   ├── 02-session-lifecycle.md     ← Lectura/Escritura obligatoria de active_context.md
    │   └── 03-code-standards.md        ← DRY, KISS, Conventional Commits, sin placeholders
    │
    └── skills/                ← 🛠️ HABILIDADES BAJO DEMANDA (Progressive Disclosure)
        ├── awwwards-experience-craft/  ← Sitios de experiencia Awwwards: tipografía monumental, GSAP, Three.js
        ├── web-frontend-craft/         ← UI funcional/SaaS, componentes, tokens CSS y accesibilidad
        ├── secret-auditor/             ← Escaneo regex de credenciales antes de commits
        ├── project-planner/            ← PRD, 3-Path Analysis, ADRs de arquitectura
        ├── documentation-writer/       ← User Guide, Developer Guide y Team Reference
        ├── backend-api-design/         ← APIs REST, contratos OpenAPI, validación en el borde
        ├── visual-qa-testing/          ← Pirámide de pruebas, responsive checks y accesibilidad
        ├── seo-cwv-optimizer/          ← SEO, GEO (llms.txt), Core Web Vitals y Astro/GSAP sin CLS
        ├── devops-infra/               ← CloudPanel + CI/CD Local Runners, PM2 y Docker Compose v2
        └── cms-plugin-extension/       ← WordPress moderno, bloques nativos y puente Headless con Astro
```

---

## ⚖️ 1. Las Reglas Invariantes (`.agents/rules/`)

Las reglas son restricciones que el agente **nunca puede violar**. Están activas de forma permanente en cada interacción:

| Regla                                                                  | Propósito                                                                                                                    |
| :--------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------- |
| [`01-security-zero-trust.md`](.agents/rules/01-security-zero-trust.md) | **Tolerancia Cero** a claves API hardcodeadas. Todo secreto va a `.env` (ignorado en Git). Verificación de regex previa.     |
| [`02-session-lifecycle.md`](.agents/rules/02-session-lifecycle.md)     | **Memoria Obligatoria**: Al iniciar, lee `active_context.md`. Al cerrar una tarea, actualiza el estado y los próximos pasos. |
| [`03-code-standards.md`](.agents/rules/03-code-standards.md)           | **Calidad 10/10**: Prohibidos bloques de código con `// TODO: implementar`. Conventional Commits y arquitectura limpia.      |

---

## 🛠️ 2. Las Habilidades Bajo Demanda (`.agents/skills/`)

Las habilidades no saturan la ventana de contexto (_tokens_). Antigravity solo lee su cabecera y abre el contenido detallado **únicamente cuando la tarea lo requiere**:

- 🏆 **`awwwards-experience-craft`**: Dirección de arte e ingeniería creativa para **sitios de experiencia nivel Awwwards**. Tipografía monumental como factor de impacto, espacio negativo intencional, tensión brutalista/minimalista, cursores magnéticos, texturas orgánicas reactivas, animaciones con GSAP/Lenis y 3D justificado con Three.js/Shaders (60 FPS) en arquitectura modular (< 200 líneas) compatible con Astro, WordPress/CMS o React.
- 🎨 **`web-frontend-craft`**: Ingeniería de alta precisión para **interfaces funcionales, SaaS, dashboards, e-commerce y sitios CMS**. Superficies silenciosas sin "AI Slop", datos `tabular-nums`, estándar `mobile-native` (cero auto-zoom iOS, 100dvh, safe areas), contenedor `.cms-prose`, los 5 estados resilientes (`break-ui`) y accesibilidad WCAG 2.2 AA.
- 🛡️ **`secret-auditor`**: Escanea cambios activos y archivos modificados con expresiones regulares para atrapar claves de Google Cloud, OpenAI, Anthropic, Stripe, AWS o llaves privadas antes del commit.
- 📐 **`project-planner`**: Define el perfil visual inicial, el mapa de módulos atómicos (`< 200 líneas`), el **Análisis de 3 Caminos** y los ADRs antes de escribir código.
- ✍️ **`documentation-writer`**: Redacta documentación especializada para tres audiencias con plantillas listas: usuarios finales, desarrolladores externos y equipo interno.
- ⚙️ **`backend-api-design`**: APIs blindadas en el borde, DTOs explícitos, paginación obligatoria, prevención de N+1, timeouts y verificación HMAC idempotente en webhooks.
- 🧪 **`visual-qa-testing`**: Protocolo de tortura **`break-ui`** (textos extremos, 0/1M ítems, doble submit, teclado), auditoría de consola cero-errores y tests en Vitest/Playwright.
- 🔍 **`seo-cwv-optimizer`**: Core Web Vitals reales (`< 2.2s` LCP, `< 0.05` CLS, `< 150ms` INP), fuentes `.woff2` sin bloqueo, islas Astro, animaciones accesibles, Schema.org y **`/llms.txt`** (GEO).
- 🚀 **`devops-infra`**: Despliegues atómicos hacia **CloudPanel** (aislamiento de `<site-user>`, Nginx, PM2, PHP-FPM) mediante **CI/CD con Local / Self-Hosted Runners** y Docker Compose v2 con healthchecks.
- 🧩 **`cms-plugin-extension`**: Desarrollo limpio sobre WordPress/CMS sin tocar el core (`< 150 líneas`), bloques modernos (`block.json`), carga condicional de GSAP/Three.js e integración Headless con Astro.

---

## 🧠 3. La Memoria Persistente (`active_context.md`)

Este archivo es la **RAM** del proyecto. Permite cerrar el IDE hoy y reanudar el trabajo dentro de 30 días con total claridad:

1. **Al iniciar una sesión:** El agente lee `active_context.md` y sabe al instante cuál es el objetivo actual, qué se terminó y qué sigue.
2. **Al cerrar una tarea:** El agente actualiza la sección de logros, registra deuda técnica asumida (`[DEBT-XXX]`) y define las próximas acciones inmediatas.

---

## 🚀 Quickstart: Cómo Instalar este Playbook desde GitHub

Este repositorio está diseñado como **plantilla limpia**: incluye `active_context.example.md` en lugar de un `active_context.md` usado para que cada nuevo proyecto nazca con su memoria en blanco.

### Opción A: Iniciar un Proyecto Nuevo desde Cero
Puedes pulsar **"Use this template"** directamente en GitHub, o descargarlo sin historial de Git desde tu terminal:

```powershell
# PowerShell (Windows)
npx degit omarbercor/AI-Native-Development-Playbook mi-nuevo-proyecto
cd mi-nuevo-proyecto
Copy-Item active_context.example.md active_context.md
```

```bash
# Bash (macOS / Linux)
npx degit omarbercor/AI-Native-Development-Playbook mi-nuevo-proyecto
cd mi-nuevo-proyecto && cp active_context.example.md active_context.md
```

### Opción B: Inyectar en un Proyecto Ya Existente (Astro, WordPress, Next.js)
Para añadir únicamente el cerebro de gobernanza sin sobrescribir el `README.md` ni archivos actuales de tu proyecto:

```powershell
# PowerShell (Windows) — Ejecutar en la raíz de tu proyecto actual
npx degit omarbercor/AI-Native-Development-Playbook/.agents .agents
Invoke-WebRequest https://raw.githubusercontent.com/omarbercor/AI-Native-Development-Playbook/main/AGENTS.md -OutFile AGENTS.md
Invoke-WebRequest https://raw.githubusercontent.com/omarbercor/AI-Native-Development-Playbook/main/active_context.example.md -OutFile active_context.md
```

```bash
# Bash (macOS / Linux) — Ejecutar en la raíz de tu proyecto actual
npx degit omarbercor/AI-Native-Development-Playbook/.agents .agents
curl -fsSLO https://raw.githubusercontent.com/omarbercor/AI-Native-Development-Playbook/main/AGENTS.md
curl -fsSL https://raw.githubusercontent.com/omarbercor/AI-Native-Development-Playbook/main/active_context.example.md -o active_context.md
```

### Opción C: Copiar desde tu Clon Local en Disco

```powershell
# PowerShell (Windows) — Desde la raíz de tu proyecto destino
$Playbook = "O:\Desarrollo\AI-Native-Development-Playbook"
Copy-Item "$Playbook\.agents" -Destination ".\.agents" -Recurse -Force
Copy-Item "$Playbook\AGENTS.md" -Destination ".\AGENTS.md" -Force
Copy-Item "$Playbook\active_context.example.md" -Destination ".\active_context.md"
```

### Arrancar la Primera Sesión con el Agente
Abre el proyecto en Antigravity (si olvidaste copiar `active_context.md`, el agente lo creará solo desde `active_context.example.md`) y lanza tu primer prompt indicando el enfoque:

- **Para un Sitio de Experiencia (Awwwards / Brutal / 3D / GSAP):**
  > *"Iniciemos el proyecto para [Marca]. Revisa `active_context.md` y planifica una experiencia en Astro con el perfil `awwwards-experience-craft`, tipografía monumental, fondo orgánico y módulos `< 200 líneas`."*
- **Para un Sitio Funcional, SaaS, E-Commerce o WordPress/CMS:**
  > *"Iniciemos el proyecto para [Cliente]. Revisa `active_context.md` y estructura la arquitectura con `web-frontend-craft` y `cms-plugin-extension`, asegurando estándar `mobile-native` y despliegue para CloudPanel."*

---

## 🔒 Manejo Seguro de Entornos

- **`.env.example`**: Siempre incluido en el repositorio con las variables requeridas pero sin valores sensibles.
- **`.env` / `.env.local`**: Protegidos por `.gitignore` y auditados automáticamente por `secret-auditor`.

---

## 📄 Licencia

Libre para usar, adaptar y distribuir en tus proyectos personales y comerciales.
