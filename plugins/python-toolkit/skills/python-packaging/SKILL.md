---
name: python-packaging
description: Configure Python package metadata, setup.py, and pyproject.toml for distribution using UV or setuptools. Use when setting up Python packages, configuring build systems, or preparing projects for PyPI publication.
---

# Python Packaging

Configure Python package metadata and build configuration for distribution.

## Quick Start

Create pyproject.toml with UV or setuptools configuration for package distribution.

## Instructions

### Choosing Build System

**Use UV with pyproject.toml (recommended) when:**
- Starting new projects
- Want fast, modern tooling
- Need dependency management + packaging
- Building libraries for PyPI
- Want PEP 621 compliance

**Use setuptools (pyproject.toml + setup.py) when:**
- Maintaining existing projects
- Need compatibility with older tools
- Require custom build steps
- Complex C extensions

**Use setuptools (pyproject.toml only) when:**
- Modern setuptools-only approach
- No custom build logic
- Want declarative configuration
- PEP 621 compliance

### UV Configuration (Recommended)

Create `pyproject.toml` with UV (PEP 621 standard):

```toml
[project]
name = "my-package"
version = "0.1.0"
description = "Package description"
readme = "README.md"
requires-python = ">=3.9"
license = {text = "MIT"}
authors = [
    {name = "Your Name", email = "you@example.com"}
]
keywords = ["keyword1", "keyword2"]
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
]
dependencies = [
    "requests>=2.28.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "black>=23.0.0",
    "mypy>=1.0.0",
]

[project.scripts]
my-cli = "my_package.cli:main"

[project.urls]
Homepage = "https://github.com/username/my-package"
Repository = "https://github.com/username/my-package"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

**Key fields:**
- `name`: Package name (use hyphens, will be converted to underscores for import)
- `version`: Semantic versioning (MAJOR.MINOR.PATCH)
- `requires-python`: Python version constraint
- `dependencies`: Runtime dependencies with version constraints
- `optional-dependencies`: Development and optional dependencies
- `scripts`: Console script entry points

**Version constraints:**
- `>=2.28.0,<3.0.0`: Compatible versions
- `~=2.28.0`: >=2.28.0, <2.29.0 (patch updates)
- `>=2.28.0`: Minimum version
- `==2.28.0`: Exact version

### Setuptools Configuration (Modern)

Create `pyproject.toml` with setuptools (PEP 621):

```toml
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "my-package"
version = "0.1.0"
description = "Package description"
readme = "README.md"
requires-python = ">=3.9"
license = {text = "MIT"}
authors = [
    {name = "Your Name", email = "you@example.com"}
]
keywords = ["keyword1", "keyword2"]
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
]
dependencies = [
    "requests>=2.28.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "black>=23.0.0",
    "mypy>=1.0.0",
]

[project.scripts]
my-cli = "my_package.cli:main"

[project.urls]
Homepage = "https://github.com/username/my-package"
Repository = "https://github.com/username/my-package"
```

### Setuptools Configuration (Legacy)

Create `setup.py` for projects requiring custom build logic:

```python
from setuptools import setup, find_packages

setup(
    name="my-package",
    version="0.1.0",
    description="Package description",
    long_description=open("README.md").read(),
    long_description_content_type="text/markdown",
    author="Your Name",
    author_email="you@example.com",
    url="https://github.com/username/my-package",
    packages=find_packages(where="src"),
    package_dir={"": "src"},
    python_requires=">=3.9",
    install_requires=[
        "requests>=2.28.0",
    ],
    extras_require={
        "dev": [
            "pytest>=7.0.0",
            "black>=23.0.0",
            "mypy>=1.0.0",
        ],
    },
    entry_points={
        "console_scripts": [
            "my-cli=my_package.cli:main",
        ],
    },
    classifiers=[
        "Development Status :: 3 - Alpha",
        "Intended Audience :: Developers",
        "License :: OSI Approved :: MIT License",
        "Programming Language :: Python :: 3",
    ],
)
```

**When to use setup.py:**
- Custom build steps (compiling C extensions)
- Dynamic version from git tags
- Complex package discovery
- Conditional dependencies

### Package Structure

**Flat layout:**
```
my-package/
├── my_package/
│   ├── __init__.py
│   └── module.py
├── tests/
├── pyproject.toml
└── README.md
```

**Src layout (recommended for libraries):**
```
my-package/
├── src/
│   └── my_package/
│       ├── __init__.py
│       └── module.py
├── tests/
├── pyproject.toml
└── README.md
```

For src layout, configure package discovery:

**UV/Hatchling (pyproject.toml):**
```toml
[tool.hatch.build.targets.wheel]
packages = ["src/my_package"]
```

**Setuptools (pyproject.toml):**
```toml
[tool.setuptools.packages.find]
where = ["src"]
```

**Setuptools (setup.py):**
```python
packages=find_packages(where="src"),
package_dir={"": "src"},
```

### Version Management

**Static version in pyproject.toml:**
```toml
[project]
version = "0.1.0"
```

**Dynamic version from file:**
```toml
[project]
dynamic = ["version"]

