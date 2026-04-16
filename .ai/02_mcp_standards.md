# 02 — Estándares de MCP (Model Context Protocol)

Directrices para diseñar, construir, registrar y mantener herramientas autónomas (MCPs) que extienden las capacidades de los agentes IA en este workspace. Un MCP mal diseñado es un vector de error o de seguridad; un MCP bien diseñado es una herramienta que el agente puede invocar con confianza ciega.

---

## 1. Estructura de Directorios (Convención Oficial)

Todos los MCPs del proyecto viven bajo una carpeta raíz llamada `mcp/` en la raíz del repositorio. Cada servidor MCP tiene su propia subcarpeta identificada por su nombre funcional en `kebab-case`:

```
[raíz del proyecto]/
└── mcp/
    ├── ssh-mcp/               ← MCP de operaciones SSH
    │   ├── index.js
    │   ├── package.json
    │   ├── .env               ← Secrets locales (never committed)
    │   └── .env.example       ← Plantilla pública de variables
    │
    ├── coolify-mcp/           ← MCP de gestión Coolify
    │   ├── index.js
    │   ├── package.json
    │   └── .env.example
    │
    └── [nuevo-mcp]/           ← Cada nuevo MCP sigue la misma estructura
        ├── index.js
        ├── package.json
        ├── README.md
        └── .env.example
```

> [!IMPORTANT]
> El nombre de la carpeta es el identificador canónico del MCP. Debe ser descriptivo del dominio que cubre (`stripe-mcp`, `github-mcp`, `cloudflare-mcp`), nunca genérico (`server1`, `tools`, `helper`).

---

## 2. Patrón Front-Matter (Determinismo IA en Herramientas)

Todo skill lógico, herramienta delegada a IA, o prompt de activación de MCP se define con un frontmatter YAML estricto. Sin una descripción precisa, el ruteo interno de los LLMs falla:

```yaml
---
name: nombre-del-tool
description: >
  Describe CUÁNDO activar esta herramienta (condición de activación),
  QUÉ hace exactamente y QUÉ NO hace (límites explícitos).
  Ejemplo: "Ejecuta comandos SSH en el servidor de producción definido en .env.
  Solo acepta comandos de la whitelist pre-aprobada. No ejecuta comandos destructivos."
---
```

**Reglas del `description`:**

- Redactado en presente indicativo.
- Incluye el dominio actuante (¿en qué servidor, servicio o sistema opera?).
- Menciona explícitamente las restricciones o límites del tool.
- Máximo 3 oraciones. Si necesita más, el tool hace demasiadas cosas.

---

## 3. Aislamiento Total por MCP (Zero Pollution Policy)

Cada MCP es un ecosistema completamente autónomo e independiente:

- **Dependencias propias**: Cada MCP tiene su propio `package.json`. Nunca comparten `node_modules` con otro MCP ni con el proyecto raíz.
- **Variables de entorno propias**: Cada MCP gestiona su propio `.env`. Las variables de un MCP no son visibles para otro.
- **Sin estado compartido**: Los MCPs no escriben en memoria o disco compartido entre sí. El estado se pasa por parámetros de invocación, no por variables globales.
- **Sin dependencias cruzadas**: Un MCP no importa módulos de otro MCP. Si necesitan lógica compartida, se extrae a una librería independiente publicada.

---

## 4. Seguridad de Acceso (Principio de Whitelist Estricta)

Un MCP con acceso a shell, APIs externas o filesystems es una superficie de ataque si no está correctamente limitado:

### 4.1 Comandos Permitidos (Whitelist Obligatoria)

Todo MCP que ejecuta comandos de sistema operativo define una whitelist explícita de comandos o patrones permitidos. Cualquier comando fuera de la lista es rechazado con error inmediato:

```javascript
const ALLOWED_COMMANDS = [
  "docker ps",
  "docker logs",
  "nginx -t",
  "systemctl status",
  // ...lista explícita y mínima
];

function validateCommand(cmd) {
  const isAllowed = ALLOWED_COMMANDS.some((allowed) => cmd.startsWith(allowed));
  if (!isAllowed)
    throw new Error(`FORBIDDEN: Command "${cmd}" not in whitelist.`);
}
```

### 4.2 Sanitización de Inputs

Todo parámetro recibido del agente IA se sanitiza antes de ser usado en comandos o queries:

- Prohibido concatenar inputs directamente en strings de shell (`exec('rm ' + userInput)`).
- Usar arrays de argumentos en lugar de strings interpolados (`execFile('docker', ['logs', containerId])`).
- Validar tipo, longitud y formato de cada parámetro con schema antes de ejecutar.

### 4.3 Secrets: Nunca en Argumentos del Proceso

Las credenciales se cargan desde `.env` via `dotenv`. Nunca se pasan como argumentos de CLI que aparecen en `ps aux` o en logs del sistema.

---

## 5. Estructura Obligatoria de un MCP (Checklist de Creación)

Al crear un nuevo MCP, estos archivos son obligatorios antes del primer uso:

| Archivo        | Propósito                                              | ¿Committed?                |
| -------------- | ------------------------------------------------------ | -------------------------- |
| `index.js`     | Punto de entrada del servidor MCP                      | ✅ Sí                      |
| `package.json` | Dependencias y scripts del MCP                         | ✅ Sí                      |
| `.env.example` | Plantilla de variables con descripción de cada una     | ✅ Sí                      |
| `README.md`    | Propósito, tools expuestos, cómo instalar y configurar | ✅ Sí                      |
| `.env`         | Variables reales con secrets                           | ❌ Nunca (en `.gitignore`) |

