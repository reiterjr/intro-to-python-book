# JSON serialization with Pydantic

Web APIs speak **JSON**. Pydantic models convert cleanly to and from JSON **text** and **bytes**.

## Model → JSON

```python
from pydantic import BaseModel

class PingBody(BaseModel):
    message: str

body = PingBody(message="pong")
json_text: str = body.model_dump_json()
json_bytes: bytes = body.model_dump_json().encode("utf-8")
```

In **FastAPI**, returning a **`dict`** or a **model** often becomes JSON automatically; under the hood similar serialization runs.

## JSON → model

```python
raw = '{"message": "pong"}'
obj = PingBody.model_validate_json(raw)
assert obj.message == "pong"
```

**`model_validate`** accepts a **dict**; **`model_validate_json`** accepts a **string**.

## Implement

1. Create a model for a **`{"status": "ok", "code": 200}`**-shaped object.
2. Serialize with **`model_dump_json()`**, parse back with **`model_validate_json`**.
3. **Commit** and **push**.
