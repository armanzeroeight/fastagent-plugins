---
name: readme-generator
description: Creates comprehensive README files with templates for different project types. Use when creating README, project documentation, or getting started guides.
---

# README Generator

## Quick Start

Generate a README based on project type:

```bash
# Detect project type
ls package.json && echo "Node.js project" || \
ls setup.py pyproject.toml && echo "Python project" || \
ls go.mod && echo "Go project"
```

## Instructions

### Step 1: Analyze Project

Gather project information:
- Project name (from package.json, pyproject.toml, etc.)
- Description (from manifest or git)
- Main language and framework
- Key features (scan source files)
- Dependencies (from manifest files)

### Step 2: Select Template

Choose template based on project type:

| Type | Template | Key Sections |
|------|----------|--------------|
| Library | library | Installation, API, Examples |
| CLI Tool | cli | Installation, Commands, Options |
| Web App | webapp | Features, Setup, Deployment |
| API | api | Endpoints, Authentication, Examples |

### Step 3: Generate Core Sections

**Title and Description:**
```markdown
# Project Name

Brief one-line description of what the project does.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](package.json)
```

**Installation:**
```markdown
## Installation

\`\`\`bash
npm install project-name
# or
pip install project-name
\`\`\`
```

**Usage:**
```markdown
## Usage

\`\`\`javascript
const project = require('project-name');

// Basic example
project.doSomething();
\`\`\`
```

### Step 4: Add Project-Specific Content

Include relevant sections:
- **Features**: Bullet list of key capabilities
- **API Reference**: For libraries
- **Configuration**: For configurable tools
- **Examples**: Real-world use cases
- **Contributing**: How to contribute
- **License**: License information

### Step 5: Add Badges and Links

Common badges:
```markdown
![Build Status](https://github.com/user/repo/workflows/CI/badge.svg)
![Coverage](https://codecov.io/gh/user/repo/branch/main/graph/badge.svg)
![npm version](https://badge.fury.io/js/package-name.svg)
```

## README Structure

### Essential Sections (all projects)

1. **Title and Description**
2. **Installation**
3. **Quick Start / Usage**
4. **License**

### Recommended Sections

5. **Features** - What makes it useful
6. **Documentation** - Link to full docs
7. **Examples** - Common use cases
8. **Contributing** - How to help
9. **Support** - Where to get help

### Optional Sections

10. **Requirements** - System dependencies
11. **Configuration** - Setup options
12. **Troubleshooting** - Common issues
13. **Changelog** - Recent changes
14. **Acknowledgments** - Credits

## Advanced

For detailed information, see:
- [Templates](reference/templates.md) - README templates by project type
- [Best Practices](reference/best-practices.md) - Documentation standards and style guide
