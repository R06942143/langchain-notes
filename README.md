# langchain-notes
## Setup

### Download and Install Anaconda

1. Visit the [Anaconda download page](https://www.anaconda.com/download)

2. Download the appropriate version for your operating system:
   - Windows: Click the Windows installer
   - macOS: Click the macOS installer 
   - Linux: Click the Linux installer

3. Run the installer:
   - Windows: Double-click the downloaded .exe file
   - macOS: Double-click the downloaded .pkg file
   - Linux: Open terminal and run:
     ```bash
     bash ~/Downloads/Anaconda3-xxxx.xx-Linux-x86_64.sh
     ```
     (Replace xxxx.xx with the version number you downloaded)

4. Follow the installation prompts:
   - Accept the license agreement
   - Choose installation location (recommended: use default)
   - Choose whether to initialize Anaconda (recommended: yes)

5. Verify the installation:
   ```bash
   conda --version
   ```

You should now have Anaconda installed and ready to use!

### Create a new environment

```bash
conda create -n langchain-notes python=3.12
```

### Activate the environment

```bash
conda activate langchain-notes
```

### Install Poetry

1. Install Poetry using pip:
   ```bash
   pip install poetry
   ```

2. Verify the installation:
   ```bash
   poetry --version
   ```

3. Initialize a new Poetry project:
   ```bash
   poetry init
   ```
   Follow the interactive prompts to create your pyproject.toml file.

### Using Poetry and Conda Together

- Use Conda for packages with complex dependencies or pre-compiled binaries
- Use Poetry for pure Python packages and managing project dependencies

To add dependencies:
- With Poetry: `poetry add package-name`
- With Conda: `conda install package-name`

Poetry will manage your project's dependencies in `pyproject.toml` and `poetry.lock` files, while Conda manages the environment-level packages.


### Configure MyPy for Static Type Checking

1. First, install MyPy using Poetry:
   ```bash
   poetry add mypy --dev
   ```

2. Create a `mypy.ini` configuration file in your project root:
   ```bash
   touch mypy.ini
   ```

3. Add the following configuration to `mypy.ini`:
   ```ini
   [mypy]
   python_version = 3.12
   warn_return_any = True
   warn_unused_configs = True
   disallow_untyped_defs = True
   disallow_incomplete_defs = True
   check_untyped_defs = True
   disallow_untyped_decorators = True
   no_implicit_optional = True
   warn_redundant_casts = True
   warn_unused_ignores = True
   warn_no_return = True
   warn_unreachable = True

   # Per-module options:
   [mypy-mycode.*]
   disallow_untyped_defs = True
   ```


### Configure Poe the Poet for Task Running

1. Install Poe the Poet using Poetry:
   ```bash
   poetry add poethepoet --dev
   ```

2. Add task configurations to `pyproject.toml`:
   ```toml
   [tool.poe.tasks]
   typecheck = "mypy ."
   # Add more tasks as needed:
   # test = "pytest"
   # lint = "ruff check ."
   ```

3. Now you can run tasks using either:
   ```bash
   poetry run poe typecheck
   # or simply
   poe typecheck
   ```

Poe provides a more flexible task runner compared to Poetry scripts, allowing for:
- Task dependencies
- Custom task arguments
- Python function tasks
- Environment variable configuration
- And more

For example, to create a task with dependencies:

### Configure Ruff for Linting

1. Install Ruff using Poetry:
   ```bash
   poetry add ruff --dev
   ```

2. Add Ruff configuration to `pyproject.toml`:
   ```toml
   [tool.ruff]
   # Enable pycodestyle ('E'), pyflakes ('F'), and import sorting ('I')
   select = ["E", "F", "I"]
   
   # Same as Black.
   line-length = 88
   
   # Assume Python 3.12
   target-version = "py312"
   
   [tool.ruff.per-file-ignores]
   # Ignore `F401` (unused imports) in all `__init__.py` files
   "__init__.py" = ["F401"]
   ```

3. Add a lint task to your Poe configuration in `pyproject.toml`:
   ```toml
   [tool.poe.tasks]
   lint = "ruff check ."
   lint-fix = "ruff check . --fix"
   ```

4. Run the linter using:
   ```bash
   poetry run poe lint
   # or to automatically fix issues:
   poetry run poe lint-fix
   ```

Ruff is an extremely fast Python linter written in Rust, providing:
- Speed: 10-100x faster than traditional Python linters
- Compatibility with popular linting rules (flake8, isort, etc.)
- Automatic code fixes for many common issues
- Highly configurable through `pyproject.toml`









