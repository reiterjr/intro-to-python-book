# Environment and tooling

## Virtual environments

A **virtual environment** is an isolated directory of packages for **one** project so you do not pollute your system Python.

```bash
python3 -m venv .venv
```

**Activate:**

- **macOS / Linux:** `source .venv/bin/activate`
- **Windows (cmd):** `.venv\Scripts\activate.bat`
- **Windows (PowerShell):** `.venv\Scripts\Activate.ps1`

Your prompt often shows `(.venv)` when active. **Deactivate** with `deactivate`.

## Installing dependencies

Your Classroom template may ship **`pyproject.toml`** or **`requirements.txt`**. Typical packages for this series (names only—versions come from the template):

- **`fastapi`** — web framework
- **`uvicorn`** — ASGI server
- **`pydantic`** — data validation / models
- **`httpx`** — HTTP client

Example with **pip**:

```bash
pip install fastapi uvicorn pydantic httpx
```

Or follow your template’s **`pip install -e .`**, **`uv sync`**, or **Poetry** instructions.

> [!TIP]
> Run **`python --version`** and **`which python`** (or **`where python`** on Windows) **after** activating the venv to confirm you are not using the wrong interpreter.

## Running a module

```bash
python -m uvicorn myapp:app --reload
```

(Exact **module path** and **`app`** variable name depend on how you structure your capstone.)

## Implement

1. Create and activate a **venv** in your Classroom repo.
2. Install dependencies per **template** instructions.
3. Confirm **`python -c "import fastapi; import uvicorn"`** succeeds.
4. **Commit** any lockfiles or config your instructor wants tracked, then **push**.
