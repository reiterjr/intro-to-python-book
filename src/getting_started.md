# Getting started: `main` and your first program

Start with a single **entry function**—**`main`**—and the **`if __name__ == "__main__":`** guard. Everything else in this chapter (**variables**, **indentation**, **type annotations**, **`print`**) lives **inside** the body of **`main`** so you have one clear place where your program runs.

## The `main` function

A **function** is a named block of code. **`main`** is the conventional name for the starting point of a small script or assignment.

**Function signature** — what you write on the `def` line:

- **`def`** — keyword that starts a function definition  
- **`main`** — the name you (and your autograder) will call  
- **`()`** — parameter list; empty for now  
- **`-> None`** — **return type annotation**: this function does not return a useful value (it returns **`None`** implicitly)

**Function body** — indented lines under the `def`. That is where your program’s work happens.

```python
def main() -> None:
    pass  # placeholder: replace with real code
```

**`pass`** is a no-op: it keeps the body valid while the block is empty. Remove it once you add real statements.

Later in the course you will use the same **“define `main` + guard”** pattern so a file can also expose a **FastAPI `app`**; **Uvicorn** appears only in the final chapters.

## Running the program:
### `if __name__ == "__main__":`

When Python runs a file **as the main script** (e.g. **`python main.py`**), the special module name **`__main__`** is used. When the same file is **imported** elsewhere, its name is usually the **module name** (e.g. **`main`**).

The **guard** calls **`main()`** only when the file is executed directly:

```python
def main() -> None:
    pass

if __name__ == "__main__":
    main()
```

So: **define** `main` at the top level (no indentation), then **invoke** it inside the guard.

## Indentation
### Inside `main`

Python uses **indentation** (usually **4 spaces**) to show what belongs to a block. Everything inside **`main`** must be indented **one level** more than **`def main`**.

```python
def main() -> None:
    print("this line is inside main")

print("this line is NOT inside main — runs at import time")
```

There are **no curly braces**; the colon **`:`** after `def main() -> None:` starts a block, and only indentation defines where it ends.

You can nest blocks (e.g. **`if`** inside **`main`**) by indenting **another** level:

```python
def main() -> None:
    if True:
        print("inside if, inside main")
```

## Variables and type annotations inside `main`

**Variables** are names that refer to values. Declare them **inside** **`main`** when they are only needed for that run.

**Type annotations** tell readers and tools what you intend (`: int`, `: str`, …). They are **optional** at runtime (Python does not enforce them unless you use extra tools) but your **template** or rubric may require them.

```python
def main() -> None:
    count: int = 670
    label: str = "SEC"
    ok: bool = True
    nothing: None = None
```

Add more statements below those lines in the same indented block.

## `print` and f-strings inside `main`

Use **`print`** to send text to standard output (what you see in the terminal).

```python
def main() -> None:
    name = "Python"
    print("Hello,", name)
    print(f"Hello, {name}! course={670}")
```

An **f-string** (`f"..."`) lets you embed expressions in **`{ ... }`** inside the string.

## Putting it together

```python
def main() -> None:
    course: int = 670
    topic: str = "Python"
    print(f"Welcome to {topic} — course {course}")

if __name__ == "__main__":
    main()
```

Run with:

```bash
python main.py
```

## Your Turn

1. Inside **`main`**, add **variables** (with annotations), **`print`**  **f-strings**
2. Run: **`python main.py`**
3. Optionally try **`pytest` locally** (see **[Testing with pytest](./testing_with_pytest.md)**)—**do not push** practice tests.
4. **Commit** and **push** your solution source only.
