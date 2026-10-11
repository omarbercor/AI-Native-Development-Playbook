---
name: devops-infra
description: >-
  DevOps, containerization, and infrastructure deployment specialist. Use this skill
  when creating Dockerfiles, docker-compose.yml, Nginx reverse proxy configs, Coolify deployments,
  CI/CD GitHub Actions workflows, or multi-environment secrets management.
---

# DevOps & Infrastructure — Infraestructura, Docker y Despliegues

Esta habilidad define cómo contenerizar, proteger y desplegar aplicaciones de forma predecible y reproducible en entornos locales, staging y producción.

---

## 1. Contenedores Docker (Buenas Prácticas)

1. **Multi-Stage Builds**:
   - Separar la etapa de compilación (*build stage*) de la etapa de ejecución final (*runtime stage*).
   - Mantener la imagen final lo más pequeña posible (usar `alpine` o `slim`).
2. **Ejecución sin Root**:
   - NUNCA ejecutar el proceso dentro del contenedor como usuario `root`. Crear un usuario sin privilegios (`USER node` o `USER app`).
3. **Manejo de Señales (`dumb-init` o `tini`)**:
   - Asegurar que el contenedor reciba y maneje señales `SIGTERM` para cierres limpios y sin colgar procesos zombies.

---

## 2. Docker Compose para Entorno Local

Todo `docker-compose.yml` debe ser reproducible y no exponer secretos:

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    env_file:
      - .env
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    restart: always
    environment:
      POSTGRES_USER: ${DB_USER:-postgres}
      POSTGRES_PASSWORD: ${DB_PASSWORD:-local_dev_password}
      POSTGRES_DB: ${DB_NAME:-app_dev}
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

---

## 3. Estrategia de Multi-Entorno y Despliegues

1. **Tres Entornos Aislados**:
   - **Local**: Máquina del desarrollador. Bases de datos en Docker.
   - **Staging**: Servidor idéntico a producción para validación final.
   - **Producción**: Servidor en vivo. Cero acceso directo no autenticado.
2. **Principio de Inmutabilidad**:
   - La misma imagen de Docker que se prueba en Staging debe ser la que se despliegue en Producción; solo cambian las variables de entorno inyectadas.
