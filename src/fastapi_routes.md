# FastAPI: apps and routes

**FastAPI** maps **URL paths** and **HTTP methods** to Python functions. JSON **dict** return values become **JSON responses**.

## Minimal app

A typical minimal FastAPI module looks like this:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def root():
    return {"hello": "world"}

@app.get("/ping")
def ping():
    return {"message": "pong"}

@app.post("/alive")
def alive():
    return {"message": "you're alive!"}
```

- **`@app.get("/path")`** registers a **GET** route.
- **`@app.post(...)`** registers **POST**.
- Return a **`dict`** → JSON object; return a **list** → JSON array.

> [!NOTE]
> Typos in keys (e.g. **`mesage`** vs **`message`**) become typos in JSON—clients and tests should match the **exact** spelling you intend.

## Path vs query parameters (preview)

```python
@app.get("/items/{item_id}")
def read_item(item_id: int, q: str | None = None):
    return {"item_id": item_id, "q": q}
```

FastAPI **parses** and **validates** types for you.

## Implement

1. Complete **[Virtual environments and packages](./virtual_environments_and_packages.md)** so **FastAPI** is installed in your venv.
2. Create **`app.py`** (or the filename your template uses) with **`app = FastAPI()`**.
3. Add **`GET /`** returning any small JSON dict.
4. Add **`GET /ping`** returning **`{"message": "pong"}`** (your **capstone** route—keep it working from here on).
5. **Commit** and **push** (application code only—not personal test files).
