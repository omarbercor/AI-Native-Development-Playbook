# AI-Native Development Playbook

> Un sistema de gobernanza para equipos de desarrollo aumentados por IA. Convierte un agente IA genérico en un equipo de especialistas con memoria persistente, estándares de clase mundial y criterios de calidad profesionales.

---

## ¿Qué es esto?

El mundo del desarrollo tiene *Scrum*, *Kanban*, *XP* — metodologías diseñadas para coordinar **personas**. Este playbook hace lo equivalente, pero diseñado para coordinar **agentes IA especializados** con un humano como director técnico.

No es un simple conjunto de prompts. Es un **sistema operativo para desarrollo de software** compuesto por cuatro capas:

| Capa | Qué hace |
|---|---|
| **Constitución** | Ley suprema que todo agente obedece antes de actuar. Sin esto, cada sesión de IA empieza desde cero. Con esto, cada sesión tiene el mismo ADN. |
| **Equipo Virtual** | 7 sub-agentes especializados con dominios, herramientas y criterios de calidad definidos. El Frontend no toca infra. El DevOps no diseña UI. El QA no hace suposiciones. |
| **Sistema Nervioso** | Memoria persistente que une sesiones. La IA sabe exactamente dónde estaba el proyecto, qué decisiones se tomaron y cuál es el siguiente paso. |
| **Estándares de Industria** | Conventional Commits, Semantic Versioning, Test Pyramid, API-First, Zero-Trust, Core Web Vitals — codificados como comportamiento obligatorio, no como sugerencias. |

---

## El Equipo (Roles del Sistema)

| Rol | Dominio |
|---|---|
| 🧠 **Orchestrator PM** | Planificación, descomposición de tareas, ruteo, cierre de sprint |
| ⚙️ **Backend Architect** | APIs REST, autenticación, base de datos, contratos de datos |
| 🐳 **DevOps SysAdmin** | Infraestructura, Docker, CI/CD, redes, backups, seguridad |
| 🎨 **Frontend Engineer** | UI/UX premium, animaciones, componentes, rendimiento visual |
| 🧪 **QA Automation** | Testing, cobertura, mocks, performance, contract testing |
| 🔍 **SEO Expert** | On-page SEO, Schema.org, Core Web Vitals, visibilidad orgánica |
| ✍️ **Tech Writer** | Documentación para usuarios, developers externos y equipo interno |

> El sistema no está limitado a estos 7 roles. El Orquestador puede proponer y crear nuevos roles cuando un proyecto lo requiera.

---

## Cómo Funciona

```
[Tú] → Solicitud o requerimiento
    ↓
[Orchestrator PM] → Descompone, prioriza y asigna al especialista correcto
    ↓
[Sub-Agente Especialista] → Ejecuta con sus reglas locales de excelencia
    ↓
[Orchestrator PM] → Verifica Definition of Done
    ↓
[Cierre] → Commit semántico + active_context.md actualizado
```

---

## Quickstart — Usar este Playbook en un Proyecto Nuevo

### 1. Copia la carpeta `.ai/` a tu proyecto

```bash
cp -r .ai/ /tu-nuevo-proyecto/.ai/
cp .gitignore /tu-nuevo-proyecto/.gitignore
mkdir /tu-nuevo-proyecto/mcp
```

### 2. Configura tu agente IA

Apunta tu agente IA al archivo de reglas de tu herramienta:

- **Cursor**: crea `.cursor/rules` apuntando a `.ai/00_project_core.md`
- **GitHub Copilot**: usa `.github/copilot-instructions.md`
- **Claude / Gemini**: pega el contenido de `00_project_core.md` como system prompt

### 3. Define tu primer sprint

Abre `.ai/active_context.md` y escribe el objetivo de tu primer sprint. La IA lo leerá al inicio de cada sesión.

### 4. Empieza a trabajar

Habla con tu IA como si fuera el Orquestador. Ejemplo:

> *"Necesito construir un sistema de autenticación con login, registro y recuperación de contraseña."*

El Orquestador descompondrá la tarea, la asignará al Backend Architect para el API y al Frontend Engineer para la UI, coordinará el QA para los tests, y cerrará el sprint con un commit semántico y el contexto actualizado.

---

## Estándares Incluidos

| Documento | Cubre |
|---|---|
| [`00_project_core.md`](.ai/00_project_core.md) | Principios universales, Definition of Done, política Zero-Trust |
| [`01_planning_and_architecture.md`](.ai/01_planning_and_architecture.md) | PRD, ADRs, 3-path analysis, API-First, observabilidad |
| [`02_mcp_standards.md`](.ai/02_mcp_standards.md) | MCP tools, whitelist de seguridad, ciclo de vida |
| [`03_documentation_standards.md`](.ai/03_documentation_standards.md) | Documentación para 3 audiencias con templates listos |
| [`04_git_standards.md`](.ai/04_git_standards.md) | Branching, PR checklist, Conventional Commits, releases |
| [`05_environment_standards.md`](.ai/05_environment_standards.md) | Secrets, multi-entorno, rotación de credenciales |

---

## Estructura del Sistema

```
[proyecto]/
├── .gitignore               ← Protección de secrets desde el primer commit
├── README.md                ← Este archivo
├── mcp/                     ← Servidores MCP (uno por carpeta)
│   └── README.md
│
└── .ai/
    ├── 00_project_core.md
    ├── 01_planning_and_architecture.md
    ├── 02_mcp_standards.md
    ├── 03_documentation_standards.md
    ├── 04_git_standards.md
    ├── 05_environment_standards.md
    ├── active_context.md    ← RAM del sistema (se actualiza cada sprint)
    ├── mcp.json             ← Registro de MCPs activos
    ├── docs/
    │   ├── user_guide.md
    │   ├── developer_guide.md
    │   └── team_reference.md
    └── roles/
        ├── orchestrator_pm.md
        ├── backend_architect.md
        ├── devops_sysadmin.md
        ├── frontend_engineer.md
        ├── qa_automation.md
        ├── seo_expert.md
        └── tech_writer.md
```

---

## Por Qué es Distinto a una Metodología Tradicional

| Metodología tradicional | AI-Native Development Playbook |
|---|---|
| Coordina personas | Coordina agentes IA con un humano como director |
| El conocimiento vive en las personas | El conocimiento vive en el repositorio |
| Requiere reuniones para alinear al equipo | El equipo se alinea leyendo el contexto al inicio de cada sesión |
| Se degrada con la rotación de personas | Es inmune a la rotación — el sistema sabe lo que sabe |
| Los estándares son aspiracionales | Los estándares son instrucciones que la IA ejecuta literalmente |
| No escala sin contratar | Escala añadiendo un nuevo archivo de rol |

---

## Principios Fundamentales

- **DRY** — Si una lógica aparece dos veces, se abstrae.
- **KISS** — La solución más simple que cumple el requerimiento es la correcta.
- **YAGNI** — No se implementa funcionalidad especulativa.
- **Zero-Trust** — Todo input externo es potencialmente hostil. Siempre.
- **Fail Fast** — Los errores se detectan y reportan lo más temprano posible.
- **Single Source of Truth** — Cada dato, config o estado tiene exactamente un lugar donde vive.

---

## Licencia

Libre para usar, adaptar y compartir con tu equipo.
