- [Concepts](#concepts)
- [Jupyter lab](#jupyter-lab)
- [Python tools](#python-tools)
- [Annotations](#annotations)

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
+ build frontends
    - build
    - pip
+ pyproject.toml: PEP standard for package building
+ editable install: for development `pip install -e .`
+ flat layout
+ src layout


# Jupyter lab

```sh
pyenv activate <venv_name>
pip install ipykernel
ipython kernel install --name "<env-name>" --user

jupyter kernelspec list
jupyter kernelspec remove <env-name>
```

# Python tools

| Tool                | Tool type                   | Configuration file        |
|:-------------------:|:----------------------------|:--------------------------|
| pyenv               | environment/version manager |                           |
| pipx                | isolated tool installer     |                           |
| pip                 | package manager             |                           |
| jupyterlab          | notebook server             |                           |
| ipython             | interactive python console  |                           |
| cookiecutter        | project template creator    |                           |
| isort               | import sort                 | pyproject.toml, isort.cfg |
| ruff                | linter/formatter            | pyproject.toml, ruff.toml |
| black               | formatter                   | pyproject.toml            |
| pydocstyle          | docstring style             | pyproject.toml            |
| pyright             | static type checker         | pyproject.toml            |
| precommit           | pre commit hooks            | .pre-commit-config.yaml   |
| flake8 (deprecated) | linter                      | .flake8                   |
| pylint (deprecated) | linter                      | .pylintrc                 |


# Annotations

+ Basic
```python
from typing import List
from typing import Tuple
from typing import Dict

def func(param: float) -> float:
def func(param: int) -> int:
def func(param: List) -> List:
def func(param: Tuple) -> Tuple:
def func(param: Tuple[int, str]) -> Tuple:
def func(param: Dict) -> Dict:
# or for python 3.9
def func(param: list) -> list:
def func(param: tuple) -> tuple:
def func(param: tuple[int, str]) -> tuple:
def func(param: tuple[int, ...]) -> tuple:
def func(param: dict) -> dict:
def func(param: dict[str, int]) -> dict:
```

+ Union:
```python
from typing import Union

def func(param: Union[int, float, str]):
# or for python 3.10
def func(param: int | float | str):
```

+ Optional
```python
from typing import Optional
from typing import Union

def func(param: Optional = None):
def func(param: Optional[int]):
def func(param: Union[int, None]):
def func(param: int | None = None):
```

+ Callable
```python
from collections.abc import Callable
from typing import Callable

def func(param: Callable[[int,str], float]):
```

+ NoReturn
```python
from typing import NoReturn

def func() -> NoReturn:
def func() -> None:
```

+ NamedTuple and TypeDict
```python
from typing import TypeDict
from typing import NamedTuple
# typed dict: can change values
class User(TypedDict):
    arg1: str
    arg2: int
# named tuple: cannot change values
class User(NamedTuple):
    arg1: str
    arg2: int
```

+ Protocols
```python
# built-in protocols
from collections.abc import Sized
from collections.abc import Callable
from collections.abc import Awaitable
from collections.abc import Iterable
# user defined
from typing import Protocol
class SetOfDunderFunctions(Protocol):
    def __len__(self) -> int:
        ...
    def __lt__(self) -> bool:
        ...
def func(param: SetOfDunderFunctions):
```

+ Generic Types