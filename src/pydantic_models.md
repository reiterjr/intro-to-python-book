# Pydantic models

**Pydantic** validates and parses data into Python objects—ideal for **JSON** bodies once you reach the web chapters. The examples below stay small so you can focus on syntax and validation first.

Install locally if needed (your template may already include it):

```bash
pip install pydantic
```

## `BaseModel` and fields

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

- **Required** fields have no default.
- **`Field(default=...)`** sets defaults and can carry validation rules (min, max, regex, …).

## Creating and reading instances

```python
h = ListenerHeader(name="alpha", port=8080)
print(h.name, h.port, h.enabled)  # enabled defaults True
```

## `Enum` and `StrEnum` (optional)

Enumerations restrict values to a fixed set:

```python
from enum import Enum

class Status(Enum):
    pending = "pending"
    done = "done"
```

**`StrEnum`** (Python 3.11+) makes members **strings** as well—handy for JSON.

## Implement

1. Define **`BaseModel`** classes for something simple (e.g. **`User`** with **`username`**, **`age`** with default **`Field`** constraints if your rubric asks).
2. Instantiate from literals, then print a field.
3. **Commit** and **push**.
