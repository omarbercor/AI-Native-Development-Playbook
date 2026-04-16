# 04 — Git Workflow Standards

Define el flujo de trabajo con Git para todos los proyectos bajo este sistema. Todo el equipo sigue estas convenciones sin excepción para garantizar trazabilidad, reversibilidad y colaboración sin fricciones.

---

## 1. Estrategia de Ramas (Branching Strategy)

```
main ──────────────────────────────────────── Producción. Siempre deployable.
  └── staging ─────────────────────────────── Pre-producción. Validación final.
        └── feature/[ticket]-[descripcion]  ← Nuevas funcionalidades
        └── fix/[ticket]-[descripcion]      ← Corrección de bugs
        └── hotfix/[ticket]-[descripcion]   ← Fix urgente directo a main
        └── chore/[descripcion]             ← Mantenimiento sin impacto funcional
```

### Reglas por Rama

| Rama | Se crea desde | Merge hacia | Eliminar post-merge |
|---|---|---|---|
| `feature/*` | `main` | `staging` → `main` | ✅ Sí |
| `fix/*` | `main` | `staging` → `main` | ✅ Sí |
| `hotfix/*` | `main` | `main` + `staging` | ✅ Sí |
| `chore/*` | `main` | `main` | ✅ Sí |
| `staging` | `main` | — | ❌ Permanente |
| `main` | — | — | ❌ Permanente. Protegida. |

> [!CAUTION]
> **Nadie hace push directo a `main` ni a `staging`.** Todo cambio entra por Pull Request con al menos una revisión aprobada y todos los checks de CI en verde.

---

## 2. Nomenclatura de Ramas

El nombre de la rama comunica intención y trazabilidad:

```
[tipo]/[id-ticket]-[descripcion-corta-en-kebab-case]
```

**Ejemplos correctos:**
```
feature/PAY-42-stripe-checkout-flow
fix/AUTH-17-refresh-token-rotation
hotfix/CRITICAL-01-null-pointer-on-login
chore/update-node-20
```

**Ejemplos incorrectos:**
```
mi-cambio        ← Sin tipo ni contexto
feature/cosas    ← No describe el cambio
FEATURE/Login    ← Mayúsculas y sin ticket
```

---

## 3. Convención de Commits (Conventional Commits)

