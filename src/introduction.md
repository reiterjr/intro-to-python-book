# Introduction

Welcome to **Intro to Python**. This book teaches Python **without** building terminal **text-mode UI** apps: we do **not** use **Textual**, **Rich**, or similar console UI libraries. Instead you will work toward **HTTP APIs** and a small **web server** using **FastAPI** and **Uvicorn**, with **Pydantic** for structured data and **HTTPX** for calling other services.

Your work lives in a **GitHub Classroom** repository created from a **base template** (boilerplate solution layout, dependency file, stubs). The book gives **theory**, **directions**, and **code samples**; you **implement** in that template and **push** for review.

> [!IMPORTANT]
> **GitHub Classroom**
>
> 1. Open **[Accept the GitHub Classroom assignment](https://classroom.github.com/a/f2frF2Rk)**.
> 2. Sign in to GitHub, **accept** the assignment, and wait for your **personal repository**.
> 3. Complete any **email / invitation** steps your course requires.
> 4. **Clone** your repo, create a **virtual environment** (see the next chapter), and open the project in your editor.

> [!NOTE]
> **One template, whole series**  
> Add files and features to the **same** Classroom project as you go. Do not assume a separate “chapter repo” per week unless your instructor says otherwise.

> [!TIP]
> **Submit after each milestone**  
> When you finish a chapter’s exercises, **commit** with a clear message and **push** to **`main`** (or the branch your instructor uses).

> [!WARNING]
> **Feedback pull request**  
> If your repo has a **Feedback** PR for instructor comments, **do not merge or close it**. Keep working on **`main`** and pushing; treat that PR as a discussion thread only.

## Capstone preview

The **final project** is a **Python web server** (FastAPI + Uvicorn) that exposes a custom route:

- **Path:** `GET /ping`
- **Response body (JSON):** `{"message": "pong"}`

Everything in the book builds toward that: types, functions, Pydantic, JSON, HTTP client usage, then serving HTTP yourself.

## What you need

- **Python 3.12+** (match your template’s `requires-python` if it specifies one).
- **Git** and a code editor.
- A terminal for **venv**, **pip** (or **uv** / **Poetry**, per template).

Next: **environment setup**.
