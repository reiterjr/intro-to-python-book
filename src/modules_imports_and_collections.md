# Modules, imports, and collections

> [!IMPORTANT]
>
> **GitHub Classroom**
>
> - Accept the [GitHub Classroom assignment]() for this chapter.
> - Complete any **email / invitation** steps your course requires.
> - **Clone** your repo and open the project in your editor.

## What is a module?

A **module** is a `.py` file that Python can load by name. The first time code runs `import some_name`, Python locates `some_name.py` (or a package named `some_name`), executes it once, and keeps the resulting **namespace**—the set of names (variables, functions, classes) defined in that file—in memory. Later imports of the same module reuse that cached result, so top-level code in the file does not run again.

You will work with three broad kinds of modules:

- **The standard library** — modules shipped with Python (`math`, `random`, `pathlib`, …). You do not install them separately; you only `import` them.
- **Third-party packages** — installed with `pip` (or `pipx`) into your environment; they appear as importable top-level names (for example `requests`).
- **Your own modules** — `.py` files in your project (for example `tokens.py`). Python finds them when they sit on the **module search path** (the directory you run from, plus `PYTHONPATH`, plus installed packages).

Thinking in terms of modules helps you **split programs into files**, **reuse code**, and **avoid one giant script**.

## Importing

The `import` statement loads a module and binds a name in the **current** namespace (usually the module that is doing the importing—often your `main` file).

### `import module` — keep the module as a namespace

```python
import string
import random
from datetime import datetime
```

With `import random`, you get a name `random` that refers to the whole module. You call its contents with **dot** notation: `random.choice(...)`, `random.randint(...)`. That makes it obvious **where** each name comes from and avoids clashes with your own function names.

You can shorten the name: `import numpy as np`.

### `from module import name` — pull names into the current file

```python
from datetime import datetime
```

Here `datetime` (the class) is available **without** the `datetime.` prefix. That saves typing for one or two heavily used names. The trade-off is that readers (and you, later) must remember that `datetime` came from the `datetime` package, not from your file.

You can import several names: `from math import sqrt, pi`, and use aliases: `from pathlib import Path as P` (unusual; prefer `Path`).

### `import module` vs `from module import name`

| Style | Typical use | Pros | Cons |
|--------|-------------|------|------|
| `import module` | Libraries you call in many places | Clear origin (`module.function`); fewer name collisions | Slightly more typing |
| `from module import name` | One or two symbols used often | Shorter call sites | Names can shadow yours or other imports; less obvious where a name is defined |

For small scripts and teaching, both are fine. In larger projects, `import module` (or `import module as short`) is often preferred for anything beyond a handful of well-known imports.

### Star imports: `from module import *`

This form imports **every public name** the module exposes (often controlled by `__all__` in the module’s source). Example:

```python
from math import *  # discouraged in real projects
```

**Why avoid it in application and library code?**

- **Namespace pollution** — hundreds of names may appear; you lose a clear list of dependencies at the top of the file.
- **Unclear origin** — reading `sqrt(x)` does not show whether `sqrt` is yours, from `math`, or from another star import.
- **Name clashes** — your own function `log` or a variable `e` can collide with imported names in subtle ways.
- **Tooling** — linters and editors have a harder time resolving names.

Star imports are occasionally used in **interactive** sessions for quick exploration. For homework and production code, prefer explicit imports.

## Making your own modules

Any `.py` file in an importable location is a module. If `tokens.py` sits next to `main.py`, you can write `import tokens` (or `from tokens import random_token`) from `main.py`. Python adds the script’s directory to `sys.path` when you run `main.py`, so sibling files are discoverable.

Conventions that keep this painless:

- Use **lowercase** module filenames with underscores: `random_token.py` is awkward; `tokens.py` is idiomatic.
- Put **reusable** functions and constants in dedicated modules; keep **entry-point** logic (parsing CLI args, `if __name__ == "__main__":`) in `main.py` or a thin `__main__` pattern.
- If a name should only run when the file is executed directly, guard it with `if __name__ == "__main__":` so `import tokens` does not accidentally run demo code.

As projects grow, you group related modules inside a **package** (a folder); see **Packages** below.

