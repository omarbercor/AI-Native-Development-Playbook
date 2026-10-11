# 03 — Regla Invariante: Estándares de Código y Calidad de Entrega (10/10)

> [!NOTE]
> Estas directrices aplican de forma universal a todo código, archivo o script generado en el proyecto, garantizando un producto final impecable y profesional.

---

## 1. Principios de Ingeniería Universales

- **DRY (Don't Repeat Yourself)**: Si una lógica, componente visual, consulta o configuración aparece dos veces, se abstrae en una utilidad, hook o componente reutilizable.
- **KISS (Keep It Simple, Stupid)**: La solución más sencilla y legible que cumpla el requerimiento es siempre la preferida. La complejidad innecesaria es deuda técnica.
- **YAGNI (You Aren't Gonna Need It)**: Solo se implementa lo que el requerimiento actual exige. Cero código especulativo o arquitecturas sobredimensionadas para "el futuro".
- **Fail Fast & Loud**: Validar en el borde. Detectar y lanzar errores estructurados y descriptivos con contexto lo más temprano posible, sin fallos silenciosos.
- **Single Source of Truth**: Cada constante, token visual, variable de entorno o esquema vive en un único lugar canónico.

---

## 2. Calidad de Salida: Cero Placeholders

- **Prohibido el código incompleto**: Ningún bloque de código entregado contendrá `// TODO: implementar aquí`, `/* tu código aquí */` o funciones vacías sin implementar, a menos que el usuario lo haya pedido explícitamente como borrador conceptual.
- **Sin código muerto ni variables huérfanas**: Antes de dar por finalizada una edición, eliminar imports no utilizados, logs temporales de depuración y variables declaradas en desuso.
- **Retornos tempranos (Guard Clauses)**: Validar precondiciones y retornar de inmediato. Mantener el flujo principal (*happy path*) en el menor nivel de indentación posible.

---

## 3. Estándar de Commits (Conventional Commits)

Todo mensaje de commit debe seguir el estándar:

```
<tipo>(<alcance>): <descripción imperativa y clara en presente>

[cuerpo opcional explicando el porqué de la decisión]
```

**Tipos válidos:**
- `feat`: Nueva funcionalidad visible para el usuario.
- `fix`: Corrección de un defecto o bug.
- `refactor`: Mejora interna del código sin cambiar comportamiento externo.
- `perf`: Optimización de rendimiento o velocidad de carga.
- `docs`: Cambios exclusivos en documentación o guías.
- `chore`: Mantenimiento, dependencias o configuración de build.
- `test`: Creación o actualización de suites de pruebas.

---

## 4. Definition of Done (Criterios para considerar una tarea 10/10)

Una tarea solo se considera concluida cuando:
1. El código compila y ejecuta sin errores ni warnings en consola.
2. Los estilos y vistas son responsivos y visualmente pulidos (sin saltos de layout ni elementos cortados).
3. No existen secretos ni tokens hardcodeados.
4. Se han probado los casos de éxito y los principales casos límite (*edge cases*).
5. `active_context.md` ha sido actualizado con el nuevo estado del proyecto.
