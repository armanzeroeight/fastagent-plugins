---
description: Generate or update project documentation including README, API docs, and changelog
allowed-tools: Bash(git:*), Bash(npm:*), Bash(cat:*), Bash(ls:*), Read, Write, Edit, Grep, Glob
argument-hint: [readme|api|changelog|all]
---

# Generate Documentation

## Context

- Project type: !`ls package.json 2>/dev/null && echo "node" || (ls setup.py pyproject.toml 2>/dev/null && echo "python" || (ls go.mod 2>/dev/null && echo "go" || echo "unknown"))`
- Existing docs: !`ls README.md CHANGELOG.md docs/ 2>/dev/null`
- Git status: !`git log --oneline -5 2>/dev/null`

## Your Task

Generate or update project documentation based on the specified type.

### Arguments

- `$1`: Documentation type (optional, default: "all")
  - `readme` - Generate/update README.md
  - `api` - Generate API documentation
  - `changelog` - Update CHANGELOG.md
  - `all` - Generate all documentation

### Steps

1. Detect project type and structure
2. Based on documentation type ($1):
   
   **For README:**
   - Analyze project structure and dependencies
   - Identify key features from source code
   - Select appropriate template (library, CLI, webapp, API)
   - Generate comprehensive README with:
     - Project description
     - Installation instructions
     - Usage examples
     - API reference (if library)
     - Contributing guidelines
   
   **For API docs:**
   - Scan for API routes/endpoints
   - Extract JSDoc comments or docstrings
   - Generate OpenAPI spec (for REST APIs)
   - Create interactive documentation
   
   **For Changelog:**
   - Review git commits since last release
   - Categorize changes (Added, Changed, Fixed, etc.)
   - Format as Keep a Changelog entry
   - Update CHANGELOG.md with new version

3. Validate generated documentation
4. Report what was created/updated

### Output Format

```markdown
# Documentation Generated

## README.md
✓ Created/Updated
- Project description added
- Installation instructions included
- Usage examples provided
- 5 sections total

## API Documentation
✓ Generated
- 12 endpoints documented
- OpenAPI spec created at docs/openapi.json
- Interactive docs available

## CHANGELOG.md
✓ Updated
- Version 2.1.0 entry added
- 3 new features
- 2 bug fixes
- 1 breaking change
```

### Examples

**Example 1: Generate README**
```
/generate-docs readme
```
Creates a comprehensive README.md based on project type

**Example 2: Update changelog**
```
/generate-docs changelog
```
Adds new changelog entry from recent commits

**Example 3: Generate all docs**
```
/generate-docs
```
Generates README, API docs, and updates changelog
