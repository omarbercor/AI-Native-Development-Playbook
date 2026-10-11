---
name: documentation-writer
description: >-
  Generates and maintains clear technical documentation for three distinct audiences:
  End-User Guide, External Developer Guide, and Internal Team Reference. Use this skill
  when documenting a completed sprint, writing onboarding docs, or creating API references.
---

# Documentation Writer — Especialista en Documentación Multiaudiencia

La documentación de calidad no es un añadido opcional al final; es un activo de ingeniería de primer nivel. Un mismo documento no sirve para todos los lectores. Esta habilidad estructura la documentación en tres capas según la audiencia.

---

## 1. Las Tres Audiencias Obligatorias

| Documento | Audiencia | Enfoque | Lenguaje |
| :--- | :--- | :--- | :--- |
| **User Guide** | Clientes o usuarios finales de la app/sitio. | Orientado a tareas y flujos de valor ("Cómo lograr X"). | Claro, sin jerga técnica, con capturas o pasos directos. |
| **Developer Guide** | Desarrolladores externos o contribuidores. | Setup local, variables de entorno, APIs y arquitectura. | Técnico, con ejemplos de código y contratos OpenAPI. |
| **Team Reference** | El equipo interno y futuros agentes. | Decisiones de arquitectura (ADRs), esquemas de BD, deuda técnica. | Técnico profundo, decisiones y justificaciones. |

---

## 2. Plantillas Disponibles

Utiliza las plantillas estandarizadas para generar o actualizar cada documento:
- 👉 [user_guide.md](./templates/user_guide.md)
- 👉 [developer_guide.md](./templates/developer_guide.md)
- 👉 [team_reference.md](./templates/team_reference.md)

---

## 3. Criterios de Calidad de Documentación

1. **Reemplazo total de placeholders**: Nunca dejes corchetes como `[Nombre del Proyecto]` o `[Comando]` en el documento final.
2. **Ejemplos ejecutables**: Todo comando o snippet de código debe haber sido probado y ser funcional.
3. **Mantenimiento continuo**: Si se añade una nueva variable de entorno o endpoint, actualizar inmediatamente el Developer Guide.
