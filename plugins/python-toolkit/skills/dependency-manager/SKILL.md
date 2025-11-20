---
name: dependency-manager
description: Manage Python dependencies using UV, pip-tools, or requirements.txt. Use when setting up dependency management, resolving conflicts, or choosing between UV, pip-tools, and requirements.txt workflows.
---

# Dependency Manager

Manage Python project dependencies with UV, pip-tools, or requirements.txt.

## Quick Start

Choose UV for new projects (fast, modern), pip-tools for existing pip workflows, or requirements.txt for simple projects.

## Instructions

### Choosing a Tool

**UV** - Fast, modern Python package manager (recommended):
- Extremely fast dependency resolution (10-100x faster than pip)
- Automatic virtual environment management
- Drop-in replacement for pip and pip-tools
- Lock file support with uv.lock
- Best for: All projects, especially new ones

**pip-tools** - Lightweight, pip-compatible workflow:
- Generates lock files from requirements.in
- Works with existing pip workflows
- Minimal changes to project structure
- Best for: Existing projects, CI/CD with pip, gradual adoption

**requirements.txt** - Simple, universal:
- Direct pip install
- No additional tools
- No lock file (unless manually maintained)
- Best for: Simple scripts, minimal dependencies, quick prototypes

### UV Setup (Recommended)

**Install UV:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
# Or with pip
pip install uv
```

**Initialize new project:**
```bash
uv init my-project
cd my-project
```

**Add dependencies:**
```bash
# Runtime dependency
uv add requests

# Development dependency
uv add --dev pytest

# With version constraint
uv add "requests>=2.28.0,<3.0.0"
```

**Install dependencies:**
```bash
uv sync
```

**Update dependencies:**
```bash
# Update all
uv lock --upgrade

# Update specific package
uv lock --upgrade-package requests
```

**Remove dependencies:**
```bash
uv remove requests
```

**Show dependency tree:**
```bash
uv tree
```

**Export to requirements.txt:**
```bash
uv pip compile pyproject.toml -o requirements.txt
```

**Run commands in virtual environment:**
```bash
uv run python script.py
uv run pytest
```

### pip-tools Setup

**Install pip-tools:**
```bash
pip install pip-tools
```

**Create requirements.in:**
```
# requirements.in
requests>=2.28.0
flask>=2.0.0
```

**Create requirements-dev.in:**
```
# requirements-dev.in
-c requirements.txt  # Constrain to production versions
pytest>=7.0.0
black>=23.0.0
mypy>=1.0.0
```

**Compile lock files:**
```bash
# Compile production dependencies
pip-compile requirements.in

# Compile dev dependencies
pip-compile requirements-dev.in
```

This generates `requirements.txt` and `requirements-dev.txt` with pinned versions.

**Install dependencies:**
```bash
pip-sync requirements.txt requirements-dev.txt
```

**Update dependencies:**
```bash
# Update all
pip-compile --upgrade requirements.in

# Update specific package
pip-compile --upgrade-package requests requirements.in
```

**Add new dependency:**
1. Add to requirements.in
2. Run `pip-compile requirements.in`
3. Run `pip-sync requirements.txt`

### requirements.txt Workflow

**Create requirements.txt:**
```bash
pip freeze > requirements.txt
```

**Install dependencies:**
```bash
pip install -r requirements.txt
```

**Separate dev dependencies:**

Create `requirements-dev.txt`:
```
-r requirements.txt
pytest>=7.0.0
black>=23.0.0
```

Install with:
```bash
pip install -r requirements-dev.txt
```

### Version Constraints

**UV/PEP 621 syntax (pyproject.toml):**
```toml
[project]
dependencies = [
    "requests>=2.28.0,<3.0.0",  # Range
    "flask~=2.0.0",              # Compatible release
    "django>=3.2,<4.0",          # Range
    "numpy==1.24.0",             # Exact version
]
```

**pip syntax (requirements.in or requirements.txt):**
```
requests>=2.28.0,<3.0.0
flask~=2.0.0
django>=3.2,<4.0
numpy==1.24.0
pandas
```

**Constraint operators:**
- `==`: Exact version
- `>=`, `<=`: Minimum/maximum version
- `~=`: Compatible release (patch updates)
- `,`: Combine constraints (AND)

### Dependency Groups

**UV/PEP 621 groups:**
```toml
[project.optional-dependencies]
dev = ["pytest>=7.0.0", "black>=23.0.0"]
docs = ["sphinx>=5.0.0"]
test = ["coverage>=7.0.0"]
```

Install specific groups:
```bash
uv sync --extra dev
uv sync --extra docs
uv sync --all-extras
```

**pip-tools approach:**

Create separate `.in` files:
- `requirements.in` - Production
- `requirements-dev.in` - Development
- `requirements-test.in` - Testing
- `requirements-docs.in` - Documentation

### Resolving Conflicts

**With UV:**

1. **Check conflict:**
   ```bash
   uv add package-name
   # UV will show conflict if it exists
   ```

2. **Update constraints in pyproject.toml:**
   ```toml
   [project]
   dependencies = [
       "package-a>=2.0.0",  # Relax constraint
       "package-b>=3.0.0",
   ]
   ```

3. **Force resolution:**
   ```bash
   uv lock
   uv sync
   ```

**With pip-tools:**

1. **Check conflict:**
   ```bash
   pip-compile requirements.in
   # Will show resolution errors
   ```

2. **Adjust constraints in requirements.in:**
   ```
   package-a>=2.0.0  # Relax constraint
   package-b>=3.0.0
   ```

3. **Recompile:**
   ```bash
   pip-compile requirements.in
   ```

**Common conflict patterns:**

- **Transitive dependency conflict**: Two packages require incompatible versions of a third package
  - Solution: Update one or both packages, or relax constraints

- **Python version conflict**: Package requires newer Python than project supports
  - Solution: Upgrade Python version or find alternative package

- **Platform-specific conflict**: Package not available on current platform
  - Solution: Use platform markers or optional dependencies

### Platform-Specific Dependencies

**UV/PEP 621:**
```toml
[project]
dependencies = [
    "pywin32>=305; sys_platform == 'win32'",
    "python-daemon>=2.3; sys_platform == 'linux'",
]
```

**pip:**
```
pywin32>=305; sys_platform == 'win32'
python-daemon>=2.3; sys_platform == 'linux'
```

### Optional Dependencies

**UV/PEP 621:**
```toml
[project.optional-dependencies]
aws = ["boto3>=1.26.0"]
all = ["boto3>=1.26.0"]  # Include all optional deps
```

Install with: 
```bash
uv sync --extra aws
uv pip install package-name[aws]
```

### Virtual Environments

**UV (automatic):**
```bash
# UV creates and manages venv automatically
uv sync
uv run python script.py
source .venv/bin/activate  # Manual activation if needed
```

**pip-tools (manual):**
```bash
# Create venv
python -m venv venv

