# 00 — Núcleo del Sistema IA (Project Core)

**Directrices Universales del Workspace.** Todos los agentes IA y sub-agentes subordinan su comportamiento a este archivo de forma absoluta y sin excepción. En caso de conflicto entre este documento y cualquier otro, este documento gana.

---

## Jerarquía de Autoridad de Documentos

```
00_project_core.md              ← Ley Suprema (este archivo)
01_planning_and_architecture.md ← Estrategia y PRD
02_mcp_standards.md             ← Estándares de herramientas MCP
03_documentation_standards.md  ← Estándares de entrega de documentación
04_git_standards.md             ← Flujo de trabajo Git y branching
05_environment_standards.md     ← Variables de entorno y multi-entorno
.ai/roles/*.md                  ← Reglas locales por especialidad
active_context.md               ← Estado vivo del sprint
```

Ningún rol, ningún sub-agente y ninguna instrucción de conversación puede anular lo que aquí se define.

---

## 1. Protocolo de Inicio de Sesión (Onboarding Obligatorio)

Antes de ejecutar cualquier tarea, todo agente IA que inicia una sesión DEBE:

1. **Leer `active_context.md`**: Entender el estado actual del proyecto, los logros previos, los errores pendientes y los próximos pasos definidos.
2. **Leer `00_project_core.md`** (este archivo): Reconfirmar las directrices universales.
3. **Identificar el rol activo**: Determinar bajo qué rol debe operar según la solicitud del usuario.
4. **Consultar el rol correspondiente**: Leer el archivo `.ai/roles/[nombre_rol].md` antes de ejecutar tareas propias de ese dominio.

> [!CAUTION]
> Una IA que actúa sin leer el contexto activo genera fragmentación de trabajo y deuda técnica. Este paso no es opcional aunque el usuario pida "ir directo al código".

---

## 2. El Flujo de Trabajo Maestro (Pipeline de Ejecución)

```
[Usuario] → Solicitud
    ↓
[orchestrator-pm] → Descompone, prioriza y rutea
    ↓
[Sub-Agente Especialista] → Ejecuta en su dominio con sus reglas locales
    ↓
[orchestrator-pm] → Verifica Definition of Done
    ↓
[Cierre Atómico] → Commit semántico + update active_context.md
```

Cada paso es obligatorio. Saltar el orquestador para ir directo a la implementación está prohibido salvo que el usuario lo solicite explícitamente en una tarea trivial (< 10 líneas de cambio).

---

## 3. Principios de Ingeniería Universales (Aplican a Todos los Roles)

Estos principios son transversales. No pertenecen a un rol; son la base de cualquier output técnico generado por cualquier agente:

### 3.1 DRY — Don't Repeat Yourself
Si una lógica, componente, consulta SQL o bloque de configuración aparece dos veces en el codebase, es candidato inmediato a abstracción. La duplicación es deuda técnica, no conveniencia.

### 3.2 KISS — Keep It Simple, Stupid
La solución más simple que cumpla los requerimientos es la correcta. La complejidad no solicitada es un bug disfrazado de sofisticación. Si no puedes explicar la solución en 2 oraciones, probablemente está sobredimensionada.

### 3.3 YAGNI — You Aren't Gonna Need It
No se implementa funcionalidad especulativa. El código que "podríamos necesitar en el futuro" es código muerto que alguien tendrá que mantener y entender. Solo se construye lo que el requerimiento actual exige.

### 3.4 Fail Fast, Fail Loud
Los errores se detectan y reportan lo antes posible en el flujo de ejecución. Un error silencioso que deja el sistema en estado inconsistente es peor que un crash inmediato. Validar en el borde, lanzar excepciones estructuradas, logear con contexto.

### 3.5 Single Source of Truth
Cada dato, configuración o estado tiene exactamente un lugar donde vive. Si el color primario de la marca está en tres archivos, no hay una fuente de verdad; hay tres fuentes de inconsistencia. Tokens CSS, variables de entorno, schemas de BD: una sola fuente.

---

## 4. Política de Seguridad Global (Zero-Trust por Defecto)

Independientemente del rol activo, estas reglas de seguridad son absolutas:

- **Cero Secretos en Código**: API Keys, tokens, contraseñas y certificados NUNCA se hardcodean en archivos commiteados. Se usan variables de entorno, Secrets Managers o vaults. Sin excepción.
- **Validación en el Borde**: Todo input externo (usuario, API, webhook) se valida antes de procesarse. El origen nunca garantiza la confiabilidad del dato.
- **Principio de Mínimo Privilegio**: Procesos, usuarios de BD, contenedores y tokens de API operan con los permisos mínimos necesarios para su función. Los permisos no se otorgan por conveniencia.
- **Superficie de Ataque Mínima**: Si un puerto, endpoint o permiso no se necesita activamente, no se expone. La seguridad por oscuridad no es seguridad; la reducción de superficie, sí.
- **Auditoría de Dependencias**: Antes de incorporar una librería de terceros, se verifica: última actualización, CVEs conocidos, y si existe alternativa nativa con menor footprint.

---

## 5. Convenciones de Código (Estilo Universal)

Reglas que aplican a todos los lenguajes y todos los roles:

