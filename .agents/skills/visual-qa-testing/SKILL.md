---
name: visual-qa-testing
description: >-
  Quality assurance, automated testing, and visual regression specialist. Use this skill
  when writing unit tests, integration tests, E2E browser tests, testing responsive breakpoints,
  and validating edge cases.
---

# Visual QA & Testing — Control de Calidad y Pruebas

Esta habilidad define la estrategia y los estándares para asegurar que cada entrega esté blindada contra regresiones visuales y de lógica funcional.

---

## 1. La Pirámide de Pruebas

1. **Pruebas Unitarias**: Rápidas y masivas. Cubren funciones puras, utilidades, parsers y lógica de negocio aislada.
2. **Pruebas de Integración**: Prueban la interacción entre módulos (API con base de datos de prueba, servicios de caché, controladores).
3. **Pruebas E2E / Visuales**: Validan el flujo completo del usuario en el navegador (Happy Path y flujos críticos de compra o autenticación).

---

## 2. Checklist de Verificación Visual y Responsive

Antes de dar una interfaz web por completada:

1. **Breakpoints Críticos**:
   - Móvil pequeño: `375px` (iPhone SE)
   - Móvil estándar: `390px - 414px`
   - Tablet: `768px - 820px` (iPad)
   - Portátil / Desktop: `1280px` y `1920px`
2. **Desbordamiento Horizontal (*Horizontal Scrollbug*)**:
   - Verificar que no exista scroll horizontal no deseado en dispositivos móviles. Inspeccionar elementos con anchos fijos o márgenes negativos.
3. **Estados Extremos de UI**:
   - **Textos largos**: ¿Qué pasa si el nombre de un usuario tiene 80 caracteres? (Truncado con elipsis o salto limpio).
   - **Listas vacías**: ¿Existe un mensaje amigable o ilustración (*Empty State*)?
   - **Estados de Carga**: ¿Hay skeletons suaves o spinners no intrusivos?
   - **Estados de Error**: ¿Se muestra un mensaje claro con botón para reintentar?

---

## 3. Principios de Mocks y Aislamiento de Tests

- **Determinismo**: Un test debe pasar el 100% de las veces en cualquier entorno.
- **Sin llamadas a servicios externos en tests**: APIs como Stripe, OpenAI, o servidores SMTP deben estar siempre simuladas mediante *mocks* o *stubs*.
- **Limpieza de estado**: Cada suite de tests limpia su estado o base de datos temporal antes y después de ejecutarse.
