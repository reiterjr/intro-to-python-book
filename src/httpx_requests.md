# HTTP client with HTTPX

**HTTPX** is a modern **HTTP client** (sync and async). Install it after you set up your **virtual environment** and dependencies (**[Virtual environments and packages](./virtual_environments_and_packages.md)**).

## GET request

```python
import httpx

def fetch_status(url: str) -> int:
    response = httpx.get(url, timeout=10.0)
    response.raise_for_status()
    return response.status_code
```

## Reading JSON

```python
data = response.json()
# data is usually a dict or list
```

## Error handling

```python
try:
    r = httpx.get("http://127.0.0.1:8000/ping", timeout=5.0)
    r.raise_for_status()
    print(r.json())
except httpx.HTTPError as exc:
    print("HTTP error:", exc)
```

## Implement

1. Start your **FastAPI** app locally (later chapter) **or** use **`https://httpbin.org/get`** for a smoke test.
2. Write a script **`probe.py`** that performs a **GET**, prints **status code** and **JSON** (if any).
3. **Commit** and **push**.
