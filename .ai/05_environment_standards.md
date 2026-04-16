# 05 — Environment Standards (Variables de Entorno y Multi-Entorno)

Define cómo se gestionan las variables de entorno, credenciales y configuración por entorno (local, staging, producción) en todos los proyectos bajo este sistema.

---

## 1. Los Tres Entornos del Sistema

Todo proyecto opera con al menos tres entornos con propósitos distintos:

| Entorno | Propósito | Branch | URL Típica |
|---|---|---|---|
| **Local** | Desarrollo e iteración del dev | cualquier | `http://localhost:[port]` |
| **Staging** | Validación pre-producción, QA, demos | `staging` | `https://staging.[domain].com` |
| **Production** | Sistema en vivo para usuarios reales | `main` | `https://[domain].com` |

> [!IMPORTANT]
> Los tres entornos **NUNCA comparten secrets**. Una API Key de producción nunca aparece en un entorno local ni en un repositorio. Una credencial comprometida en local no debe poder afectar producción.

---

## 2. Archivos de Entorno: Reglas Absolutas

### Los únicos archivos permitidos y su destino:

| Archivo | Commiteado | Contiene | Propósito |
|---|---|---|---|
| `.env.example` | ✅ SÍ | Variables vacías + descripción | Plantilla pública para onboarding |
| `.env` | ❌ NUNCA | Valores reales locales | Entorno local del dev |
| `.env.local` | ❌ NUNCA | Overrides locales adicionales | Personalización local |
| `.env.staging` | ❌ NUNCA | Valores de staging | Inyectado por CI/CD |
| `.env.production` | ❌ NUNCA | Valores de producción | Inyectado por CI/CD o Secrets Manager |

> [!CAUTION]
> Si un archivo `.env` con valores reales aparece en un commit, es una **brecha de seguridad activa**, no un error menor. Se debe revocar inmediatamente la credencial expuesta y rotar todos los secrets afectados, incluso si el commit fue eliminado después (el historial de Git es permanente públicamente).

---

## 3. Estructura del `.env.example` (Plantilla Obligatoria)

El `.env.example` es documentación ejecutable. Debe ser útil sin ser peligroso:

```bash
# =============================================================================
# APPLICATION
# =============================================================================
APP_NAME="[Project Name]"
APP_ENV=local                    # local | staging | production
APP_URL=http://localhost:3000
APP_PORT=3000

# =============================================================================
# DATABASE
# =============================================================================
DB_CONNECTION=postgresql          # postgresql | mysql | sqlite
DB_HOST=localhost
DB_PORT=5432
DB_DATABASE=project_db
DB_USERNAME=project_user
DB_PASSWORD=                      # Required. Min 16 chars recommended.

# =============================================================================
# AUTHENTICATION
# =============================================================================
JWT_SECRET=                       # Required. Generate: openssl rand -hex 32
JWT_EXPIRES_IN=7d

# =============================================================================
# EXTERNAL SERVICES (use sandbox/test keys in local)
# =============================================================================
STRIPE_KEY=pk_test_              # Use test key: pk_test_... (never pk_live_...)
STRIPE_SECRET=sk_test_           # Use test key: sk_test_... (never sk_live_...)
STRIPE_WEBHOOK_SECRET=           # whsec_... from Stripe Dashboard

# =============================================================================
# EMAIL
# =============================================================================
MAIL_DRIVER=smtp                  # smtp | mailhog | log (use log or mailhog locally)
MAIL_HOST=localhost
MAIL_PORT=1025
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_FROM_ADDRESS=noreply@localhost

# =============================================================================
# STORAGE
# =============================================================================
STORAGE_DRIVER=local              # local | s3 (use local in development)
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
```

**Convenciones del `.env.example`:**
- Comentarios de sección con `# ===...===` para separar grupos.
- Cada variable tiene un comentario si su valor no es obvio.
- Los valores de ejemplo son seguros (no resuelven a nada real).
- Los campos requeridos tienen el valor vacío con una nota `# Required.`
- Los campos opcionales tienen un valor por defecto funcional.

---

## 4. Gestión de Secrets por Entorno

### Local
- El dev copia `.env.example` → `.env` y lo llena manualmente.
- Nunca usa credenciales de producción localmente.
- Para servicios de terceros, usa siempre el **modo sandbox/test** del proveedor.

