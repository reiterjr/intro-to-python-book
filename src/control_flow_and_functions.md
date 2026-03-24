# Control flow and functions

> [!IMPORTANT]
>
> **GitHub Classroom**
>
> - Accept the [GitHub Classroom assignment](https://classroom.github.com/a/p2CE9iqI) for this chapter.
> - Complete any **email / invitation** steps your course requires.
> - **Clone** your repo and open the project in your editor.

## `if` / `elif` / `else`

**Conditional execution** lets a program choose different code paths depending on data. In Python, an **`if`** statement runs a block only when its condition is **true**.

- **`if`**: the first branch; runs if the condition holds.
- **`elif`** (short for “else if”): additional branches, checked **in order** only after earlier conditions failed. Use as many as you need.
- **`else`**: optional “catch-all” that runs when **none** of the `if` / `elif` conditions were true.

Python decides truth using **boolean expressions**: comparisons like `x < y`, combinations with **`and`**, **`or`**, **`not`**, and membership tests like **`x in items`**. Non-boolean values also have **truthiness** (for example, empty strings and empty lists count as false; non-zero numbers and non-empty collections count as true), but for clarity it is usually best to write explicit comparisons when you are learning.

Each branch is **indented** under its header. Only **one** branch from a single `if` / `elif` / … chain runs—the first whose condition is true. Order matters: put **more specific** or **stricter** checks before broader ones, or a broad condition may “win” and hide later checks.

```python
def grade(score: int) -> str:
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    else:
        return "C"
```

Here a score of **95** satisfies `score >= 90`, so Python returns **`"A"`** and never evaluates the `elif` or `else`. A score of **85** fails the first test, passes the second, and returns **`"B"`**. Anything below **80** falls through to **`else`**.

You can use **`if`** alone (no `elif` or `else`), or **`if`** / **`else`** without `elif`. Nested **`if`** statements are allowed when a decision depends on another decision; keep nesting shallow or refactor into functions or clearer conditions so the logic stays readable.

```python
def describe(n: int) -> str:
    if n < 0:
        return "negative"
    elif n == 0:
        return "zero"
    else:
        return "positive"
```

## Loops

Loops **repeat** a block of code. Python’s two main loop forms are **`for`** (definite iteration over a sequence or iterable) and **`while`** (indefinite iteration while a condition stays true).

### `for`

**`for`** assigns each value from an **iterable** to a name and runs the body once per value. Common iterables include **lists**, **strings**, **tuples**, **`range`**, and many built-in and library types.

```python
for i in range(3):
    print(i)

items = ["a", "b", "c"]
for item in items:
    print(item)
```

**`range`** is especially useful with **`for`**:

- **`range(stop)`**: `0` up to **`stop - 1`**.
- **`range(start, stop)`**: from **`start`** up to **`stop - 1`**.
- **`range(start, stop, step)`**: same idea, advancing by **`step`** (can be negative to count down).

```python
for i in range(2, 8, 2):
    print(i)  # 2, 4, 6
```

If you need both an **index** and the **value**, use **`enumerate`**:

```python
for index, letter in enumerate("hi"):
    print(index, letter)
```

**`break`** exits the loop immediately; **`continue`** skips the rest of the current iteration and moves to the next value. Use them sparingly so the loop’s structure stays easy to follow.

### `while`

**`while`** evaluates a **condition** before each iteration. If the condition is true, the body runs; then the condition is checked again. When the condition becomes false, the loop ends without running the body again.

```python
n = 3
while n > 0:
    print(n)
    n -= 1
```

The body must **eventually** make the condition false (or exit with **`break`**), or the loop runs forever—an **infinite loop**. That is sometimes intentional (for example, a server or event loop), but often it is a bug when a counter or state never updates.

**`while`** fits problems where you **do not know in advance** how many iterations you need: reading until a sentinel value, retrying until input is valid, or polling until a condition holds. When you **do** know the count, **`for`** with **`range`** is usually clearer.

```python
x = 1
while x < 100:
    x *= 2
# x is now the first power of 2 that is not less than 100
```

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

## Your Turn
> [!NOTE]
>
> Refer to your source code files for more details on what to do.


> [!IMPORTANT]
>
> Commit often and push your final solution when ready

1. Write **`clamp(value: int, low: int, high: int) -> int`** that returns **`value`** limited to **`[low, high]`**.
2. Write a loop that prints **squares** for **`i`** from **0** through **5** (e.g. `i * i`).
3. **Commit** and **push**.
