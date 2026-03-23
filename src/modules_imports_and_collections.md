# Modules, imports, and collections

## Importing

```python
import string
import random
from datetime import datetime
```

- **`import module`** — use **`module.name`**
- **`from module import name`** — use **`name`** directly

## Lists and dicts

**List** — ordered, mutable:

```python
nums = [1, 2, 3]
nums.append(4)
first = nums[0]
```

**Dict** — key → value:

```python
config = {"host": "127.0.0.1", "port": 8080}
print(config["port"])
config["timeout"] = 30
```

JSON-like APIs often map naturally to **dicts**.

## Useful standard library snippets

Small **utility** modules often wrap the standard library like this:

```python
import string
import random

def random_token(length: int = 8) -> str:
    letters = string.ascii_uppercase
    return "".join(random.choice(letters) for _ in range(length))
```

```python
from datetime import datetime

def timestamp() -> str:
    return datetime.now().strftime("%Y-%m-%d %H:%M:%S")
```

## Packages

A **package** is a directory with **`__init__.py`** (can be empty). Your template might use **`app/`** or **`src/`** layout—follow it for **`import`** paths.

## Implement

1. Create a module **`tokens.py`** with **`random_token`** and **`timestamp`** (or reuse names your rubric gives).
2. Import and call them from **`main.py`** or a small test script.
3. Build a **`dict`** representing a fake server config (`host`, `port`, `use_ssl`) and print one field with an f-string.
4. **Commit** and **push**.
