> Last Updated: YYYY-MM-DD | Version: 1.0 | Sprint: [sprint-name]

# [Project Name] — Developer Guide

> **Nota para el desarrollador**: Documento para desarrolladores externos o nuevos miembros del equipo que van a configurar, integrar o contribuir al proyecto.

---

## 1. Visión General del Stack

[2-3 oraciones describiendo la arquitectura general del proyecto.]

- **Runtime / Lenguaje**: [e.g., Node.js 20 LTS / TypeScript 5 / Python 3.11]
- **Framework**: [e.g., Next.js 14 / Vite + React / FastAPI]
- **Base de Datos / Storage**: [e.g., PostgreSQL / Redis / Supabase]
- **Estilos / UI**: [e.g., Vanilla CSS Tokens / Tailwind CSS / Radix UI]

---

## 2. Requisitos Previos

| Herramienta | Versión Mínima | Enlace / Instrucción |
| :--- | :--- | :--- |
| [Node.js / Python] | [v20.x] | [https://nodejs.org] |
| [Docker] | [v24.x] | [https://docker.com] |
| [Package Manager] | [npm / pnpm / poetry] | — |

---

## 3. Configuración Local en 5 Minutos

```bash
# 1. Clonar el repositorio
git clone https://github.com/[org]/[repo].git
cd [repo]

# 2. Instalar dependencias
npm install

# 3. Configurar variables de entorno
cp .env.example .env
# Editar .env con tus credenciales locales

# 4. Iniciar servicios o base de datos (si aplica)
docker compose up -d

# 5. Iniciar servidor de desarrollo
npm run dev
```

---

## 4. Variables de Entorno

> ⚠️ **NUNCA subas archivos `.env` a Git**. Usa `.env.example` como referencia.

| Variable | Descripción | Ejemplo / Default | ¿Obligatoria? |
| :--- | :--- | :--- | :--- |
| `PORT` | Puerto del servidor local | `3000` | No |
| `DATABASE_URL` | String de conexión a base de datos | `postgresql://user:pass@localhost:5432/db` | Sí |
| `API_SECRET_KEY` | Clave secreta para JWT/auth | `tu_clave_secreta_local` | Sí |

---

## 5. Endpoints y Referencia de API

**Base URL**: `http://localhost:3000/api/v1`

### Grupo: Autenticación
#### `POST /auth/login`
- **Headers**: `Content-Type: application/json`
- **Body**:
  ```json
  {
    "email": "user@example.com",
    "password": "secretpassword"
  }
  ```
- **Respuesta 200 OK**:
  ```json
  {
    "success": true,
    "token": "jwt_token_string"
  }
  ```

---

## 6. Flujo de Trabajo Git y Commits

- Ramas: `feature/[ticket]-[desc]`, `fix/[ticket]-[desc]`
- Commits: Conventional Commits (`feat(...)`, `fix(...)`, `refactor(...)`, etc.)
- Revisión obligatoria antes de merge hacia `main`.
