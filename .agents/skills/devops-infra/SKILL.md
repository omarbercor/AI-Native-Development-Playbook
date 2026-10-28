---
name: devops-infra
description: >-
  DevOps, CloudPanel server management, self-hosted/local runner CI/CD pipelines, Nginx,
  and Docker containerization specialist. Use this skill when configuring CloudPanel deployments
  (Astro static/SSR, Node.js PM2, WordPress/PHP-FPM), GitHub/Gitea local runners, Nginx reverse
  proxies, Docker Compose v2, or multi-environment secrets management.
---

# DevOps & Infrastructure — CloudPanel, Local Runners CI/CD y Contenedores (10/10)

Esta habilidad estandariza despliegues **sin tiempo de inactividad (*zero-downtime*)**, seguros y reproducibles, priorizando la infraestructura habitual del estudio: **servidores gestionados con CloudPanel + pipelines CI/CD ejecutados en Local / Self-Hosted Runners**, así como entornos contenerizados con Docker Compose v2.

---

## 1. Estándar de Despliegue en CloudPanel (Reglas Invariantes)

En **CloudPanel**, cada sitio corre aislado bajo su propio **Site User** de Linux (`/home/<site-user>/htdocs/<dominio>/`). Ignorar esto rompe permisos (`403/502`). Cumple siempre estas reglas:

1. **Prohibido Desplegar o Compilar como `root`**:
   - Todo archivo en `/home/<site-user>/htdocs/<dominio>/` debe pertenecer estrictamente a `<site-user>:<site-user>`. Si un proceso administrativo toca el directorio, restaura permisos de inmediato.
2. **Arquitectura Según Tipo de Proyecto en CloudPanel**:
   - **Astro Estático / Jamstack**: Compilar fuera del directorio vivo y sincronizar el contenido de `dist/` hacia `htdocs/<dominio>/` servido directo por Nginx con caché de estáticos agresiva.
   - **Astro SSR / Node.js API**: Sitio tipo *Node.js* en CloudPanel (Nginx Reverse Proxy hacia puerto interno ej. `3000`), gestionado con **PM2** (`ecosystem.config.cjs`) bajo el `<site-user>`, haciendo `pm2 reload <app> --update-env` tras cada release.
   - **WordPress / PHP-FPM**: Sitio tipo *WordPress / PHP* en CloudPanel. El CI/CD **solo sincroniza** el tema custom (`wp-content/themes/mi-tema/`) o plugins propios (`wp-content/plugins/mi-plugin/`).
3. **Archivos Intocables en Producción**:
   - Todo comando `rsync` o script de despliegue **debe excluir explícitamente**: `.env`, `wp-config.php`, `wp-content/uploads/`, `wp-content/upgrade/` y `storage/`.
4. **Purga de Caché Post-Deploy**:
   - Si el sitio usa **Varnish** o **Redis Object Cache** en CloudPanel, limpiar caché al finalizar un despliegue exitoso y verificar salud con `curl -fsS`.

---

## 2. Pipelines CI/CD con Local / Self-Hosted Runners

A diferencia de los runners efímeros en la nube, un **Local Runner** persiste estado entre ejecuciones y corre en infraestructura propia:

1. **Higiene del Workspace del Runner**:
   - Usar `npm ci` (o `pnpm install --frozen-lockfile`) para builds deterministas.
   - Limpiar artefactos previos (`rm -rf dist .astro`) antes de compilar para no arrastrar archivos huérfanos de builds anteriores.
2. **Compilar Primero, Sincronizar Después (Atomicidad)**:
   - **Jamás** ejecutes `npm run build` directamente dentro de la carpeta pública viva `htdocs/<dominio>/` (dejaría la web caída o incompleta mientras compila). Compila en el workspace del runner y solo si el build + tests pasan con código `0`, sincroniza a `htdocs/` (o actualiza symlink `releases/<sha>` → `current`).
3. **Seguridad del Runner Local**:
   - Nunca ejecutar workflows de Pull Requests externos no confiables en un self-hosted runner.
   - Prohibido imprimir variables de entorno en los logs (`set -x` sobre secretos o `printenv`). Inyectar credenciales mediante Secrets del repositorio hacia un `.env` fuera del alcance público o variables del servicio PM2.

---

## 3. Contenedores Docker para Desarrollo Local y Microservicios

Cuando el proyecto use Docker local o despliegue contenerizado:

1. **Docker Compose v2 Moderno**:
   - **No incluir `version: '3.8'`** (obsoleto en Compose v2; genera warnings).
   - Usar siempre `healthcheck:` real y `depends_on: { db: { condition: service_healthy } }` para que la app no falle al arrancar antes que la base de datos.
2. **Multi-Stage y Usuario No-Root**:
   - Separar etapa `builder` de `runner`, usar imágenes ligeras (`node:22-alpine`), manejar señales (`dumb-init` / `tini`) y ejecutar con `USER node` (nunca `root`).

---

## 4. Referencia Ejecutable de Pipelines y Configuración

Consulta las plantillas listas para copiar de **GitHub/Gitea Actions con Self-Hosted Runner → CloudPanel (Astro y WordPress)**, PM2 y Docker Compose v2 en:
👉 [cloudpanel-local-runner-cicd.md](./references/cloudpanel-local-runner-cicd.md)