Todo commit sigue el estándar [Conventional Commits](https://www.conventionalcommits.org/). Definido también en `00_project_core.md §9`.

```
<tipo>(<scope>): <descripción imperativa en presente>

[cuerpo opcional]
[footer opcional: BREAKING CHANGE, Closes #issue]
```

| Tipo | Uso |
|---|---|
| `feat` | Nueva funcionalidad visible al usuario |
| `fix` | Corrección de bug |
| `refactor` | Mejora interna sin cambio de comportamiento |
| `perf` | Optimización de rendimiento |
| `test` | Añadir o corregir tests |
| `docs` | Solo documentación |
| `chore` | Dependencias, configuración, mantenimiento |
| `ci` | Cambios en pipeline CI/CD |
| `style` | Formato, espaciado (sin cambio de lógica) |
| `revert` | Revertir un commit anterior |

**Breaking changes** se señalan con `!` después del tipo o con footer `BREAKING CHANGE:`:
```
feat(api)!: remove deprecated v1 endpoints

BREAKING CHANGE: /api/v1/users no longer exists. Migrate to /api/v2/users.
```

---

## 4. Proceso de Pull Request (PR)

### Checklist obligatorio antes de abrir un PR:

- [ ] Todos los tests pasan localmente (`npm test` / `php artisan test`)
- [ ] Linting sin errores (`npm run lint`)
- [ ] Sin archivos `.env` con secrets en el diff
- [ ] Sin `console.log`, `dd()`, `var_dump()`, o código de debug
- [ ] Sin TODOs bloqueantes en el código entregado
- [ ] El título del PR sigue el formato Conventional Commits
- [ ] La descripción explica **qué** cambia y **por qué**
- [ ] Las ramas de dependencia están mergeadas primero

### Template de descripción de PR:

```markdown
## What
[Descripción breve de qué cambia en este PR.]

## Why
[Contexto: por qué este cambio es necesario. Link al ticket si existe.]

## How
[Decisiones técnicas no obvias tomadas en esta implementación.]

## Testing
- [ ] Tests unitarios añadidos/actualizados
- [ ] Probado en local
- [ ] Caso edge X contemplado

## Screenshots (si hay cambios de UI)
```

### Reglas de revisión:

- **Mínimo 1 aprobación** requerida para merge en `staging`.
- **Mínimo 2 aprobaciones** para merge en `main` en proyectos de equipo.
- El autor no puede aprobar su propio PR.
- Los comentarios de revisión se resuelven antes del merge, no después.
- Un PR con CI en rojo no se mergea, sin excepciones.

---

## 5. Estrategia de Merge

| Destino | Estrategia | Por qué |
|---|---|---|
| `staging` ← `feature/*` | **Squash and Merge** | Mantiene historial limpio en staging |
| `main` ← `staging` | **Merge Commit** | Preserva el contexto del sprint entero |
| `main` ← `hotfix/*` | **Merge Commit** | Trazabilidad del fix urgente |

> **Nunca** `git rebase` en ramas compartidas (`staging`, `main`). Solo en ramas locales propias antes de abrir el PR.

---

## 6. Protección de Ramas (Branch Protection Rules)

Configurar en GitHub/GitLab al crear el repositorio:

### Para `main`:
- ✅ Require pull request before merging
- ✅ Require approvals: 1 (mínimo)
- ✅ Dismiss stale reviews when new commits are pushed
- ✅ Require status checks to pass (CI pipeline)
- ✅ Require branches to be up to date before merging
- ✅ Include administrators
- 🚫 Allow force pushes: **OFF**
- 🚫 Allow deletions: **OFF**

### Para `staging`:
- ✅ Require pull request before merging
- ✅ Require status checks to pass
- 🚫 Allow force pushes: **OFF**

---

## 7. Tagging y Releases

Las versiones siguen [Semantic Versioning](https://semver.org/): `MAJOR.MINOR.PATCH`

| Escenario | Bump |
|---|---|
| Bug fix sin breaking change | `PATCH` → `1.0.1` |
| Nueva funcionalidad sin breaking change | `MINOR` → `1.1.0` |
| Breaking change (API incompatible) | `MAJOR` → `2.0.0` |

**Crear un tag de release:**
```bash
git tag -a v1.2.0 -m "feat: add payment module with Stripe integration"
git push origin v1.2.0
```

Cada tag de release va acompañado de un entry en el `CHANGELOG.md` o en las Release Notes de GitHub.

---

## 8. Hotfixes (Emergencias en Producción)

Cuando hay un bug crítico en producción que no puede esperar al siguiente sprint:

```
1. Crear rama: git checkout -b hotfix/CRITICAL-01-descripcion main
2. Implementar el fix mínimo necesario (no aprovechar para añadir features)
3. Abrir PR urgente hacia main (requiere mínimo 1 aprobación)
4. Después del merge a main: merge también a staging para sincronizar
5. Tag de patch release: v1.0.1
6. Documentar el incidente en team_reference.md §Incident Runbook
```

> [!CAUTION]
> Un hotfix es cirugía de emergencia, no una oportunidad de refactor. El cambio debe ser el mínimo posible para resolver el problema.

---

## 9. Comandos de Referencia Rápida

```bash
# Crear rama de feature
git checkout -b feature/TICKET-123-descripcion

# Sincronizar con main antes de abrir PR
git fetch origin
git rebase origin/main

# Ver estado limpio antes de commit
git status
git diff --staged

# Commit con formato correcto
git commit -m "feat(auth): add refresh token with rotation"

# Crear tag de release
git tag -a v1.2.0 -m "feat: payment module"
git push origin v1.2.0

# Eliminar rama remota post-merge
git push origin --delete feature/TICKET-123-descripcion
```
