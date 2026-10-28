---
trigger: always_on
description: Política invariante Zero-Trust de protección absoluta de secretos, variables .env y validación en el borde.
---

# 01 — Regla Invariante: Seguridad Zero-Trust y Protección Absoluta de Secretos

> [!CAUTION]
> **POLÍTICA DE TOLERANCIA CERO**: Ningún agente de IA, bajo ninguna circunstancia, escribirá credenciales reales, tokens, contraseñas ni claves API en el código fuente ni en archivos sujetos a control de versiones (Git).

---

## 1. Prohibición Categórica de Hardcoding de Credenciales

1. **Nunca escribir valores reales de secretos en código**:
   - Claves de OpenAI, Anthropic, Google Gemini, AWS, Stripe, GitHub, base de datos, JWT secrets, o tokens de terceros.
   - Si se necesita un valor de demostración, usar explícitamente cadenas ficticias como: `"your_api_key_here"`, `"demo_token_placeholder"`, o referenciar la variable de entorno.

2. **Detección Activa de Patrones de Claves**:
   - El agente debe auto-revisar cualquier bloque de código antes de guardarlo. Si detecta cadenas que coinciden con formatos de credenciales (ej. `AIza[0-9A-Za-z-_]{35}`, `sk-[a-zA-Z0-9]{32,}`, `ghp_[a-zA-Z0-9]{36}`, `AKIA[0-9A-Z]{16}`), debe detenerse inmediatamente y extraerlas a variables de entorno.

3. **Consumo Exclusivo mediante Variables de Entorno**:
   - Todo secreto o configuración sensible debe consumirse a través de variables de entorno:
     - Node.js / TypeScript: `process.env.VARIABLE_NAME`
     - Python: `os.environ.get("VARIABLE_NAME")`
     - PHP: `$_ENV['VARIABLE_NAME']` o `env('VARIABLE_NAME')`
     - Frontends (Vite/Next): `import.meta.env.VITE_PUBLIC_*` o `process.env.NEXT_PUBLIC_*` (solo para datos públicos que no sean secretos).

---

## 2. Gestión de Archivos `.env`

1. **Archivos Prohibidos en Git**:
   - `.env`, `.env.local`, `.env.staging`, `.env.production`, `.env.*.local` NUNCA deben añadirse al control de versiones.
   - Antes de iniciar cualquier proyecto o commit, verificar que `.gitignore` contenga la regla:
     ```gitignore
     .env
     .env.*
     !.env.example
     ```

2. **El Único Archivo Permitido: `.env.example`**:
   - Todo proyecto DEBE tener un archivo `.env.example` con la lista de todas las variables requeridas, pero con valores vacíos o explicativos:
     ```bash
     # API Keys (Obtener en dashboard correspondiente)
     OPENAI_API_KEY=
     DATABASE_URL=postgresql://user:password@localhost:5432/dbname
     JWT_SECRET=minimo_32_caracteres_aleatorios
     ```

---

## 3. Sanitización y Validación en el Borde (Input/Output)

1. **Todo input externo es potencialmente hostil**:
   - Validar esquemas y tipos estrictos antes de procesar cualquier entrada de usuario, query param o webhook (usando bibliotecas como Zod, Pydantic, o validadores nativos).
2. **Logs Limpios**:
   - NUNCA imprimir en logs (`console.log`, `print`, loggers) objetos de petición completos que puedan contener `Authorization`, tokens, contraseñas o tarjetas de crédito.
3. **Comandos de Sistema Sanitizados**:
   - Prohibida la interpolación directa de cadenas en comandos de shell (`exec("cmd " + input)`). Usar listas de argumentos estructuradas (`execFile("cmd", [input])`).
