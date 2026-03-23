# Capstone: JSON `ping` web server

## Requirements

Deliver a **Python** service using **FastAPI** and **Uvicorn** that:

| Item | Requirement |
|------|-------------|
| **Framework** | **FastAPI** |
| **Server** | **Uvicorn** (CLI or `Server` in code) |
| **Route** | **`GET /ping`** |
| **Response** | **JSON** body exactly: **`{"message": "pong"}`** |
| **Status** | **200 OK** for that route |

Optional extras (only if assigned): **`GET /`** with a welcome JSON, **`POST /alive`** with a JSON body, health checks, or **HTTPX** integration tests.

## Example core

Your implementation can look like this:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/ping")
def ping():
    return {"message": "pong"}
```

Start with:

```bash
uvicorn your_module:app --host 0.0.0.0 --port 8000
```

Replace **`your_module`** with your actual module path.

## Checklist

- [ ] **`GET /ping`** returns **`Content-Type: application/json`** and body **`{"message":"pong"}`** (spacing may vary; keys/values must match).
- [ ] Dependencies listed in **`requirements.txt`** / **`pyproject.toml`** per template.
- [ ] **README** explains how to create venv, install, and run.
- [ ] **Committed** and **pushed** to **`main`** for final review.

Congratulations—you now have a minimal **JSON API** foundation without relying on terminal UI frameworks.
