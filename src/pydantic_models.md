# Pydantic models

> [!IMPORTANT]
>
> **GitHub Classroom**
>
> - Accept the [GitHub Classroom assignment]() for this chapter.
> - Complete any **email / invitation** steps your course requires.
> - **Clone** your repo and open the project in your editor.

**Pydantic** is a library that **validates** incoming data and builds **regular Python objects** you can use with attribute access (`obj.field`). It shines at system boundaries: **JSON** from HTTP APIs, records from databases, or anything parsed from text. If a value has the wrong type or breaks a rule you set, Pydantic raises a clear error instead of letting bad data spread through your code.

The examples below stay small so you can focus on syntax and validation first.

Install locally if needed (your template may already include it):

```bash
pip install pydantic
```

## `BaseModel` and fields

### What `BaseModel` is

`BaseModel` is a class you **subclass**. Pydantic injects behavior into that subclass: it knows how to **construct** instances from arguments or from mapping-like data (for example a `dict` that came from `json.loads`), **check** types and constraints, and **convert** simple values when safe (for example a numeric string to an `int`, depending on configuration).

You do not call `BaseModel` directly. You write:

```python
from pydantic import BaseModel, Field

class ListenerHeader(BaseModel):
    name: str
    port: int
    enabled: bool = Field(default=True)

class ListenerOptions(BaseModel):
    use_ssl: bool = Field(default=False)
    jitter: int = Field(default=1)
    port: int = Field(default=4444)
```

Here `ListenerHeader` and `ListenerOptions` are **model classes**. Each **field** is declared with a **name** and a **type annotation** (`str`, `int`, `bool`, …). Together, the annotations describe the **shape** of valid data—like a schema for one record.

### Fields: annotations vs `Field(...)`

A **field** is one named property of the model.

- **Type only** — `name: str` means “this attribute must be a string.” There is no default, so **`name` is required** when you construct the model.
- **`Field(...)`** — use this when you need a **default**, extra **validation** (minimum length, numeric bounds, regex), **documentation** metadata, or **aliases** for names that differ between Python and JSON.

So: **`Field(default=True)`** means “if the caller omits `enabled`, use `True`.” Without a default, the caller must supply `name` and `port` every time.

Rough mental model:

| Declaration | Meaning |
|-------------|---------|
| `port: int` | Required; must be an `int` (or value Pydantic can coerce to `int`, depending on settings). |
| `enabled: bool = Field(default=True)` | Optional at construction time; defaults to `True`. |
| `jitter: int = Field(default=1, ge=0)` | Default `1`; `ge=0` means “greater than or equal to 0” (example of a constraint you will see in docs and rubrics). |

You can also write simple defaults without `Field`, for example `enabled: bool = True`. `Field` is still preferred when you add **constraints** or want to attach **descriptions** for generated documentation.

### After validation: ordinary attributes

Once Pydantic has built an instance, you read data with **dot notation**: `h.name`, `h.port`. The object is meant to be a **convenient, typed** view of the payload—not a loose `dict` of `Any`.

### From a `dict` (JSON-shaped data)

APIs usually give you a `dict`, not keyword arguments. With Pydantic v2 you typically use `model_validate`:

```python
data = {"name": "alpha", "port": 8080}
h = ListenerHeader.model_validate(data)
```

If `data` is missing a required key or has incompatible types, Pydantic raises `pydantic.ValidationError`. You can catch it or let it propagate; printing the exception shows **which fields** failed and **why**:

```python
from pydantic import ValidationError

# Missing required field `port`
try:
    ListenerHeader.model_validate({"name": "alpha"})
except ValidationError as err:
    print(err)
```

Example output (wording may vary slightly by Pydantic version):

```text
1 validation error for ListenerHeader
port
  Field required [type=missing, input_value={'name': 'alpha'}, input_type=dict]
```

Wrong type for `port`:

```python
try:
    ListenerHeader.model_validate({"name": "alpha", "port": "nope"})
except ValidationError as err:
    print(err)
```

```text
1 validation error for ListenerHeader
port
  Input should be a valid integer, unable to parse string as an integer [type=int_parsing, input_value='nope', input_type=str]
```

For programmatic handling, `err.errors()` returns a list of dicts with keys like `type`, `loc`, `msg`, and `input`.

To go back to a plain `dict` (for example to serialize again), use `model_dump()`:

```python
dumps = h.model_dump()  # {"name": "...", "port": ..., "enabled": ...}
print(f"the model {dumps}")
```

## Creating and reading instances

You can construct models with **keyword arguments** matching field names:

```python
h = ListenerHeader(name="alpha", port=8080)
print(h.name, h.port, h.enabled)  # enabled defaults True
```

Extra keys in a `dict` are ignored by default unless you change model configuration; missing required fields or wrong types trigger validation errors. That is the main payoff: **invalid data fails fast** at the edge of your program.

## `Enum` and `StrEnum` (optional)

Sometimes a field should only allow **one of several fixed values**. Python’s `enum.Enum` (and `enum.StrEnum` on Python 3.11+) models that set; Pydantic accepts the member or the underlying value when validating.

```python
from enum import Enum

class Status(Enum):
    pending = "pending"
    done = "done"
```

`StrEnum` makes each member **also behave as a `str`**—natural when the serialized JSON should be a string like `"pending"`, not a separate JSON type.

## Your Turn

> [!NOTE]
>
> Refer to your source code files for more details on what to do.

> [!IMPORTANT]
>
> Commit often and push your final solution when ready


1. Define `BaseModel` classes for something simple (for example `User` with `username`, `age`, and any default or `Field` constraints your rubric asks for).
2. Instantiate from literals or from a `dict` with `model_validate`, then print a field.
3. **Commit** and **push**.
