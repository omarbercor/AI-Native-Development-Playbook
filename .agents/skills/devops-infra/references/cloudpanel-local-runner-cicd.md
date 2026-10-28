# Blueprints: CloudPanel + Self-Hosted Runner CI/CD y Docker Compose v2

Plantillas listas para producción adaptadas a **Local / Self-Hosted Runners** desplegando hacia **CloudPanel** (Astro Estático/SSR y WordPress) y entornos locales con Docker.

---

## 1. Workflow CI/CD: Astro / Node → CloudPanel (`self-hosted` runner)

Compila en el workspace aislado del runner local, protege `.env` y despliega atómicamente bajo el usuario del sitio en CloudPanel:

```yaml
# .github/workflows/deploy-cloudpanel.yml
name: Deploy to CloudPanel (Local Runner)

on:
  push:
    branches: [main]

concurrency:
  group: production-deploy
  cancel-in-progress: false

jobs:
  build-and-deploy:
    runs-on: self-hosted
    env:
      SITE_USER: mi-sitio-user
      SITE_DOMAIN: midominio.com
      APP_PM2_NAME: midominio-ssr # Omitir si es sitio 100% estático
    steps:
      - name: Checkout código
        uses: actions/checkout@v4

      - name: Instalar dependencias limpias y auditar build
        run: |
          rm -rf dist .astro
          npm ci
          npm run build

      - name: Sincronizar artefactos a CloudPanel htdocs (Zero-Clobber)
        run: |
          TARGET="/home/${SITE_USER}/htdocs/${SITE_DOMAIN}"
          rsync -az --delete \
            --exclude='.env' \
            --exclude='.well-known/' \
            ./dist/ "${TARGET}/dist/"

      - name: Recargar proceso PM2 (Solo si es Astro SSR / Node API)
        run: |
          sudo -u "${SITE_USER}" pm2 reload "${APP_PM2_NAME}" --update-env || true

      - name: Healthcheck HTTP Post-Deploy
        run: |
          curl -fsS --max-time 10 "https://${SITE_DOMAIN}" > /dev/null
```

> [!NOTE]
> **Si el runner está en una máquina distinta al VPS de CloudPanel**, cambia el `rsync` local por `rsync -az --delete -e "ssh -i ~/.ssh/deploy_key -o StrictHostKeyChecking=accept-new" ./dist/ ${SITE_USER}@${SERVER_HOST}:/home/${SITE_USER}/htdocs/${SITE_DOMAIN}/`.

---

## 2. Paso de Sincronización para WordPress en CloudPanel

En proyectos WordPress, **jamás** hagas `rsync --delete` sobre la raíz de `htdocs/<dominio>/`. Sincroniza únicamente tu tema o plugin custom:

```bash
# Sincronizar solo el tema custom compilado sin tocar core, wp-config ni uploads
THEME_TARGET="/home/${SITE_USER}/htdocs/${SITE_DOMAIN}/wp-content/themes/mi-tema-custom/"

rsync -az --delete \
  --exclude='.git*' \
  --exclude='node_modules/' \
  --exclude='.env' \
  ./wp-content/themes/mi-tema-custom/ "${THEME_TARGET}"

chown -R "${SITE_USER}:${SITE_USER}" "${THEME_TARGET}"
```

---

## 3. Configuración PM2 Limpia para CloudPanel (`ecosystem.config.cjs`)

Para sitios Node.js / Astro SSR corriendo detrás del Reverse Proxy Nginx de CloudPanel:

```javascript
module.exports = {
  apps: [
    {
      name: 'midominio-ssr',
      script: './dist/server/entry.mjs',
      instances: 'max',
      exec_mode: 'cluster',
      max_memory_restart: '512M',
      env: {
        NODE_ENV: 'production',
        HOST: '127.0.0.1',
        PORT: 3000,
      },
    },
  ],
};
```

---

## 4. `docker-compose.yml` Moderno (Compose v2 con Healthchecks)

Para levantar bases de datos o servicios auxiliares en desarrollo sin `version:` obsoleto ni condiciones de carrera al iniciar:

```yaml
services:
  app:
    build:
      context: .
      target: runner
    ports:
      - "3000:3000"
    env_file:
      - .env
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: ${DB_USER:-app_user}
      POSTGRES_PASSWORD: ${DB_PASSWORD:-local_dev_only}
      POSTGRES_DB: ${DB_NAME:-app_dev}
    ports:
      - "127.0.0.1:5432:5432"
    volumes:
      - pg_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U $${POSTGRES_USER} -d $${POSTGRES_DB}"]
      interval: 5s
      timeout: 3s
      retries: 5

volumes:
  pg_data:
```

