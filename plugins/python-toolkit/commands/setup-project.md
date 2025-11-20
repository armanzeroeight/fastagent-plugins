---
description: Initialize a new Python project with proper structure, configuration, and tooling
allowed-tools: Write, Bash(mkdir:*), Bash(touch:*)
argument-hint: [project-name] [--type=app|library|cli]
---

# Setup Python Project

Initialize a new Python project with proper structure and configuration.

## Context

- Current directory: !`pwd`
- Existing files: !`ls -la`

## Your Task

Create a well-structured Python project with appropriate configuration based on the project type.

### Arguments

- `$1`: Project name (required) - will be used for directory and package names
- `$2`: Project type (optional) - `app`, `library`, or `cli` (defaults to `app`)

### Steps

1. **Determine project type from arguments or ask user:**
   - `app`: Application (web service, script, etc.)
   - `library`: Reusable library for distribution
   - `cli`: Command-line tool

2. **Create project structure based on type:**

   **For applications (app):**
   ```
   project-name/
   ├── src/
   │   └── project_name/
   │       ├── __init__.py
   │       ├── main.py
   │       └── config.py
   ├── tests/
   │   ├── __init__.py
   │   └── test_main.py
   ├── pyproject.toml
   ├── .gitignore
   └── README.md
   ```

   **For libraries (library):**
   ```
   project-name/
   ├── src/
   │   └── project_name/
   │       ├── __init__.py
   │       └── core.py
   ├── tests/
   │   ├── __init__.py
   │   └── test_core.py
   ├── docs/
   │   └── index.md
   ├── pyproject.toml
   ├── .gitignore
   └── README.md
   ```

   **For CLI tools (cli):**
   ```
   project-name/
   ├── src/
   │   └── project_name/
   │       ├── __init__.py
   │       ├── cli.py
   │       └── commands/
   │           └── __init__.py
   ├── tests/
   │   ├── __init__.py
   │   └── test_cli.py
   ├── pyproject.toml
   ├── .gitignore
   └── README.md
   ```

3. **Create pyproject.toml with UV/PEP 621 configuration:**

   ```toml
   [project]
   name = "project-name"
   version = "0.1.0"
   description = "Project description"
   readme = "README.md"
   requires-python = ">=3.9"
   authors = [
       {name = "Your Name", email = "you@example.com"}
   ]
   dependencies = []
   
   [project.optional-dependencies]
   dev = [
       "pytest>=7.0.0",
       "black>=23.0.0",
       "mypy>=1.0.0",
       "ruff>=0.1.0",
   ]
   
   [build-system]
   requires = ["hatchling"]
   build-backend = "hatchling.build"
   ```

   **For CLI projects, add:**
   ```toml
   [project.scripts]
   project-name = "project_name.cli:main"
   ```

   **For libraries with src layout, add:**
   ```toml
   [tool.hatch.build.targets.wheel]
   packages = ["src/project_name"]
   ```

4. **Create .gitignore:**

   ```
   # Python
   __pycache__/
   *.py[cod]
   *$py.class
   *.so
   .Python
   
   # Virtual environments
   .venv/
   venv/
   env/
   ENV/
   
   # UV
   uv.lock
   
   # IDEs
   .vscode/
   .idea/
   *.swp
   *.swo
   
   # Testing
   .pytest_cache/
   .coverage
   htmlcov/
   
   # Distribution
   dist/
   build/
   *.egg-info/
   ```

5. **Create README.md with basic structure:**

   ```markdown
   # Project Name
   
   Project description goes here.
   
   ## Installation
   
   ```bash
   uv sync
   ```
   
   ## Usage
   
   [Usage instructions]
   
   ## Development
   
   ```bash
   # Install dependencies
   uv sync
   
   # Run tests
   uv run pytest
   
   # Format code
   uv run black .
   
   # Type check
   uv run mypy src/
   ```
   
   ## License
   
   [License information]
   ```

6. **Create initial Python files:**

   **For app - src/project_name/main.py:**
   ```python
   """Main application module."""
   
   def main():
       """Main entry point."""
       print("Hello from project-name!")
   
   if __name__ == "__main__":
       main()
   ```

   **For library - src/project_name/core.py:**
   ```python
   """Core library functionality."""
   
   def hello(name: str) -> str:
       """Return a greeting.
       
       Args:
           name: Name to greet
           
       Returns:
           Greeting message
       """
       return f"Hello, {name}!"
   ```

   **For CLI - src/project_name/cli.py:**
   ```python
   """Command-line interface."""
   import argparse
   
   def main():
       """Main CLI entry point."""
       parser = argparse.ArgumentParser(description="Project description")
       parser.add_argument("--version", action="version", version="0.1.0")
       
       args = parser.parse_args()
       print("Hello from project-name CLI!")
   
   if __name__ == "__main__":
       main()
   ```

7. **Create src/project_name/__init__.py:**

   ```python
   """Project Name - Project description."""
   
   __version__ = "0.1.0"
   ```

8. **Create initial test file:**

   ```python
   """Tests for main module."""
   import pytest
   
   def test_example():
       """Example test."""
       assert True
   ```

9. **Initialize git repository:**
   ```bash
   cd project-name
   git init
   git add .
   git commit -m "Initial commit"
   ```

10. **Provide next steps to user:**
    - Navigate to project: `cd project-name`
    - Install dependencies: `uv sync`
    - Run tests: `uv run pytest`
    - Start development

### Examples

**Example 1: Create a web application**
```
/setup-project my-api app
```

Creates a Python application structure for a web API.

**Example 2: Create a library**
```
/setup-project my-utils library
```

Creates a library structure suitable for PyPI distribution.

**Example 3: Create a CLI tool**
```
/setup-project my-tool cli
```

Creates a CLI tool with entry point configuration.

**Example 4: Simple invocation**
```
/setup-project my-project
```

Creates an application (default type) named "my-project".

## Notes

- Use src layout for better import isolation
- Include UV for fast, modern dependency management
- Add common development tools (pytest, black, mypy, ruff)
- Create .gitignore to exclude common Python artifacts
- Initialize git repository for version control
- Convert project-name (with hyphens) to project_name (with underscores) for Python package names
- UV automatically creates and manages virtual environments in .venv/
