---
draft: true
title: "# Python Development Environment Setup: A Comprehensive Guide to pyenv and Poetry"
date: 2020-01-01
description: "Setting up a Python development environment can be challenging, especially when managing multiple projects with different Python versions…"
---
Setting up a Python development environment can be challenging, especially when managing multiple projects with different Python versions and dependencies. This comprehensive guide will walk you through using pyenv and Poetry to create a robust, maintainable Python development environment.

## The Dynamic Duo: pyenv and Poetry

Before diving into the setup process, let's understand why we need these tools and how they work together:

**pyenv** manages your Python interpreters:
- Installs and manages multiple Python versions on your system
- Allows you to specify different Python versions for different projects
- Handles seamless switching between Python versions

**Poetry** handles your project dependencies:
- Manages virtual environments for your projects
- Handles package installation, updates, and removal
- Ensures dependency version consistency
- Generates lock files to freeze dependency versions

Together, they create a powerful workflow that makes Python development more efficient and reliable.

## Getting Started

### System Requirements
- Windows 7 or newer
- PowerShell 5.0 or newer
- Git (recommended)

### Installing pyenv-win

First, let's install pyenv for Windows. Open PowerShell and run:

```powershell
Invoke-WebRequest -UseBasicParsing -Uri "[https://raw.githubusercontent.com/pyenv-win/pyenv-win/master/pyenv-win/install-pyenv-win.ps1](https://raw.githubusercontent.com/pyenv-win/pyenv-win/master/pyenv-win/install-pyenv-win.ps1)" -OutFile "./install-pyenv-win.ps1"; &"./install-pyenv-win.ps1"
```

Verify the installation:
```powershell
pyenv --version
```

### Installing Poetry

Next, install Poetry using PowerShell:

```powershell
(Invoke-WebRequest -Uri [https://install.python-poetry.org](https://install.python-poetry.org) -UseBasicParsing).Content | python -
```

If you need to manually add Poetry to your PATH:
```powershell
$env:Path += ";C:\Users\$env:USERNAME\AppData\Roaming\Python\Scripts"
```

Verify the installation:
```powershell
poetry --version
```

## Setting Up Your Development Environment

### Installing Python Versions

With pyenv installed, you can now manage multiple Python versions:

```powershell
# View available Python versions
pyenv install --list

# Install a specific version (e.g., 3.8.10)
pyenv install 3.8.10

# Check installed versions
pyenv versions
```

### Creating a New Project

Let's set up a new Python project:

```powershell
# Create and enter project directory
mkdir my-project
cd my-project

# Set local Python version
pyenv local 3.8.10

# Initialize project (choose one):
poetry new project-name    # Create new project
# or
poetry init               # Initialize in existing directory

# Configure virtual environment in project directory (recommended)
poetry config virtualenvs.in-project true
poetry install            # Install all packages from lock file
```

### IDE Configuration (PyCharm)

1. Open Project Settings (File → Settings → Project → Python Interpreter)
2. Click the gear icon → Add
3. Select "Poetry Environment"
4. Choose the virtual environment in your project directory

## Managing Dependencies with Poetry

### Basic Package Management

Here are the essential commands for managing packages:

```powershell
# Installing packages
poetry add package_name               # Install single package
poetry add package_name==1.2.3        # Install specific version
poetry add "package_name>=1.2.3"      # Install minimum version
poetry add package1 package2          # Install multiple packages
poetry add pytest --dev               # Install development dependency

# Removing packages
poetry remove package_name
poetry remove package1 package2
poetry remove pytest --dev            # Remove development dependency

# Updating packages
poetry update                         # Update all packages
poetry update package_name            # Update specific package
poetry update --dev                   # Update development dependencies
```
