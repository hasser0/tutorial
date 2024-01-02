- [Create new kernel](#create-new-kernel)
- [Remove kernel](#remove-kernel)

# Create new kernel
```sh
python3 -m venv env-name
source env-name/bin/activate
pip install ipykernel
ipython kernel install --name "env-name" --user
```

# Remove kernel
```sh
jupyter kernelspec list
jupyter kernelspec remove env-name
```
