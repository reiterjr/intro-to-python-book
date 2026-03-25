# JSON serialization with Pydantic

> [!IMPORTANT]
>
> **GitHub Classroom**
>
> - Accept the [GitHub Classroom assignment]() for this chapter.
> - Complete any **email / invitation** steps your course requires.
> - **Clone** your repo and open the project in your editor.

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

## Your Turn

> [!NOTE]
>
> Refer to your source code files for more details on what to do.

> [!IMPORTANT]
>
> Commit often and push your final solution when ready

- Create a model for a **`{"status": "ok", "code": 200}`**-shaped object.
- Serialize with **`model_dump_json()`**, parse back with **`model_validate_json`**.
- **Commit** and **push**.