### Staging
- Las variables se inyectan vía el panel de secretos del CI/CD (GitHub Actions Secrets, GitLab CI Variables) o el Secrets Manager del proveedor cloud.
- El pipeline de CI crea el archivo `.env` durante el build, usando los valores del Secrets Manager. Nunca el archivo `.env` se sube al código.

### Production
- Ídem a staging pero con las credenciales reales.
- Uso obligatorio de un Secrets Manager dedicado:
  - **Coolify**: Variables de entorno en el panel de servicios.
  - **AWS**: AWS Secrets Manager o Parameter Store.
  - **GCP**: Secret Manager.
  - **Self-hosted**: HashiCorp Vault o Infisical.
- Los secrets de producción son accesibles solo para el proceso de deploy y el runtime de la aplicación, no para developers individuales.

---

## 5. Convenciones de Nomenclatura de Variables

| Convención | Correcto | Incorrecto |
|---|---|---|
| `SCREAMING_SNAKE_CASE` | `DATABASE_URL` | `databaseUrl`, `database-url` |
| Prefijo por servicio | `STRIPE_KEY`, `AWS_BUCKET` | `KEY`, `BUCKET` |
| Sufijo descriptivo | `JWT_SECRET`, `JWT_EXPIRES_IN` | `JWT_S`, `JWT_E` |
| Sin abreviaciones ambiguas | `MAIL_FROM_ADDRESS` | `MAIL_FA` |

---

## 6. Variables por Entorno: Qué Cambia y Qué No

### ✅ Siempre distintas entre entornos:
- Credenciales de DB (host, usuario, contraseña, nombre de BD)
- API Keys de servicios externos (Stripe, SendGrid, AWS, etc.)
- JWT secrets y claves de cifrado
- URLs base de la aplicación
- Nivel de logging (`debug` en local, `error` en producción)

### ⚠️ Pueden ser iguales pero idealmente distintas:
- Configuración de caché (Redis puede ser local en dev, gestionado en producción)
- Drivers de storage (local vs S3)
- Driver de email (Mailhog local vs SendGrid en producción)

### ✅ Pueden ser idénticas en todos los entornos:
- Nombre de la aplicación (`APP_NAME`)
- Configuración de timezone
- Valores de configuración de negocio que no son secrets

---

## 7. Detección Automática de Secrets Filtrados

Como parte del pipeline CI/CD y de los pre-commit hooks:

### Pre-commit (local):
Instalar `gitleaks` o `detect-secrets` para bloquear commits con patrones de secrets:
```bash
# Instalar gitleaks
# https://github.com/gitleaks/gitleaks
gitleaks protect --staged
```

### Pipeline CI:
```yaml
# GitHub Actions example
- name: Scan for secrets
  uses: gitleaks/gitleaks-action@v2
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

Si gitleaks detecta un pattern de secret en el código, el pipeline falla y el merge se bloquea automáticamente.

---

## 8. Rotación de Secrets (Secret Rotation Policy)

- **Al menos cada 90 días**: Rotar JWT secrets, API keys de servicios críticos.
- **Inmediatamente**: Ante cualquier sospecha de exposición o al salir un miembro del equipo.
- **Al rotar**: Actualizar en el Secrets Manager, verificar que el servicio sigue funcionando, eliminar la key anterior en el proveedor (no solo dejarla de usar).
- **Sin downtime**: Los servicios deben soportar un período de transición de 2 keys simultáneas al rotar para evitar interrupciones.

---

## 9. Checklist de Onboarding de Nuevo Entorno

Cuando se configura un entorno nuevo (nuevo developer, nuevo servidor):

- [ ] Copiar `.env.example` → `.env`
- [ ] Llenar todos los campos marcados como `# Required`
- [ ] Usar credenciales de sandbox/test para servicios de terceros (no producción)
- [ ] Verificar que `.env` está en `.gitignore` antes del primer commit
- [ ] Levantar servicios dependientes (DB, Redis) con Docker si aplica
- [ ] Correr migraciones: `npm run migrate` o equivalente
- [ ] Ejecutar suite de tests para confirmar configuración correcta: `npm test`
- [ ] Confirmar que ninguna variable de producción está en el `.env` local
