---
description: Run a comprehensive security scan including vulnerability detection, secrets scanning, and dependency audit
allowed-tools: Bash(npm:*), Bash(pip:*), Bash(grep:*), Bash(gitleaks:*), Bash(bandit:*), Bash(govulncheck:*), Read, Grep, Glob
argument-hint: [--type code|secrets|deps|all] [--severity critical|high|medium|all]
---

# Security Scan

## Context

- Project type: !`ls package.json 2>/dev/null && echo "node" || (ls requirements.txt pyproject.toml 2>/dev/null && echo "python" || (ls go.mod 2>/dev/null && echo "go" || echo "unknown"))`
- Git status: !`git status --short 2>/dev/null | head -5`

## Your Task

Run a comprehensive security scan on the codebase and report findings organized by severity.

### Arguments

- `$1`: Scan type (optional)
  - `code` - Scan for code vulnerabilities only
  - `secrets` - Scan for hardcoded secrets only
  - `deps` - Audit dependencies only
  - `all` - Run all scans (default)
- `$2`: Minimum severity to report (optional)
  - `critical` - Only critical issues
  - `high` - High and critical
  - `medium` - Medium and above
  - `all` - All severities (default)

### Steps

1. Detect project type from manifest files
2. Based on scan type ($1 or "all"):
   - **Code scan**: Run appropriate static analysis (bandit for Python, eslint-security for JS)
   - **Secrets scan**: Run gitleaks or grep for common patterns
   - **Dependency audit**: Run npm audit, pip-audit, or govulncheck
3. Collect and categorize findings by severity
4. Filter by minimum severity ($2 or "all")
5. Generate consolidated report

### Output Format

```markdown
# Security Scan Report

**Scanned:** [timestamp]
**Project:** [project name]
**Scan Types:** [code, secrets, deps]

## Summary

| Severity | Count |
|----------|-------|
| Critical | X     |
| High     | X     |
| Medium   | X     |
| Low      | X     |

## Critical Issues

[List critical findings with file, line, description, and fix]

## High Issues

[List high findings]

## Recommendations

1. [Priority action items]
```

### Examples

**Example 1: Full scan**
```
/security-scan
```
Runs all security checks and reports all findings

**Example 2: Secrets only**
```
/security-scan secrets
```
Only scans for hardcoded secrets

**Example 3: Critical issues only**
```
/security-scan all critical
```
Runs all scans but only reports critical severity issues