El `README.md` de cada MCP incluye como mínimo:

1. **¿Qué hace este MCP?** (1 párrafo)
2. **Tools expuestos**: nombre, descripción, parámetros inputs/outputs.
3. **Instalación**: `npm install` y configuración del `.env`.
4. **Registro en `mcp.json`**: El snippet exacto a añadir.
5. **Limitaciones conocidas**: Qué NO puede hacer.

---

## 6. Registro en el Archivo de Configuración (mcp.json)

El archivo `.ai/mcp.json` (o equivalente de configuración del agente) es el registro central de todos los MCPs activos. Cada entrada sigue este formato estandarizado:

```json
{
  "mcpServers": {
    "[nombre-identificador]": {
      "command": "node",
      "args": ["[ruta-absoluta-o-relativa]/mcp/[nombre-mcp]/index.js"],
      "env": {
        "DOTENV_CONFIG_PATH": "[ruta]/mcp/[nombre-mcp]/.env"
      }
    }
  }
}
```

**Reglas de registro:**

- El `nombre-identificador` es el mismo `name` del frontmatter del MCP.
- Las rutas en `args` son absolutas para evitar ambigüedad entre entornos.
- Si el MCP requiere variables de entorno, se especifica la ruta al `.env` explícitamente en el campo `env`.
- Un MCP desactivado temporalmente se comenta con `_disabled_` como prefijo del nombre, no se elimina la entrada.

---

## 7. Diseño de Tools dentro de un MCP (API Interna)

Cada tool expuesto por un MCP sigue estas convenciones:

### 7.1 Naming de Tools

- `snake_case` o `kebab-case` consistente con el ecosistema.
- Verbo + sustantivo que describe la acción: `execute_ssh_command`, `deploy_service`, `get_container_logs`.
- Nunca nombres genéricos: `run`, `do_thing`, `process`.

### 7.2 Schema de Inputs y Outputs

Todo tool define explícitamente su schema de entrada y salida con tipos estrictos:

```javascript
{
  name: "execute_ssh_command",
  description: "Ejecuta un comando pre-aprobado en el servidor SSH configurado. Solo acepta comandos de la whitelist definida.",
  inputSchema: {
    type: "object",
    properties: {
      command: {
        type: "string",
        description: "Comando a ejecutar. Debe estar en la whitelist de comandos permitidos."
      }
    },
    required: ["command"]
  }
}
```

### 7.3 Manejo de Errores Estructurado

Los errores del MCP son estructurados y descriptivos. El agente IA debe poder entender qué falló y por qué para decidir el siguiente paso:

```javascript
// ✅ Error estructurado y accionable
{
  success: false,
  error_code: "SSH_CONNECTION_TIMEOUT",
  message: "No se pudo conectar al servidor en 10s. Verifica que el host esté activo y que el puerto SSH sea accesible.",
  host: "server.ejemplo.com",
  port: 22
}

// ❌ Error inutilizable
{
  error: "Connection failed"
}
```

---

## 8. MCPs Disponibles en el Proyecto

Registro de los MCPs actualmente operativos. Vacío en el estado inicial — se añade una fila por cada MCP creado y registrado en `mcp.json`.

| Carpeta | Identificador | Dominio | Rol Autorizado |
|---|---|---|---|
| — | — | — | — |

> [!NOTE]
> Cuando se cree un nuevo MCP, añadir una fila a esta tabla con: carpeta `mcp/[nombre]/`, identificador en `mcp.json`, dominio funcional y el rol autorizado a invocarlo. Reflejar también en `active_context.md §Active MCPs`.

---

## 9. Ciclo de Vida de un MCP (Creación → Producción)

```
1. Necesidad identificada por el Orquestador
        ↓
2. ADR: ¿Vale la pena un MCP o basta con una llamada directa?
        ↓
3. Creación de carpeta mcp/[nombre-mcp]/ con estructura obligatoria
        ↓
4. Implementación con whitelist, sanitización y manejo de errores
        ↓
5. Prueba local con el agente en modo sandbox
        ↓
6. Registro en mcp.json + actualización de la tabla §8
        ↓
7. Commit: chore(mcp): add [nombre-mcp] with tools [lista]
        ↓
8. Actualizar active_context.md con el nuevo MCP disponible
```

---

## 10. Prohibiciones Absolutas

Estas reglas nunca se negocian, sin importar el contexto o la urgencia:

- 🚫 **Commitear `.env`** con secrets reales. El `.gitignore` del MCP debe incluir `.env` explícitamente.
- 🚫 **Comandos de shell dinámicos** construidos por concatenación de strings de inputs del usuario.
- 🚫 **MCPs sin whitelist** que acepten comandos arbitrarios del agente IA.
- 🚫 **Compartir `node_modules`** entre MCPs o con el proyecto raíz.
- 🚫 **MCPs con múltiples dominios**: Un MCP de SSH no gestiona también pagos. Un MCP por dominio, sin excepción.
- 🚫 **Secrets en logs**: Ningún logger del MCP imprime variables de entorno, tokens o contraseñas, ni en modo debug.
