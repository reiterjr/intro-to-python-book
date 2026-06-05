# Virtual environments and packages

> [!IMPORTANT]
>
> **GitHub Classroom**
>
> - Accept the [GitHub Classroom assignment](https://classroom.github.com/a/DKToSdAw) for this chapter.
> - Complete any **email / invitation** steps your course requires.
> - **Clone** your repo and open the project in your editor.

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

Or use **`pip install -r requirements.txt`**, **`uv sync`**, or **`Poetry`**, per your template.

## A tiny API in your venv

After installing packages, prove the venv works by running a **minimal** FastAPI app. Don't worry, the full capstone project is just around the corner.

Create **`ping_demo.py`** in your project root (any filename is fine; just something to keep it separate from **`app.py`** in later chapters):

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def root():
    return {"message": "ping"}


@app.get("/ping")
def ping():
    return {"message": "pong"}
```

Two routes, two JSON objects—same pattern you will use for the real web chapters.

### Start the server

With **`.venv` activated**, from the directory that contains **`ping_demo.py`**:

```bash
uvicorn ping_demo:app --host 127.0.0.1 --port 8000 --reload
```

- First **`ping_demo`** = module name (**`ping_demo.py`** → **`ping_demo`**).
- Second **`app`** = the **`FastAPI()`** instance in that file.
- **`--reload`** restarts when you save changes (development only).

You should see Uvicorn log lines ending with something like **`Uvicorn running on http://127.0.0.1:8000`**.

### Check the responses

In another terminal (venv activated is optional for **`curl`**):

```bash
curl http://127.0.0.1:8000/
curl http://127.0.0.1:8000/ping
```

Expected bodies:

```json
{"message":"ping"}
```

```json
{"message":"pong"}
```

Or open those URLs in a browser. FastAPI also serves interactive docs at **`http://127.0.0.1:8000/docs`**—try **`GET /ping`** from there.

Stop the server with **Ctrl+C** when you are done.

> [!TIP]
> If **`uvicorn`** is “command not found”, the venv is probably not activated, or install failed—run **`which uvicorn`** (or **`where uvicorn`**) and confirm it points inside **`.venv`**.

## What comes next

The **[FastAPI routes](./fastapi_routes.md)** chapter replaces this demo with your project **`app.py`** (including the capstone **`/ping`** route). **[Uvicorn](./uvicorn_server.md)** goes deeper on CLI vs programmatic startup. For now, a successful **`ping`** / **`pong`** run means your environment is ready.

## Implement

1. Create and activate **`.venv`** in your repo (if you have not already).
2. Install **FastAPI**, **Uvicorn**, **HTTPX**, and ensure **Pydantic** is available.
3. Confirm **`python -c "import fastapi, uvicorn, httpx, pydantic"`** succeeds.
4. Add **`ping_demo.py`** with **`GET /`** → **`{"message": "ping"}`** and **`GET /ping`** → **`{"message": "pong"}`**.
5. Run **`uvicorn ping_demo:app --host 127.0.0.1 --port 8000 --reload`** and verify both URLs (browser, **`curl`**, or **`/docs`**).
6. **Commit** lockfiles / dependency files your instructor wants—**not** **`.venv`** itself. **`ping_demo.py`** is optional to commit unless your instructor asks for it.
