- [Manage python versions](#manage-python-versions)
- [Handle virtual environments](#handle-virtual-environments)
- [Add kernel to jupyter lab](#add-kernel-to-jupyter-lab)
- [Remove kernel](#remove-kernel)

# Manage python versions
```sh
pyenv install <version>
pyenv uninstall <version>
pyenv versions
pyenv local <version>
pyenv global <version>
```

# Handle virtual environments
```sh
pyenv virtualenv <venv_name>
pyenv virtualenv-delete <venv_name>
pyenv activate <venv_name>
pyenv deactivate <venv_name>
```

# Add kernel to jupyter lab
```sh
pyenv activate <venv_name>
pip install ipykernel
ipython kernel install --name "<env-name>" --user
```

# Remove kernel
```sh
jupyter kernelspec list
jupyter kernelspec remove <env-name>
```
