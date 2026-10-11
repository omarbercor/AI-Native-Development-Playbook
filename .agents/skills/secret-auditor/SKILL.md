---
name: secret-auditor
description: >-
  Audits the repository and active code changes for leaked credentials, hardcoded API keys,
  exposed tokens, or uncommitted .env configurations. Use this skill before creating git commits,
  closing sprints, or when validating project security.
---

# Secret Auditor — Auditor de Fugas de Credenciales y Secretos

Esta habilidad permite ejecutar revisiones proactivas para garantizar que ningún secreto, token de IA, clave de API o credencial sensible quede expuesto en el código o en el historial de Git.

---

## 1. Procedimiento de Auditoría Pre-Commit

Antes de ejecutar `git add` y `git commit`, o antes de cerrar cualquier tarea, sigue estos pasos:

### Paso 1: Verificación de `.gitignore`
Asegúrate de que el archivo `.gitignore` incluya explícitamente:
```gitignore
.env
.env.*
!.env.example
*.pem
*.key
*.p12
*.pfx
```
Ejecuta `git status --ignored` para confirmar que los archivos `.env` locales estén siendo ignorados y no aparezcan en *Untracked files*.

### Paso 2: Escaneo de Expresiones Regulares en Cambios Activos
Revisa el `git diff` o los archivos recientemente modificados buscando coincidencias con los patrones de secretos definidos en [secret_patterns.md](./references/secret_patterns.md).

Búsquedas críticas comunes:
- Google AI / Maps: `AIza[0-9A-Za-z-_]{35}`
- OpenAI: `sk-[a-zA-Z0-9]{32,}` o `sk-proj-[a-zA-Z0-9_-]+`
- Anthropic: `sk-ant-[a-zA-Z0-9_-]+`
- GitHub Tokens: `ghp_[a-zA-Z0-9]{36}` o `github_pat_[a-zA-Z0-9_]+`
- AWS Keys: `AKIA[0-9A-Z]{16}`
- Stripe: `(sk|pk)_(test|live)_[0-9a-zA-Z]{24,}`
- Llaves privadas: `BEGIN (RSA|EC|OPENSSH|PRIVATE) KEY`

### Paso 3: Inspección de Variables de Entorno y `.env.example`
1. Comprueba que todo secreto utilizado en el código se invoque mediante `process.env` (o equivalente del lenguaje).
2. Comprueba que toda nueva variable de entorno requerida esté declarada en `.env.example` con valor en blanco y descripción de cómo obtenerla.

---

## 2. Protocolo de Acción si se Detecta un Secreto Hardcodeado

Si el auditor encuentra una clave expuesta en el código:

1. **Detener la operación**: No proceder con el commit ni marcar la tarea como lista.
2. **Extraer a `.env`**: Mover el secreto al archivo local `.env`.
3. **Reemplazar en código**: Sustituir el valor hardcodeado por la variable de entorno correspondiente.
4. **Actualizar `.env.example`**: Agregar la clave vacía en el archivo de ejemplo.
5. **Si el secreto ya fue commiteado en el pasado**:
   - Considerar la clave como **comprometida**.
   - Notificar al usuario inmediatamente para que revoque y genere una nueva clave en la plataforma del proveedor (el historial de Git puede quedar expuesto incluso si se borra el commit después).
