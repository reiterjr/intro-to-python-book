# Control flow and functions

## `if` / `elif` / `else`

```python
def grade(score: int) -> str:
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    else:
        return "C"
```

## Loops

**`for`** iterates over any iterable (list, string, range, …):

```python
for i in range(3):
    print(i)

items = ["a", "b", "c"]
for item in items:
    print(item)
```

**`while`** repeats while a condition is true.

## Functions

Define with **`def`**, document with a **docstring**, optionally annotate parameters and return type:

```python
def add(a: int, b: int) -> int:
    """Return the sum of a and b."""
    return a + b
```

**Multiple return values** use a tuple (often unpacked):

```python
def divmod_custom(a: int, b: int) -> tuple[int, int]:
    return a // b, a % b

q, r = divmod_custom(10, 3)
```

## Implement

1. Write **`clamp(value: int, low: int, high: int) -> int`** that returns **`value`** limited to **`[low, high]`**.
2. Write a loop that prints **squares** for **`i`** from **0** through **5** (e.g. `i * i`).
3. **Commit** and **push**.