[tool.setuptools.dynamic]
version = {attr = "my_package.__version__"}
```

Then in `src/my_package/__init__.py`:
```python
__version__ = "0.1.0"
```

**Dynamic version from git tags (requires setup.py):**
```python
from setuptools import setup
from setuptools_scm import get_version

setup(
    use_scm_version=True,
    setup_requires=["setuptools_scm"],
)
```

### Entry Points and Scripts

**Console scripts** create command-line tools:

```toml
[project.scripts]
my-cli = "my_package.cli:main"
another-tool = "my_package.tools:run"
```

This creates executables that call the specified functions.

**Entry point function:**
```python
# my_package/cli.py
def main():
    print("Hello from my-cli!")
    
if __name__ == "__main__":
    main()
```

### Including Data Files

**Include package data:**

```toml
[tool.setuptools.package-data]
my_package = ["data/*.json", "templates/*.html"]
```

**Include non-package files:**

Create `MANIFEST.in`:
```
include README.md
include LICENSE
recursive-include src/my_package/data *
```

### Classifiers

Use PyPI classifiers to categorize your package:

```toml
classifiers = [
    # Development status
    "Development Status :: 3 - Alpha",
    "Development Status :: 4 - Beta",
    "Development Status :: 5 - Production/Stable",
    
    # Audience
    "Intended Audience :: Developers",
    "Intended Audience :: Science/Research",
    
    # License
    "License :: OSI Approved :: MIT License",
    
    # Python versions
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
    
    # Topics
    "Topic :: Software Development :: Libraries",
    "Topic :: Internet :: WWW/HTTP",
]
```

Full list: https://pypi.org/classifiers/

### Building and Publishing

**With UV (recommended):**
```bash
# Build distribution
uv build

# Publish to PyPI
uv publish

# Or use twine
uv build
twine upload dist/*
```

**With setuptools:**
```bash
# Install build tools
pip install build twine

# Build distribution
python -m build

# Publish to PyPI
twine upload dist/*
```

### Testing Installation

Test your package locally before publishing:

```bash
# Install in editable mode with UV
uv pip install -e .

# Or with pip
pip install -e .

# Test the package
python -c "import my_package; print(my_package.__version__)"

# Test console scripts
my-cli --help
```

## Common Patterns

### Multi-Package Project

For projects with multiple packages:

```toml
[tool.hatch.build.targets.wheel]
packages = ["src/package1", "src/package2"]
```

### Optional Dependencies

Group optional features:

```toml
[project.optional-dependencies]
dev = ["pytest", "black", "mypy"]
docs = ["sphinx", "sphinx-rtd-theme"]
aws = ["boto3"]
all = ["pytest", "black", "mypy", "sphinx", "boto3"]
```

Install with: `pip install my-package[dev]` or `pip install my-package[all]`

### Platform-Specific Dependencies

```toml
[project]
dependencies = [
    "requests",
    "pywin32; platform_system=='Windows'",
    "python-daemon; platform_system=='Linux'",
]
```

## Validation

Before publishing, verify:

1. **Package builds successfully:**
   ```bash
   uv build  # or python -m build
   ```

2. **Metadata is correct:**
   ```bash
   twine check dist/*
   ```

3. **Installation works:**
   ```bash
   uv pip install dist/*.whl
   ```

4. **Entry points work:**
   ```bash
   my-cli --version
   ```

5. **Imports work:**
   ```python
   import my_package
   print(my_package.__version__)
   ```

## Troubleshooting

**Package not found after installation:**
- Check package name vs. import name (hyphens vs. underscores)
- Verify package discovery configuration
- Ensure `__init__.py` exists in package directory

**Entry points not working:**
- Verify function signature (should take no arguments or use argparse)
- Check entry point syntax: `"script-name = "package.module:function"`
- Reinstall package after changes

**Data files not included:**
- Add to `package-data` in pyproject.toml
- Or create MANIFEST.in
- Verify with: `python -m tarfile -l dist/*.tar.gz`

**Version conflicts:**
- Use compatible version constraints (`>=,<` syntax)
- Test with different dependency versions
- UV automatically creates lock files for reproducibility
