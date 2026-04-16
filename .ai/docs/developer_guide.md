> Last Updated: YYYY-MM-DD | Version: 1.0 | Sprint: [sprint-name]

# [Project Name] — Developer Guide

> **Note for tech-writer**: This document targets external developers integrating with or contributing to the system. Assume they know how to code but know nothing about this project. Replace all `[brackets]` before publishing.

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Local Setup](#local-setup)
4. [Environment Variables](#environment-variables)
5. [System Architecture](#system-architecture)
6. [API Reference](#api-reference)
7. [MCP Servers](#mcp-servers)
8. [Contributing](#contributing)
9. [Deployment](#deployment)
10. [Changelog](#changelog)

---

## Overview

[2-3 sentences describing what this repository builds. Include the tech stack at a high level. Example: "This repository contains the backend API for [Project Name], built with Node.js + Express + PostgreSQL. It exposes REST endpoints consumed by the web frontend and third-party integrations."]

**Tech Stack:**
- **Runtime**: [e.g., Node.js 20 / PHP 8.3 / Python 3.11]
- **Framework**: [e.g., Express / Laravel / FastAPI]
- **Database**: [e.g., PostgreSQL 15]
- **Infrastructure**: [e.g., Docker + Coolify + Nginx]
- **Key Libraries**: [List 3-5 most important ones]

---

## Prerequisites

Exact versions required to run this project locally:

| Tool | Version | Install Link |
|---|---|---|
| [Node.js] | [>= 20.x] | [nodejs.org] |
| [npm] | [>= 10.x] | Bundled with Node |
| [Docker] | [>= 24.x] | [docker.com] |
| [Tool N] | [version] | [link] |

---

## Local Setup

From zero to running locally in under 30 minutes.

### 1. Clone the repository

```bash
git clone https://github.com/[org]/[repo].git
cd [repo]
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

```bash
cp .env.example .env
```

Open `.env` and fill in the required values. See [Environment Variables](#environment-variables) for details.

### 4. Start the database

```bash
docker compose up -d db
```

**Expected output**:
```
✔ Container [project]-db-1  Started
```

### 5. Run database migrations

```bash
npm run migrate
```

### 6. Start the development server

```bash
npm run dev
```

**Expected output**:
```
Server running at http://localhost:3000
Database connected ✔
```

The API is now available at `http://localhost:3000/api/v1/`.

---

## Environment Variables

All required variables are listed in `.env.example`. **Never commit `.env` with real values.**

| Variable | Description | Example | Required |
|---|---|---|---|
| `DATABASE_URL` | PostgreSQL connection string | `postgresql://user:pass@localhost:5432/dbname` | ✅ Yes |
| `JWT_SECRET` | Secret key for signing JWT tokens | `a-long-random-string` | ✅ Yes |
| `PORT` | Port the server listens on | `3000` | ❌ No (default: 3000) |
| `[VAR_NAME]` | [Description] | `[example value]` | [Yes/No] |

---

## System Architecture

```mermaid
graph TD
    Client[Client / Frontend] -->|HTTPS| Gateway[API Gateway / Nginx]
    Gateway --> App[Application Server]
    App --> DB[(PostgreSQL)]
    App --> Queue[Redis Queue]
    Queue --> Worker[Background Worker]
    Worker --> External[External Services]
```

### Key Design Decisions

- **[Decision 1]**: [Brief justification — link to ADR if available]
- **[Decision 2]**: [Brief justification]
- **[Decision 3]**: [Brief justification]

---

## API Reference

**Base URL**: `https://api.[domain].com/api/v1`

**Authentication**: All endpoints require a Bearer token in the `Authorization` header unless marked as public.

```http
Authorization: Bearer <your-jwt-token>
```

---

### Authentication

#### `POST /auth/login`

Authenticates a user and returns a JWT token.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "your-password"
}
```

**Response `200 OK`:**
```json
{
  "success": true,
  "token": "eyJhbGci...",
  "expires_at": "2026-04-17T00:00:00Z"
}
```

**Error Responses:**
| Status | Code | Description |
|---|---|---|
| `401` | `AUTH_INVALID_CREDENTIALS` | Email or password is incorrect |
| `429` | `RATE_LIMIT_EXCEEDED` | Too many requests |

---

#### `POST /auth/refresh`

Refreshes an expiring JWT token.

[Continue pattern for each endpoint group...]

---

## MCP Servers

[Only include this section if the project exposes MCP tools.]

Available MCP servers for AI agent integration:

| MCP | Identifier | Tools Exposed | Authorized Role |
|---|---|---|---|
| `mcp/ssh-mcp/` | `ssh-wordpress` | `execute_ssh_command` | `devops-sysadmin` |
| `mcp/coolify-mcp/` | `coolify` | `deploy_service`, `get_service_status` | `devops-sysadmin` |

Setup instructions:
1. Copy `.env.example` to `.env` inside each MCP folder and fill in values.
2. Register each MCP in your agent's `mcp.json` using the snippet in each MCP's `README.md`.

---

## Contributing

### Branching Strategy

```
main          ← Production-ready code. Protected.
staging       ← Pre-production testing.
feature/[name] ← New features. Branch from main.
fix/[name]    ← Bug fixes. Branch from main.
```

### Pull Request Process

1. Branch from `main` using the naming convention above.
2. Ensure all tests pass locally: `npm test`
3. Ensure linting passes: `npm run lint`
4. Open a PR against `main` with a clear description of what changed and why.
5. At least one review required before merge.
6. PRs with failing CI checks are not mergeable.

### Commit Convention

Follow [Conventional Commits](https://www.conventionalcommits.org/):
```
feat(auth): add refresh token rotation
fix(payments): handle Stripe timeout gracefully
docs(developer): update environment variables table
```

---

## Deployment

### Staging

```bash
# Triggered automatically on merge to staging branch via CI/CD
# Manual deploy:
npm run deploy:staging
```

### Production

```bash
# Triggered automatically on merge to main branch via CI/CD
# Requires approval from a team lead before execution
```

**Post-deploy verification**:
```bash
curl https://api.[domain].com/api/v1/health
# Expected: { "status": "ok", "version": "x.x.x" }
```

---

## Changelog

| Version | Date | Summary |
|---|---|---|
| 1.0.0 | YYYY-MM-DD | Initial release |
