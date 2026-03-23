# Testing with pytest

Your **GitHub Classroom** assignment is graded in part by an **autograder** that runs **`pytest`** against your repository. You should learn to run tests **locally** so you catch failures before you push.

## Install pytest locally

On your machine (using the same Python you use for the assignment—see the **virtual environments** chapter at the end of the book if you use a venv):

```bash
pip install pytest
```

Then run from your project root (where your code and the autograder’s layout expect to run):

```bash
pytest
```

Your **template** may already list **`pytest`** as a dependency; use whatever **`pip install -r`** / **`uv sync`** instructions you were given.

## A function you can test

Prefer testing **functions that return values**—easy to assert:

```python
# greeting.py
def greeting(name: str) -> str:
    return f"Hello, {name}!"
```

```python
# test_greeting.py  (LOCAL ONLY — see warning below)
from greeting import greeting

def test_greeting_includes_name():
    assert greeting("World") == "Hello, World!"
```

Run: **`pytest test_greeting.py -v`**

## Testing `print` output with `capsys`

To check **`print("Hello, World!")`**, capture **stdout** with pytest’s **`capsys`** fixture:

```python
# hello.py
def say_hello() -> None:
    print("Hello, World!")
```

```python
# test_hello.py  (LOCAL ONLY)
from hello import say_hello

def test_say_hello_prints(capsys):
    say_hello()
    captured = capsys.readouterr()
    assert captured.out.strip() == "Hello, World!"
```

**`captured.out`** is everything printed to standard output during the test.

## Parametrize (optional)

Run one test logic with many inputs:

```python
import pytest

@pytest.mark.parametrize("n,expected", [(1, 1), (2, 4), (3, 9)])
def test_square(n, expected):
    assert n * n == expected
```

> [!WARNING]
> **Do not push your own tests**  
> The course **autograder** supplies the official **`pytest`** tests on GitHub Classroom. They run in CI when you push; you do **not** see those files as something to submit.
>
> **Locally**, you may create **`test_*.py`** files or a **`tests/`** folder to practice. **Never `git add` / commit / push** your personal test files unless your instructor explicitly tells you to. Pushing extra tests can interfere with grading, collide with hidden tests, or violate the assignment rules.
>
> **Do not push** **`__pycache__`**, **`.pytest_cache/`**, or local virtualenv folders either—use **`.gitignore`** as your template provides.

## Final Example
Here is one final example complete with test output. For this one, the main program is quite simple, just like what you have seen already.

```python
def main() -> None:
    # intentionally missing the comma "," after Hello
    print("Hello World!")    


if __name__ == "__main__":
    main()
```

When you run it, you should hope to see this:

```zsh
➜  hello git:(main) ✗ python3 hello.py 
Hello World!
```

To test, you could make this:

```python
import pytest
import hello  # ← imports the hello.py module


def test_main_prints_hello_world(capsys):
    """Check that main() prints exactly 'Hello, World!'"""
    
    # Run the student's main function
    hello.main()
    
    # Capture everything that was printed
    captured = capsys.readouterr()
    
    # Clean up the output (remove extra spaces/newlines)
    output = captured.out.strip()
    expected = "Hello, World!"
    
    # Use pytest.fail() instead of assert → no ugly diff at the bottom!
    if output != expected:
        pytest.fail(f"Expected exactly this: {expected} But your main() printed: \
{output}")
```

The output of this failed program looks like this:

```zsh
➜  hello git:(main) ✗ pytest              
================================================================================= test session starts =================================================================================
platform darwin -- Python 3.14.3, pytest-9.0.2, pluggy-1.6.0
rootdir: /Users/user/Documents/IntroToPythonTemplate/IntroToPythonTemplate/hello
collected 1 item                                                                                                                                                                      

test_hello.py F                                                                                                                                                                 [100%]

====================================================================================== FAILURES =======================================================================================
____________________________________________________________________________ test_main_prints_hello_world _____________________________________________________________________________

capsys = <_pytest.capture.CaptureFixture object at 0x10be817f0>

    def test_main_prints_hello_world(capsys):
        """Check that main() prints exactly 'Hello, World!'"""
    
        # Run the student's main function
        hello.main()
    
        # Capture everything that was printed
        captured = capsys.readouterr()
    
        # Clean up the output (remove extra spaces/newlines)
        output = captured.out.strip()
        expected = "Hello, World!"
    
        # Use pytest.fail() instead of assert → no ugly diff at the bottom!
        if output != expected:
>           pytest.fail(f"Expected exactly this: {expected} But your main() printed: \
    {output}")
E           Failed: Expected exactly this: Hello, World! But your main() printed: Hello World!

test_hello.py:20: Failed
=============================================================================== short test summary info ===============================================================================
FAILED test_hello.py::test_main_prints_hello_world - Failed: Expected exactly this: Hello, World! But your main() printed: Hello World!
================================================================================== 1 failed in 0.05s ==================================================================================
```

Your turn!

## Implement

1. **`pip install pytest`** (or use your template’s env).
2. Write **`greet(name)`** that returns a string and a **`say_hi()`** that **`print`s** a fixed greeting; verify both **locally** with **`pytest`** and **`capsys`**.
3. **Delete or keep** your practice tests **only on disk**—do **not** push them.
4. **Commit** and **push** only your **solution source** (e.g. **`hello.py`**, **`main.py`**) per the rubric.