| Regla | Criterio |
|---|---|
| **Naming Semántico** | Los nombres describen intención, no implementación. `fetchUserBillingDetails()` > `getData()`. |
| **Funciones Pequeñas** | Una función hace una sola cosa. Si tiene más de 20 líneas, probablemente hace dos cosas. |
| **Comentarios de "Por qué"** | El código explica el "qué". Los comentarios explican el "por qué" (decisiones no obvias). |
| **Sin Código Muerto** | Variables no usadas, imports no usados y funciones unreachable se eliminan antes del commit. |
| **Consistencia sobre Preferencia** | Si el proyecto ya usa una convención (camelCase, tabs vs spaces), se respeta aunque no sea la favorita personal del agente. |
| **Early Return** | Validar y retornar temprano. El happy-path corre en el nivel más bajo de indentación. |

---

## 6. Gestión de la Memoria Persistente (active_context.md)

El archivo `active_context.md` es la RAM del sistema. Sus reglas son:

- **Lectura obligatoria al inicio**: Ver punto §1.
- **Escritura obligatoria al final**: Antes de dar cualquier tarea importante por cerrada, el orquestador actualiza este archivo.
- **No es un log histórico**: Solo contiene el estado ACTUAL. Lo que ya está resuelto y cerrado se elimina para no contaminar el contexto futuro.
- **Formato estructurado**: Siempre incluye: objetivo del sprint, logros de la sesión, errores pendientes, deuda técnica y próximos pasos.
- **Deuda Técnica Visible**: Los workarounds, decisiones pragmáticas bajo presión y limitaciones conocidas se documentan aquí explícitamente. Ocultarlos es acumular deuda invisible.

---

## 7. Economía de Tokens y Comunicación Eficiente

- **Sin Verbosidad Decorativa**: Cero saludos, confirmaciones empáticas ni redundancias del tipo "Entendido, con mucho gusto procedo a...". La salida es: diagnóstico, código, o preguntas concretas.
- **Ediciones por Delta**: Para modificar archivos existentes se usan herramientas de edición por chunks/reemplazo. Devolver el archivo completo en chat cuando el cambio es de pocas líneas está prohibido.
- **Preguntas Mínimas y Suficientes**: Si la tarea es ambigua, se hacen las preguntas mínimas necesarias para eliminar la ambigüedad, no un cuestionario exhaustivo.
- **Código sin Placeholders**: Ningún bloque de código entregado al usuario contiene `// TODO`, `// implementar aquí` o datos de ejemplo sin reemplazar. El output es funcional o especifica explícitamente qué falta y por qué.

---

## 8. Ecosistema de Roles (Roster del Equipo)

El directorio `.ai/roles/` contiene los sub-agentes especializados del equipo. El orquestador los activa según la naturaleza de la tarea:

| Archivo | Rol | Dominio Principal |
|---|---|---|
| `orchestrator_pm.md` | Orquestador PM | Planificación, ruteo, cierre de sprint |
| `backend_architect.md` | Backend Architect | APIs, BD, auth, contratos de datos |
| `devops_sysadmin.md` | DevOps SysAdmin | Infra, Docker, CI/CD, redes |
| `frontend_engineer.md` | Frontend Engineer | UI/UX, animaciones, componentes, rendimiento |
| `qa_automation.md` | QA Automation | Testing, cobertura, mocks, performance |
| `seo_expert.md` | SEO Expert | On-page SEO, schema, CWV, visibilidad orgánica |
| `tech_writer.md` | Tech Writer | Documentación de usuario, dev y equipo interno |

> [!NOTE]
> Este roster no es fijo. El orquestador tiene autoridad para proponer nuevos roles cuando el proyecto lo requiera. Cualquier nuevo rol sigue el estándar de formato: frontmatter YAML + perfil + herramientas autorizadas + mínimo 8 directrices de excelencia.

---

## 9. Estándar de Versionado (Conventional Commits)

Todo commit generado por el sistema IA sigue el estándar [Conventional Commits](https://www.conventionalcommits.org/):

```
<tipo>(<alcance>): <descripción imperativa en presente>

[cuerpo opcional: qué cambió y por qué]
[roles involucrados]
```

**Tipos válidos:**

| Tipo | Cuándo usarlo |
|---|---|
| `feat` | Nueva funcionalidad visible al usuario |
| `fix` | Corrección de bug |
| `refactor` | Mejora interna sin cambio de comportamiento |
| `chore` | Mantenimiento, dependencias, configuración |
| `docs` | Cambios exclusivos en documentación |
| `test` | Adición o corrección de tests |
| `perf` | Optimización de rendimiento |
| `ci` | Cambios en pipeline de CI/CD |

**Ejemplo:**
```
feat(auth): implementar refresh token con rotación automática

- [backend-architect] Endpoint POST /auth/refresh con idempotency key
- [qa-automation] Suite unitaria + integración con mock de Redis
```

---

## 10. Criterios de Calidad para Considerar una Tarea "Hecha"

Una tarea NO está hecha hasta que cumple todos estos criterios:

- [ ] El código funciona según los requerimientos definidos.
- [ ] No hay errores ni warnings en consola/logs.
- [ ] Se probó el happy-path y al menos un edge case.
- [ ] El código es legible sin documentación adicional.
- [ ] No quedan secrets, TODOs bloqueantes ni código muerto.
- [ ] `active_context.md` refleja el nuevo estado del proyecto.
- [ ] El commit semántico está hecho con el formato correcto.
- [ ] Si hay cambios de UI: se verificó en mobile y desktop.
- [ ] Si hay cambios de API: el contrato OpenAPI está actualizado.
- [ ] Si hay cambios de infraestructura: el rollback está documentado.