# Activate
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows

# Install
pip-sync requirements.txt
```

**requirements.txt (manual):**
```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### CI/CD Integration

**UV in CI (recommended):**
```yaml
# .github/workflows/test.yml
- name: Install UV
  run: curl -LsSf https://astral.sh/uv/install.sh | sh

- name: Install dependencies
  run: uv sync

- name: Run tests
  run: uv run pytest
```

**pip-tools in CI:**
```yaml
- name: Install dependencies
  run: |
    pip install pip-tools
    pip-sync requirements.txt requirements-dev.txt

- name: Run tests
  run: pytest
```

**requirements.txt in CI:**
```yaml
- name: Install dependencies
  run: pip install -r requirements.txt

- name: Run tests
  run: pytest
```

### Migration Strategies

**From requirements.txt to UV:**

1. **Initialize UV project:**
   ```bash
   uv init --no-readme
   ```

2. **Import dependencies:**
   ```bash
   # Add each dependency from requirements.txt
   cat requirements.txt | grep -v "^#" | xargs -I {} uv add {}
   ```

3. **Separate dev dependencies:**
   ```bash
   uv add --dev pytest black mypy
   ```

4. **Test:**
   ```bash
   uv sync
   uv run pytest
   ```

**From requirements.txt to pip-tools:**

1. **Rename requirements.txt:**
   ```bash
   mv requirements.txt requirements.in
   ```

2. **Compile lock file:**
   ```bash
   pip-compile requirements.in
   ```

3. **Create dev requirements:**
   ```bash
   echo "-c requirements.txt" > requirements-dev.in
   echo "pytest" >> requirements-dev.in
   pip-compile requirements-dev.in
   ```

4. **Test:**
   ```bash
   pip-sync requirements.txt requirements-dev.txt
   ```

**From pip-tools to UV:**

1. **Create pyproject.toml:**
   ```bash
   uv init --no-readme
   ```

2. **Import from requirements.in:**
   ```bash
   cat requirements.in | grep -v "^#" | grep -v "^-" | xargs -I {} uv add {}
   ```

3. **Test:**
   ```bash
   uv sync
   ```

## Common Patterns

### Monorepo with Shared Dependencies

**UV workspace:**
```toml
# Root pyproject.toml
[tool.uv.workspace]
members = ["packages/*"]

# packages/package1/pyproject.toml
[project]
name = "package1"
dependencies = ["requests>=2.28.0"]

# packages/package2/pyproject.toml
[project]
name = "package2"
dependencies = ["requests>=2.28.0"]
```

**pip-tools approach:**
```
# shared-requirements.in
requests>=2.28.0

# package1/requirements.in
-c ../shared-requirements.txt
flask>=2.0.0

# package2/requirements.in
-c ../shared-requirements.txt
django>=4.0.0
```

### Lock File Best Practices

**Commit lock files:**
- `uv.lock` - Always commit
- `requirements.txt` (from pip-compile) - Always commit
- `requirements.txt` (from pip freeze) - Consider committing

**Update regularly:**
```bash
# UV
uv lock --upgrade

# pip-tools
pip-compile --upgrade requirements.in
```

**Verify after updates:**
```bash
# Run tests
uv run pytest

# Check for security issues
pip-audit
```

## Troubleshooting

**UV lock takes too long:**
- UV is typically very fast; if slow, check for network issues
- Use `uv lock --offline` to use cached packages
- Check for circular dependencies

**pip-compile fails with conflict:**
- Relax version constraints in requirements.in
- Use `pip-compile --resolver=backtracking` for better resolution
- Check for incompatible transitive dependencies

**Dependencies not found:**
- Verify package name (check PyPI)
- Check Python version compatibility
- Ensure correct index URL (for private packages)
- With UV: `uv pip install --index-url https://custom-index.com package`

**Virtual environment issues:**
- Delete and recreate: `rm -rf .venv && uv sync`
- Check Python version matches project requirements
- UV automatically manages virtual environments

**Outdated dependencies:**
- Check with: `uv tree --outdated` or `pip list --outdated`
- Update carefully, test after each major update
- Use `uv lock --upgrade --dry-run` to preview changes