## Lists and dicts

### Lists

A **list** is an **ordered** sequence of values. It is **mutable**: you can change elements, append, pop, sort, and so on. Elements can be heterogeneous types, though in practice you often keep one “kind” of thing per list for clarity.

```python
# Create a list of three integers (order is preserved).
nums = [1, 2, 3]

# Add 4 to the end of the list in place.
nums.append(4)

# First item uses index 0 (zero-based indexing).
first = nums[0]

# Negative indices count from the end; -1 is the last element.
last = nums[-1]
```

**Indexing** is **zero-based**: the first item is index `0`. **Negative** indices count from the end. Accessing an index that does not exist raises `IndexError`.

Common patterns: **iterate** with `for x in nums`, **length** with `len(nums)`, **slice** with `nums[1:3]` (a shallow copy of a sub-range). Lists are a good default when **order matters** and you need duplicates or frequent updates at the end.

### Dicts

A **dict** (**dictionary**) maps **keys** to **values**. As of Python 3.7+, dicts preserve **insertion order**; the important mental model is still **lookup by key**, not position.

```python
config = {"host": "127.0.0.1", "port": 8080}
print(config["port"])
config["timeout"] = 30
```

**Keys** must be **hashable** (strings, numbers, tuples of immutables, …). **Values** can be any type. Reading `config["missing"]` for a key that is not there raises `KeyError`. For optional keys, use `.get`:

```python
timeout = config.get("timeout", 30)  # default 30 if absent
```

Dicts shine when you have **labeled** data (records, configs, JSON-like structures). Many APIs and `json.load` results map naturally to **nested dicts and lists**.

### Lists vs dicts (when to reach for which)

- Use a **list** when **order** matters and you identify items by **integer position** (or you only ever scan the whole collection).
- Use a **dict** when you look up by a **meaningful key** (`user_id`, `"host"`, …) or you are modeling **named fields** without defining a class yet.

## Useful standard library snippets

The examples below tie together **imports** and small **functions** you might put in your own utility module. They rely only on the standard library.

`string` exposes useful string **constants** (for example `ascii_uppercase`, `digits`). `random` offers **pseudorandom** choices—fine for games and tokens in exercises; for **security-sensitive** secrets, prefer `secrets` from the standard library instead.

```python
import string
import random

def random_token(length: int = 8) -> str:
    letters = string.ascii_uppercase
    return "".join(random.choice(letters) for _ in range(length))
```

`datetime` in the `datetime` package models dates and times. `datetime.now()` returns the current local time; `.strftime(...)` formats it as a string.

```python
from datetime import datetime

def timestamp() -> str:
    return datetime.now().strftime("%Y-%m-%d %H:%M:%S")
```

Notice `from datetime import datetime`: the **package** is `datetime`, and the **class** is also named `datetime`. The import brings the **class** into scope; the module pattern would be `import datetime` then `datetime.datetime.now()`.

## Packages

A **package** is a **directory** of modules. For classic “regular” packages, the directory contains `__init__.py` (it can be empty). That file marks the directory as a package and can run package-level initialization or re-export names.

Example layout:

```text
myapp/
  __init__.py
  tokens.py
  main.py
```

From `main.py` inside `myapp`, you might use `import myapp.tokens` or `from myapp import tokens` depending on how you run the app and how `sys.path` is set. Course templates often use an `app/` or `src/` layout; **follow the template** so `import` paths match what the autograder and `pytest` expect.

**Subpackages** are nested directories, each with `__init__.py`, forming dotted names like `myapp.data.loaders`. You do not need to design deep hierarchies early; start with **one package folder** and a few modules, then split when files grow large.

## Your Turn

> [!NOTE]
>
> Refer to your source code files for more details on what to do.

> [!IMPORTANT]
>
> Commit often and push your final solution when ready

- Create a folder names modules
- Under modules, create `tokens.py` with a `random_token` function and `timestamp`
- Create an empty `__init__.py` file under the modules folder
- Import and call them from `main.py` or a small test script.
- Build a `dict` representing a fake server config (`host`, `port`, `use_ssl`) and iterate over each field to print its value with a formatted string.
- **Commit** and **push**.
