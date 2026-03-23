# Running the server with Uvicorn

**Uvicorn** is an **ASGI** server. It runs your **FastAPI** **`app`** object and listens on a **host** and **port**.

## Command line

From the directory that can import your module:

```bash
uvicorn app:app --host 0.0.0.0 --port 8000 --reload
```

- First **`app`**: Python **module** name (e.g. **`app.py`** → **`app`**).
- Second **`app`**: variable holding the **`FastAPI()`** instance.

**`--reload`** restarts on file changes (development only).

## In Python (programmatic startup)

You can start Uvicorn from code instead of the CLI:

```python
import uvicorn
from fastapi import FastAPI

app = FastAPI()

def run(port: int = 5050) -> None:
    config = uvicorn.Config(app=app, host="0.0.0.0", port=port, log_level="info")
    server = uvicorn.Server(config=config)
    server.run()

if __name__ == "__main__":
    run()
```

Use this when your rubric asks for **`python app.py`** instead of the **`uvicorn`** CLI.

### Threading note (advanced)

Some applications run Uvicorn in a **background thread** next to other code. For this course, run **only** Uvicorn in the foreground for labs unless your instructor assigns threading.

## Check it

1. Start the server.
2. Visit **`http://127.0.0.1:8000/docs`** for interactive **OpenAPI** docs (FastAPI feature).
3. **`curl http://127.0.0.1:8000/ping`** should show **`{"message":"pong"}`**.

## Implement

1. Document in **`README.md`** the exact command to start your app (or **`python ...`** entry).
2. Verify **`/ping`** from browser or **`curl`** / **HTTPX**.
3. **Commit** and **push**.
