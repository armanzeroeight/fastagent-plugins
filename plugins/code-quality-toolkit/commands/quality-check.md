---
description: Run code quality analysis including code smell detection, complexity analysis, and refactoring recommendations
allowed-tools: Bash(eslint:*), Bash(pylint:*), Bash(radon:*), Bash(grep:*), Read, Grep, Glob
argument-hint: [--type smells|complexity|all] [--threshold low|medium|high]
---

# Quality Check

## Context

- Project type: !`ls package.json 2>/dev/null && echo "node" || (ls setup.py pyproject.toml 2>/dev/null && echo "python" || echo "unknown")`
- File count: !`find src -type f \( -name "*.js" -o -name "*.ts" -o -name "*.py" \) 2>/dev/null | wc -l`

## Your Task

Analyze code quality and provide actionable recommendations for improvement.

### Arguments

- `$1`: Analysis type (optional, default: "all")
  - `smells` - Detect code smells only
  - `complexity` - Analyze complexity only
  - `all` - Run all analyses
- `$2`: Severity threshold (optional, default: "medium")
  - `low` - Report all issues
  - `medium` - Report medium and high severity
  - `high` - Report only high severity issues

### Steps

1. Detect project type and locate source files
2. Based on analysis type ($1):
   
   **For code smells:**
   - Scan for long methods (>50 lines)
   - Identify god classes (>20 methods)
   - Detect duplicated code
   - Find magic numbers
   - Check for deep nesting (>4 levels)
   
   **For complexity:**
   - Calculate cyclomatic complexity
   - Identify functions with complexity >10
   - Analyze cognitive complexity
   - Find deeply nested code
   
3. Categorize findings by severity
4. Filter by threshold ($2)
5. Provide refactoring recommendations

### Output Format

```markdown
# Code Quality Report

**Project:** [name]
**Files Analyzed:** [count]
**Issues Found:** [count]

## Summary

| Severity | Code Smells | Complexity | Total |
|----------|-------------|------------|-------|
| High     | X           | X          | X     |
| Medium   | X           | X          | X     |
| Low      | X           | X          | X     |

## High Priority Issues

### God Class: UserManager (src/services/UserManager.js)
- **Problem:** 523 lines, 18 methods, too many responsibilities
- **Impact:** Hard to maintain, test, and understand
- **Recommendation:** Split into UserService, AuthService, EmailService
- **Effort:** Medium (2-3 days)

### High Complexity: processOrder() (src/orders/processor.js:45)
- **Problem:** Cyclomatic complexity 28, cognitive complexity 35
- **Impact:** Error-prone, hard to test
- **Recommendation:** Extract validation, calculation, persistence
- **Effort:** Low (4-6 hours)

## Recommendations

1. **Immediate:** Refactor processOrder() - high complexity, high risk
2. **This Sprint:** Split UserManager god class
3. **Next Sprint:** Address duplicated code in payment processing
```

### Examples

**Example 1: Full analysis**
```
/quality-check
```
Runs all quality checks and reports all issues

**Example 2: Complexity only**
```
/quality-check complexity
```
Only analyzes code complexity

**Example 3: High severity only**
```
/quality-check all high
```
Runs all checks but only reports high severity issues
