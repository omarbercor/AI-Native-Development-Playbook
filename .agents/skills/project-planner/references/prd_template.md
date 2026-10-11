# Plantilla de PRD (Product Requirements Document)

```markdown
# PRD: [Nombre del Feature o Módulo]

## 1. Problema
[Describe en 1 o 2 párrafos la necesidad, dolor o fricción actual desde la perspectiva del usuario o del negocio.]

## 2. Objetivo
[Qué resultado concreto se logrará cuando este desarrollo esté desplegado en producción.]

## 3. Historias de Usuario
- **Como** [rol/tipo de usuario], **quiero** [acción concreta] **para** [beneficio obtenido].
  - *Happy path*: [Paso a paso del flujo ideal exitoso].
  - *Edge cases*: [Qué ocurre si no hay conexión, si el input es inválido, si no tiene permisos].

## 4. Decisiones Técnicas y Arquitectura
- Tecnologías / librerías a utilizar: [Detalle]
- Flujo de datos o esquema: [Descripción o diagrama]

## 5. Criterios de Aceptación (Definition of Done)
- [ ] Criterio verificable 1
- [ ] Criterio verificable 2
- [ ] Pruebas unitarias/integración pasando sin errores
- [ ] Verificado en entorno móvil y de escritorio
- [ ] Cero secretos o credenciales en código

## 6. Degradación Elegante y Manejo de Errores
- Si [servicio externo] falla: [Comportamiento esperado / Fallback al usuario]
```
