---
title: "Secrets management"
type: document
tags: ["#convention", "#security", "#secrets"]
status: published
version: 1
---

# Secrets management

## Hard rules

1. **No secrets in code.** Ever. Not even in tests.
2. **No secrets in CI logs.** Mask outputs that might contain them.
3. **No secrets in error messages or stack traces.** Sanitize before logging or showing.
4. **No secrets in client-side bundles** (JS, mobile binaries). Anything shipped to user devices is public.
5. **Secrets have owners.** Every credential has a documented owner, rotation cadence, and revocation procedure.

## Where secrets live

| Environment | Storage |
|---|---|
| Local dev | `.env` (gitignored), or a secret manager via CLI (1Password, Doppler, AWS Secrets Manager) |
| CI | Native CI secrets (GitHub Actions Secrets, GitLab CI variables) |
| Staging / prod | Secret manager (AWS Secrets Manager / GCP Secret Manager / HashiCorp Vault) with IAM-controlled access |
| Containerized | Mounted at runtime via the secret manager, not baked into images |

## Detection

- **gitleaks** or **trufflehog** in CI scans every PR for accidental commits.
- Pre-commit hook (`.pre-commit-config.yaml`) catches them before push.
- If a secret lands in git history: assume compromised. Rotate, audit access, then rewrite history (or leave the history and rotate — the secret is already burned).

## Rotation

- **Long-lived API keys**: rotate every 90 days minimum.
- **Database passwords**: rotate on personnel change at minimum; quarterly otherwise.
- **OAuth client secrets**: rotate on suspicious activity or annually.
- **Signing keys** (JWT, code signing): documented rotation procedure with overlap period.

## Application code

```python
# Bad
API_KEY = "sk-abc123"

# Bad
API_KEY = os.environ["API_KEY"]  # crashes on missing; no validation

# Better
class Config(BaseSettings):
    api_key: SecretStr = Field(..., description="Vendor API key")

config = Config()
client = VendorClient(api_key=config.api_key.get_secret_value())
```

Use a typed config layer (`pydantic-settings`, `viper`, `figment`) so missing or malformed secrets fail at boot, not at first use.

## Anti-patterns

- Committing a `.env.sample` with real-looking placeholder values that get re-used as defaults.
- Logging the entire request body — secrets travel in headers and bodies.
- Sharing secrets via Slack DM. Use a vault or 1Password share link.
- Storing secrets in a "secrets" repo with broad access. Defeats the point.

## See also

[[threat-modeling]] · [[auth-patterns]] · [[pii-handling]]
