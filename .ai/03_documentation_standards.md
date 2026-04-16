# 03 — Estándares de Documentación (Documentation Standards)

Define cómo, cuándo y para quién se produce documentación en este proyecto. La documentación no es un afterthought al final del sprint; es un entregable con la misma jerarquía que el código.

---

## 1. Principio Fundamental: Tres Audiencias, Tres Documentos

Cada proyecto desarrollado bajo este sistema produce **tres documentos distintos**, uno por audiencia. No existe un documento universal que sirva a todos:

| Documento | Audiencia | Lenguaje | Ubicación |
|---|---|---|---|
| `user_guide.md` | Usuario final del sistema | No técnico, orientado a tareas | `.ai/docs/user_guide.md` |
| `developer_guide.md` | Dev externo que integra o contribuye | Técnico, orientado a setup y API | `.ai/docs/developer_guide.md` |
| `team_reference.md` | Dev interno del equipo | Técnico profundo, decisiones y contexto | `.ai/docs/team_reference.md` |

> [!IMPORTANT]
> El rol responsable de producir y mantener estos documentos es `tech-writer`. El orquestador lo activa al cerrar cada sprint que modifique funcionalidad visible al usuario o contratos de API.

---

## 2. Cuándo se Escribe Documentación (Triggers Obligatorios)

La documentación se actualiza ante cualquiera de estos eventos:

| Trigger | Documentos Afectados |
|---|---|
| Nueva funcionalidad visible al usuario | `user_guide.md` |
| Nuevo endpoint o cambio en contrato de API | `developer_guide.md` |
| Decisión arquitectónica (ADR) tomada | `team_reference.md` |
| Nueva variable de entorno o dependencia | `developer_guide.md` |
| Cambio en el flujo de onboarding/setup | `developer_guide.md` |
| Bug crítico corregido con cambio de comportamiento | Los tres documentos (según impacto) |
| Nuevo MCP creado | `team_reference.md` + `developer_guide.md` |
| Sprint cerrado con features completos | Revisión de los tres documentos |

---

## 3. Estándar del User Guide (`user_guide.md`)

**Audiencia**: Persona que usa el sistema. No necesariamente técnica. Puede ser cliente, operador o usuario interno no-dev.

**Principios de escritura:**
- Lenguaje en segunda persona activa: "Haz clic en...", "Navega a...", "Introduce tu..."
- Sin jerga técnica. Si se necesita un término técnico, se define en un glosario al final.
- Orientado a tareas, no a features. El usuario quiere lograr algo, no conocer el sistema.
- Capturas de pantalla o diagramas de flujo para acciones complejas.

**Estructura obligatoria:**
```
1. ¿Qué hace este sistema? (1 párrafo, lenguaje de beneficio)
2. Primeros Pasos (Quickstart — de cero a usando en < 5 minutos)
3. Guías de Uso por Flujo (una sección por flujo principal)
4. Preguntas Frecuentes (FAQ)
5. Solución de Problemas Comunes (Troubleshooting)
6. Glosario (solo si hay términos inevitables)
```

**Criterio de calidad:**
- Una persona sin conocimiento técnico puede completar el flujo principal leyendo solo este documento.
- Todas las pantallas, formularios o acciones descritas existen en el sistema actual (cero documentación fantasma).

---

## 4. Estándar del Developer Guide (`developer_guide.md`)

**Audiencia**: Developer externo que quiere integrar, contribuir o desplegar el sistema. Conoce código pero no conoce este proyecto.

**Principios de escritura:**
- Comandos copiables y ejecutables directamente (no pseudocódigo).
- Variables de entorno documentadas con nombre, descripción, ejemplo y si son obligatorias u opcionales.
- Flujos de autenticación y autorización explicados paso a paso.
- Arquitectura de alto nivel en diagrama Mermaid.

**Estructura obligatoria:**
```
1. Overview (¿Qué construye este repositorio?)
2. Prerrequisitos (versiones exactas: Node 20, PHP 8.3, etc.)
3. Instalación y Setup Local (de cero a running en < 30 min)
4. Variables de Entorno (tabla: nombre | descripción | ejemplo | requerida)
5. Arquitectura del Sistema (diagrama Mermaid de componentes)
6. API Reference (endpoints, métodos, parámetros, ejemplos de request/response)
7. MCPs Disponibles (si aplica)
8. Guía de Contribución (branching strategy, PR process, coding standards)
9. Deployment (cómo desplegar a staging y producción)
10. Changelog (últimos cambios significativos)
```

