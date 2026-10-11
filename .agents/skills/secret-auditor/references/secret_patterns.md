# Patrones y Firmas de Secretos (Secret Audit Reference)

Lista de firmas y expresiones regulares para auditoría de código antes de realizar commits.

---

## 1. Patrones de Expresiones Regulares Comunes

| Proveedor / Servicio | Patrón Regex | Ejemplo de Formato |
| :--- | :--- | :--- |
| **Google Cloud / Gemini** | `AIza[0-9A-Za-z-_]{35}` | `AIzaSyD...` |
| **OpenAI API Key** | `sk-(?:proj-)?[a-zA-Z0-9_-]{20,}` | `sk-proj-abc123...` |
| **Anthropic Claude** | `sk-ant-[a-zA-Z0-9_-]{30,}` | `sk-ant-api03-...` |
| **GitHub Personal Token**| `ghp_[a-zA-Z0-9]{36}` | `ghp_123456...` |
| **GitHub Fine-grained** | `github_pat_[a-zA-Z0-9_]{82}` | `github_pat_11...` |
| **AWS Access Key ID** | `(?:A3T[A-Z0-9]|AKIA|AGPA|AIDA|AROA|AIPA|ANPA|ANVA|ASIA)[A-Z0-9]{16}` | `AKIAIOSFODNN7EXAMPLE` |
| **AWS Secret Access Key** | `(?i)aws_secret_access_key\s*=\s*['"][A-Za-z0-9/+=]{40}['"]` | `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY` |
| **Stripe Secret Key** | `sk_(?:test|live)_[0-9a-zA-Z]{24,}` | `sk_test_51...` |
| **Stripe Publishable** | `pk_(?:test|live)_[0-9a-zA-Z]{24,}` | `pk_test_51...` |
| **JWT Tokens** | `eyJ[A-Za-z0-9_-]{10,}\.eyJ[A-Za-z0-9_-]{10,}\.[A-Za-z0-9_-]{10,}` | `eyJhbGciOi...` |
| **Slack Webhooks** | `https:\/\/hooks\.slack\.com\/services\/T[a-zA-Z0-9_]{8}\/B[a-zA-Z0-9_]{8,12}\/[a-zA-Z0-9_]{24}` | URL de Webhook |
| **Private Keys (SSH/SSL)**| `-----BEGIN (?:RSA|DSA|EC|OPENSSH|PRIVATE) KEY-----` | Bloque PEM |

---

## 2. Comandos de Verificación Rápida en Terminal

### Buscar secretos sospechosos en archivos modificados:
```powershell
# En PowerShell (Windows):
git diff --name-only | ForEach-Object { Get-Content $_ | Select-String -Pattern "sk-proj-", "AIza", "AKIA", "ghp_" }
```

### Verificar que .env esté siendo ignorado por git:
```bash
git check-ignore -v .env
# Si retorna la línea de .gitignore, está correctamente protegido.
```
