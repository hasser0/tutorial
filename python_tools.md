- [Concepts](#concepts)
- [Project files](#project-files)
- [Python tools](#python-tools)
- [Python versions and virtual environments](#python-versions-and-virtual-environments)
- [Jupyter lab](#jupyter-lab)

# Concepts

+ modules: py files
+ package: several modules
    - distribution package: Pillow
    - import package: PIL
    - regular packages: congruent with file structure
    - namespace packages: package in several folder
+ build backends
    - hatch
    - setuptools
    - poetry (pure python)
    - flit (pure python)
+ build frontends:
    - build
    - pip
+ pyproject.toml: PEP standard for package building
+ editable install: for development `pip install -e .`
+ flat layout
+ src layout

# Project files
+ pyproject.toml
+ README.md
+ LICENSE: No license equivalent to private software
+ MANIFEST.in: Non python files to include in distribution package

# Python tools

+ pyenv: version manager and virtual environments
+ pipx: virtual environment isolated python applications
    - build: package builder (frontend for setuptools)
    - tox
    - pre-commit
    - cookiecutter
+ pip: package installation

# Python versions and virtual environments
```sh
pyenv install <version>
pyenv uninstall <version>
pyenv versions
pyenv local <version>
pyenv global <version>

pyenv virtualenv <venv_name>
pyenv virtualenv-delete <venv_name>
pyenv activate <venv_name>
pyenv deactivate <venv_name>
```

# Jupyter lab

```sh
pyenv activate <venv_name>
pip install ipykernel
ipython kernel install --name "<env-name>" --user

jupyter kernelspec list
jupyter kernelspec remove <env-name>
```
