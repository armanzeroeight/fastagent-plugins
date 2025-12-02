---
name: secrets-detector
description: Detects hardcoded secrets, API keys, passwords, and credentials in source code. Use when checking for leaked secrets, credential exposure, or before committing code.
---

# Secrets Detector

## Quick Start

Scan for secrets using gitleaks:

```bash
# Install
brew install gitleaks  # macOS
# or
pip install detect-secrets

# Scan current directory
gitleaks detect --source .
```

## Instructions

### Step 1: Choose Detection Tool

**Gitleaks** (recommended):
```bash
gitleaks detect --source . --verbose
```

**detect-secrets**:
```bash
detect-secrets scan . --all-files
```

**Manual grep patterns**:
```bash
grep -rn "AKIA[0-9A-Z]{16}" .  # AWS Access Key
grep -rn "ghp_[a-zA-Z0-9]{36}" .  # GitHub Token
```

### Step 2: Scan for Common Patterns

| Secret Type | Pattern | Example |
|-------------|---------|---------|
| AWS Access Key | `AKIA[0-9A-Z]{16}` | AKIAIOSFODNN7EXAMPLE |
| AWS Secret Key | `[A-Za-z0-9/+=]{40}` | wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY |
| GitHub Token | `ghp_[a-zA-Z0-9]{36}` | ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx |
| GitHub OAuth | `gho_[a-zA-Z0-9]{36}` | gho_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx |
| Slack Token | `xox[baprs]-[0-9a-zA-Z-]+` | xoxb-123456789-abcdefghij |
| Private Key | `-----BEGIN.*PRIVATE KEY-----` | RSA/EC private keys |
| Generic API Key | `api[_-]?key.*=.*['\"][a-zA-Z0-9]{20,}` | api_key = "abc123..." |
| Generic Password | `password.*=.*['\"][^'\"]+['\"]` | password = "secret123" |

### Step 3: Check Git History

Secrets may exist in git history even if removed:

```bash
# Scan entire git history
gitleaks detect --source . --log-opts="--all"

# Check specific commits
git log -p --all -S 'password' --source
```

### Step 4: Categorize Findings

**Critical** - Immediate rotation required:
- Cloud provider credentials (AWS, GCP, Azure)
- Database connection strings
- Private keys

**High** - Rotate soon:
- API keys for external services
- OAuth tokens
- Webhook secrets

**Medium** - Review and rotate:
- Internal service tokens
- Test credentials that might be reused

### Step 5: Report Findings

```markdown
## Secrets Detection Report

### Critical (1)
1. **AWS Secret Key** - config/aws.js:12
   - Type: AWS credentials
   - Action: Rotate immediately in AWS console

### High (2)
1. **GitHub Token** - scripts/deploy.sh:45
   - Type: Personal access token
   - Action: Revoke and regenerate

2. **Slack Webhook** - src/notifications.js:23
   - Type: Incoming webhook URL
   - Action: Regenerate webhook
```

## Prevention

### Pre-commit Hook

```bash
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.18.0
    hooks:
      - id: gitleaks
```

### .gitignore Patterns

```gitignore
# Environment files
.env
.env.local
.env.*.local

# Key files
*.pem
*.key
*_rsa
*_ecdsa
*_ed25519

# Config with secrets
config/secrets.yml
credentials.json
```

### Environment Variables

Move secrets to environment variables:

```javascript
// BAD
const apiKey = "sk-abc123...";

// GOOD
const apiKey = process.env.API_KEY;
```

## Common False Positives

- Example/placeholder values in documentation
- Test fixtures with fake credentials
- Base64-encoded non-secret data
- Hash values (SHA, MD5)

Review each finding to confirm it's a real secret before taking action.
