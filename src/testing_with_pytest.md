# Testing with pytest
> [!TIP]
>
> For this chapter, you will only be creating and testing locally. The GitHub Classroom is already setup with tests to grade your solutions.

## Install pytest locally

On your machine (using the same Python you use for the assignment—see the **virtual environments** chapter at the end of the book if you use a venv):

```bash
pip install pytest
```

Then run from your project root (where your code and the autograder’s layout expect to run):

```bash
pytest
```

## A function you can test

Prefer testing **functions that return values** as they are often easier to assert:

```python
# greeting.py
# a function that accepts a str typed argument and returns a str typed value
def greeting(name: str) -> str:
    return f"Hello, {name}!"
```

```python
# tests/test_greeting.py  (LOCAL ONLY — see warning below)
from greeting import greeting

def test_greeting():
    assert greeting("World") == "Hello, World!"
```

Run: **`pytest test_greeting.py -v`**

### Example test output
```zsh
pytest -v 
===================================================================================== test session starts =====================================================================================
platform darwin -- Python 3.14.3, pytest-9.0.2, pluggy-1.6.0 -- /Users/user/.local/pipx/venvs/pytest/bin/python
cachedir: .pytest_cache
rootdir: /Users/user/Documents/Student-Views/pygames
collected 1 item                                                                                                                                                                              

test_greeting.py::test_greeting_name PASSED                                                                                                                                             [100%]

====================================================================================== 1 passed in 0.00s ======================================================================================
```

> [!TIP]
>
> `pytest` will automatically find your files that have the "test" prefix or suffix and use it. The `-v` is nice to see more details about the test.

## Testing output
###  Using capsys

To check **`print("Hello, World!")`**, capture **stdout** with pytest’s **`capsys`** fixture:

```python
# hello.py
def say_hello() -> None:
    print("Hello, World!")
```

```python
# tests/test_hello.py  (LOCAL ONLY)
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

That setup is useful when some **code under test** should map each input to a known result—here, squaring integers. A tiny module might define:

```python
# math_utils.py
def square(n: int) -> int:
    return n * n
```

The same parametrized table then targets that function: use `assert square(n) == expected` in the test body instead of inlining `n * n`, so the examples exercise **your** implementation, not only the math in the test.

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
# tests/test_hello.py

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

## Your Turn

> [!NOTE]
>
> Reminder, because there is no assigment for this chapter, there is no classroom repo to push your code.

Recreate what was shown above under [A function you can test](./testing_with_pytest.md#a-function-you-can-test) but modify it a bit. Instead of the `greeting` function accepting a `str` parameter, have it accept an `int` parameter for a zip code. Optionally, this is a great spot to practice parameter testing. 

The `return` statement can still be a formatted string. For example, `return f"Zip code {zipcode}"`.