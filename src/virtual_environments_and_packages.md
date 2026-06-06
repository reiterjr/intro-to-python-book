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

Create **`hello.py`** in your project root (any filename is fine; just something to keep it separate from **`app.py`** in later chapters):

```python
# hello.py
import argparse

import uvicorn
from fastapi import FastAPI

# the object for the app itself
app = FastAPI()


# specify the root page of the app
@app.get("/")
def root():
    return {"message": "it works!"}


# the main function to drive all of it
def main() -> None:
    parser = argparse.ArgumentParser(description="Run the demo API")
    parser.add_argument("--host", default="127.0.0.1", help="Host to bind")
    parser.add_argument("--port", type=int, default=8000, help="Port to listen on")
    parser.add_argument(
        "--reload",
        action="store_true",
        help="Restart on file changes (development only)",
    )
    args = parser.parse_args()
    uvicorn.run("hello:app", host=args.host, port=args.port, reload=args.reload)


if __name__ == "__main__":
    main()
```

Two routes, two JSON objects—same pattern you will use for the real web chapters. **`main()`** starts Uvicorn when you run the file as a script (see below).

### Start the server

With **`.venv` activated**, from the directory that contains **`hello.py`**, you can start the app in either of two ways.

**1. Uvicorn CLI** (points at the **`app`** object in your module):

```bash
uvicorn hello:app --host 127.0.0.1 --port 8000 --reload
```

- First **`hello`** = module name (**`hello.py`** → **`hello`**).
- Second **`app`** = the **`FastAPI()`** instance in that file.
- **`--reload`** restarts when you save changes (development only).

**2. From `main()` inside the file** (same **`--host`**, **`--port`**, and **`--reload`** flags, parsed in Python):

```bash
python hello.py --host 127.0.0.1 --port 8000 --reload
```

Omit flags to use the defaults (**`127.0.0.1`**, port **`8000`**, no reload). Example without reload:

```bash
python hello.py --port 9000
```

Both approaches call Uvicorn under the hood. Use whichever you prefer; the **[Uvicorn](./uvicorn_server.md)** chapter compares them in more detail.

You should see log lines ending with something like **`Uvicorn running on http://127.0.0.1:8000`** (host/port match what you passed).

### Check the responses

In another terminal (venv activated is optional for **`curl`**):

```bash
curl http://127.0.0.1:8000/
```

Expected bodies:

```json
{"message":"it works!"}
```

Or open those URLs in a browser. FastAPI also serves interactive docs at **`http://127.0.0.1:8000/docs`**.

Stop the server with **Ctrl+C** when you are done.

> [!TIP]
> If **`uvicorn`** is “command not found”, the venv is probably not activated, or install failed—run **`which uvicorn`** (or **`where uvicorn`**) and confirm it points inside **`.venv`**.

## Your Turn

1. Create and activate **`.venv`** in your repo (if you have not already).
2. Install **FastAPI**, **Uvicorn**, **HTTPX**, and ensure **Pydantic** is available.
3. Confirm **`python -c "import fastapi, uvicorn, httpx, pydantic"`** succeeds.
4. Add **`hello.py`** with **`GET /`** → **`{"message": "ping"}`** and **`GET /ping`** → **`{"message": "pong"}`**.
5. Start the server with **`python hello.py --host 127.0.0.1 --port 8000 --reload`**, then verify both URLs (browser, **`curl`**, or **`/docs`**).
6. **Commit** lockfiles / dependency files your instructor wants—**not** **`.venv`** itself. **`hello.py`** is optional to commit unless your instructor asks for it.
