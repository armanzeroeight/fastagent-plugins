---
name: dependency-audit
description: Analyzes project dependencies for known security vulnerabilities using npm audit, pip-audit, or similar tools. Use when auditing packages, checking for CVEs, or updating vulnerable dependencies.
---

# Dependency Audit

## Quick Start

Audit dependencies based on project type:

```bash
# Node.js
npm audit

# Python
pip-audit

# Go
govulncheck ./...
```

## Instructions

### Step 1: Identify Package Manager

Check for manifest files:
- `package.json` / `package-lock.json` → npm/yarn
- `requirements.txt` / `pyproject.toml` → pip
- `go.mod` → Go modules
- `Cargo.toml` → Cargo (Rust)
- `Gemfile` → Bundler (Ruby)

### Step 2: Run Audit

**Node.js (npm):**
```bash
npm audit
npm audit --json  # Machine-readable output
```

**Node.js (yarn):**
```bash
yarn audit
yarn audit --json
```

**Python:**
```bash
pip install pip-audit
pip-audit
pip-audit -r requirements.txt
```

**Go:**
```bash
govulncheck ./...
```

**Ruby:**
```bash
bundle audit check --update
```

### Step 3: Analyze Results

Categorize by severity:

| Severity | CVSS | Action |
|----------|------|--------|
| Critical | 9.0+ | Update immediately |
| High | 7.0-8.9 | Update within 24h |
| Moderate | 4.0-6.9 | Update this sprint |
| Low | < 4.0 | Update when convenient |

### Step 4: Fix Vulnerabilities

**npm - Auto-fix:**
```bash
npm audit fix
npm audit fix --force  # Breaking changes allowed
```

**npm - Manual update:**
```bash
npm update vulnerable-package
# or specific version
npm install vulnerable-package@2.0.0
```

**Python - Update package:**
```bash
pip install --upgrade vulnerable-package
# or pin safe version in requirements.txt
vulnerable-package>=2.0.0
```

### Step 5: Verify Fixes

Re-run audit to confirm:
```bash
npm audit  # Should show 0 vulnerabilities
pip-audit  # Should show no issues
```

## Common Scenarios

### Transitive Dependencies

When vulnerability is in a sub-dependency:

```bash
# Check dependency tree
npm ls vulnerable-package

# Force resolution (npm)
# Add to package.json:
{
  "overrides": {
    "vulnerable-package": "2.0.0"
  }
}
```

### No Fix Available

When no patched version exists:
1. Check if vulnerability affects your usage
2. Consider alternative packages
3. Implement workarounds if possible
4. Monitor for updates

### Breaking Changes

When fix requires major version bump:
1. Review changelog for breaking changes
2. Update code to accommodate changes
3. Run tests thoroughly
4. Consider gradual rollout

## Report Format

```markdown
## Dependency Audit Report

**Project:** my-app
**Date:** 2024-01-15
**Total Dependencies:** 245
**Vulnerabilities Found:** 3

### Critical (1)

**lodash** - Prototype Pollution
- Installed: 4.17.15
- Fixed in: 4.17.21
- CVE: CVE-2021-23337
- Fix: `npm install lodash@4.17.21`

### High (1)

**axios** - SSRF Vulnerability
- Installed: 0.21.0
- Fixed in: 0.21.2
- CVE: CVE-2021-3749
- Fix: `npm install axios@0.21.2`

### Moderate (1)

**minimist** - Prototype Pollution
- Installed: 1.2.5
- Fixed in: 1.2.6
- CVE: CVE-2021-44906
- Fix: `npm audit fix`
```

## CI/CD Integration

### GitHub Actions

```yaml
- name: Audit dependencies
  run: |
    npm audit --audit-level=high
    # Fails if high or critical vulnerabilities found
```

### Pre-commit

```bash
# package.json scripts
{
  "scripts": {
    "precommit": "npm audit --audit-level=moderate"
  }
}
```