**Criterio de calidad:**
- Un developer que nunca vio el proyecto puede tener el entorno local corriendo siguiendo este documento, sin preguntar nada.
- Todos los comandos del documento funcionan en el entorno descrito (se prueba en cada release).

---

## 5. Estándar del Team Reference (`team_reference.md`)

**Audiencia**: Developer interno del equipo. Conoce el proyecto, necesita contexto profundo de decisiones, patrones internos y estado de la arquitectura.

**Principios de escritura:**
- Técnico y directo. Sin explicar conceptos básicos que el equipo ya conoce.
- Enfocado en el "por qué" de las decisiones, no solo el "qué".
- Actualizado en cada sprint como parte del cierre.
- Incluye deuda técnica activa y su impacto estimado.

**Estructura obligatoria:**
```
1. Estado Actual de la Arquitectura (diagrama actualizado)
2. Decisiones Arquitectónicas (ADRs activos y su justificación)
3. Mapa de Módulos y Responsabilidades
4. Patterns y Convenciones Internas del Proyecto
5. MCPs Activos (tabla con nombre, tools expuestos, rol autorizado)
6. Deuda Técnica Activa (IDs, descripción, prioridad, impacto)
7. Entornos y Configuración de Infraestructura
8. Runbook de Incidentes (pasos para diagnosticar los problemas más frecuentes)
9. Contactos y Ownership por Módulo
```

**Criterio de calidad:**
- Un dev del equipo que regresa de vacaciones puede ponerse al día leyendo solo este documento.
- La deuda técnica está documentada con suficiente contexto para que cualquier dev del equipo pueda atacarla sin preguntar.

---

## 6. Reglas Globales de Formato

Aplican a los tres documentos sin excepción:

- **Idioma**: Los documentos de esta carpeta se escriben en **inglés** por convención de industria y para facilitar colaboración internacional.
- **Diagramas**: Se usan bloques Mermaid dentro del Markdown. No se enlaza a herramientas externas (Figma, Miro, Lucidchart) como única fuente.
- **Código**: Todos los bloques de código especifican el lenguaje (`\`\`\`bash`, `\`\`\`json`, `\`\`\`javascript`).
- **Versiones**: Cada documento tiene un campo `Last Updated` y `Version` al inicio.
- **Sin Placeholders**: Cero textos del tipo `[TODO: completar esto]` o `[Ejemplo a definir]` en documentos publicados.
- **Links Internos**: Los links entre documentos son rutas relativas, no absolutas, para que funcionen en cualquier entorno.

---

## 7. Ciclo de Vida de la Documentación

```
[Feature completado y verificado por qa-automation]
        ↓
[orchestrator-pm activa tech-writer]
        ↓
[tech-writer identifica qué documentos impacta el cambio]
        ↓
[tech-writer actualiza los documentos correspondientes en .ai/docs/]
        ↓
[Commit: docs([audiencia]): update [sección] for [feature]]
        ↓
[active_context.md refleja que la doc está actualizada]
```

---

## 8. Convención de Commits para Documentación

```
docs(user):      Cambios en user_guide.md
docs(developer): Cambios en developer_guide.md
docs(team):      Cambios en team_reference.md
docs(all):       Cambios que afectan los tres documentos
```

---

## 9. Anti-Patterns de Documentación (Prohibido)

- 🚫 **Documentación fantasma**: Describir features que no existen o fueron eliminados.
- 🚫 **Screenshots desactualizados**: Una imagen de una UI que ya no existe confunde más que ayuda.
- 🚫 **Comandos sin probar**: Un comando en el `developer_guide.md` que no funciona destruye la confianza en toda la documentación.
- 🚫 **Documentación solo en el chat**: Cualquier decisión, acuerdo o instrucción que solo existe en una conversación de chat es conocimiento perdido.
- 🚫 **Mezclar audiencias**: No escribir en `user_guide.md` pensando en developers, ni en `developer_guide.md` pensando en usuarios finales.
