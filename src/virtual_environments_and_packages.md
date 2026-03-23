# Virtual environments and packages (before the web modules)

By this point in the course you have been writing Python with whatever interpreter your template and editor use. For the **final web chapters**, you want an isolated environment and the right **libraries** installed cleanly.

## Virtual environments

A **virtual environment** is a directory of packages for **one** project.

```bash
python3 -m venv .venv
```

**Activate:**

- **macOS / Linux:** `source .venv/bin/activate`
- **Windows (cmd):** `.venv\Scripts\activate.bat`
- **Windows (PowerShell):** `.venv\Scripts\Activate.ps1`

**Deactivate:** `deactivate`

> [!TIP]
> After activating, run **`python --version`** and **`which python`** (or **`where python`**) to confirm you are inside the venv.

## Installing web and HTTP dependencies

Your **Classroom template** may ship **`pyproject.toml`** or **`requirements.txt`**. Typical packages for the closing chapters:

- **`fastapi`** — web framework  
- **`uvicorn`** — ASGI server  
- **`pydantic`** — data validation (you already used it earlier; ensure it is installed here if needed)  
- **`httpx`** — HTTP client  

Example:

```bash
pip install fastapi uvicorn pydantic httpx
```

Or use **`pip install -r requirements.txt`**, **`uv sync`**, or **Poetry**, per your template.

## Running the web server (preview)

Once **`app`** exists in a module (next chapters):

```bash
uvicorn app:app --host 0.0.0.0 --port 8000 --reload
```

First **`app`** = module name; second **`app`** = **`FastAPI()`** instance.

## Implement

1. Create and activate **`.venv`** in your repo (if you have not already).
2. Install **FastAPI**, **Uvicorn**, **HTTPX**, and ensure **Pydantic** is available.
3. Confirm **`python -c "import fastapi, uvicorn, httpx, pydantic"`** succeeds.
4. **Commit** lockfiles / dependency files your instructor wants—**not** `.venv` itself.
