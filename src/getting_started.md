# Getting started: syntax, `print`, and `if __name__`

## Statements and indentation

Python uses **indentation** (usually 4 spaces) to group statements. No curly braces.

```python
if True:
    print("indented block")
print("back to top level")
```

## Variables and basic types

Names point to objects. Common built-ins:

```python
count: int = 670
label: str = "SEC"
ok: bool = True
nothing: None = None
```

**Type annotations** (`: int`) are optional at runtime but help editors and tools; your template may require them.

## `print` and f-strings

```python
name = "Python"
print("Hello,", name)
print(f"Hello, {name}! course={670}")
```

## The `if __name__ == "__main__":` guard

When Python runs a file **as the main script**, the special name **`__main__`** is set. When the same file is **imported**, it is usually the **module name**. The guard runs code only for “run directly”:

```python
def main() -> None:
    print("starting")

if __name__ == "__main__":
    main()
```

This pattern matches how **`server.py`**-style modules can both define an **`app`** for Uvicorn and optionally run startup code.

## Implement

1. Add a small **`main.py`** (or use the template’s entry file) that defines **`main()`**, prints a greeting with an **f-string**, and calls it under **`if __name__ == "__main__"`**.
2. Run: **`python main.py`**
3. **Commit** and **push**.
