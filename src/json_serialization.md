# JSON serialization with Pydantic

> [!IMPORTANT]
>
> **GitHub Classroom**
>
> - Accept the [GitHub Classroom assignment]() for this chapter.
> - Complete any **email / invitation** steps your course requires.
> - **Clone** your repo and open the project in your editor.

## What JSON is

**JSON** (JavaScript Object Notation) is a **text** format for structured data. A JSON document is built from a few value types:

- **Objects** — unordered mappings from **string keys** to values, written with `{` `}` and key/value pairs.
- **Arrays** — ordered lists of values, written with `[` `]`.
- **Scalars** — **strings** (in double quotes), **numbers**, `true`, `false`, and `null` (JSON’s “no value here” sentinel).

Despite the name, JSON is **not tied to JavaScript** today: it is a **language-neutral** interchange format. Many runtimes and tools read and write it the same way, which is why it dominates **HTTP APIs**, configs, and logging.

JSON is usually **UTF-8** text. You can open it in an editor, **diff** it in Git, and debug network traffic without a binary decoder—useful for teaching and for production support.

### Purpose and typical use cases

JSON sits in the middle of systems that do not share memory: they agree on **fields**, **types**, and **nesting** as text.

Common situations:

- **Web APIs** — clients and servers send **request and response bodies** as JSON (often with `Content-Type: application/json`).
- **Config and tooling** — project metadata (`package.json`, editor settings, CI YAML-adjacent configs in JSON form) and CLI tools that emit machine-readable output.
- **Mobile and desktop apps** — same payloads as browsers when talking to the same backend.
- **Databases and search** — systems that store **JSON documents** or return aggregate results as JSON for dashboards.

In Python, JSON maps naturally to `dict` (object), `list` (array), `str`, `int` / `float`, `bool`, and `None` (for JSON `null`) via the standard library’s `json` module. That mapping is convenient but **untyped**: every value arrives as generic dict/list primitives until you **validate** and narrow types.

### Why pair JSON with Pydantic

At the **boundary** of your program (HTTP body, file, environment-driven config), data is “foreign”: keys might be missing, types wrong, or strings not parseable as numbers. **Pydantic models** turn that text into **explicit fields** with types and rules, and **fail with structured errors** when the payload does not match—before your business logic runs on bad data.

The sections below show **serialization** (model → JSON text) and **parsing** (JSON text → model). That round trip is the backbone of JSON APIs and of tests that lock the shape of your payloads.

## Model → JSON

```python
from pydantic import BaseModel

class PingBody(BaseModel):
    message: str

body = PingBody(message="pong")
json_text: str = body.model_dump_json()
json_bytes: bytes = body.model_dump_json().encode("utf-8")
```

`model_dump_json()` produces a **string** of JSON. Encoding with **`utf-8`** gives **bytes**, as many HTTP stacks expect for wire data.

In **FastAPI**, returning a `dict` or a **model** often becomes JSON automatically; under the hood similar serialization runs.

## JSON → model

```python
raw = '{"message": "pong"}'
obj = PingBody.model_validate_json(raw)
assert obj.message == "pong"
```

`model_validate` accepts a `dict` (or other mapping-like input, depending on settings). `model_validate_json` accepts a `str` of JSON and parses it first—one step when the body arrives as raw text from a socket or file.

## Testing JSON-shaped data

Treat JSON as **input and output** you must respect in tests.

**Round trip.** Build a model, serialize, parse again, then assert fields match. That guards your schema and defaults without hitting the network:

```python
def test_ping_body_round_trip_json():
    original = PingBody(message="pong")
    text = original.model_dump_json()
    loaded = PingBody.model_validate_json(text)
    assert loaded.message == original.message
```

**Invalid JSON strings.** Malformed text never becomes a model: in Pydantic v2 you usually get a `ValidationError` whose `errors()` list describes a JSON parse problem rather than a field mismatch. Exercise:

```python
import pytest
from pydantic import ValidationError

def test_ping_body_rejects_invalid_json():
    with pytest.raises(ValidationError):
        PingBody.model_validate_json("{not valid json")
```

**Validation, not just parsing.** Valid JSON with wrong **shape or types** should still raise `ValidationError`—same idea as in the Pydantic chapter when `model_validate` fails:

```python
import pytest
from pydantic import ValidationError

def test_ping_body_wrong_type_in_json():
    raw = '{"message": 123}'  # JSON allows this; your model expects a string
    with pytest.raises(ValidationError):
        PingBody.model_validate_json(raw)
```

**Snapshot-style checks.** Sometimes you assert the **exact serialized string** (ordering, spacing). Pydantic’s JSON output is generally stable for a given model instance, but minor version differences can exist; asserting on `model_dump()` or on parsed fields is often more robust than brittle string equality.

**API tests later.** When you reach FastAPI, you will often combine **HTTPX** `client.get` / `post` with `response.json()` and then `model_validate` on the result—same validation pattern, one layer higher.

## Your Turn

> [!NOTE]
>
> Refer to your source code files for more details on what to do.

> [!IMPORTANT]
>
> Commit often and push your final solution when ready

- Create a model for a `{"status": "ok", "code": 200}`-shaped object.
- Serialize with `model_dump_json()`, parse back with `model_validate_json`.
- **Commit** and **push**.
