# Lab 3 — Refactoring & Migration

**Duration:** ~1 hour  
**Prerequisites:** Python 3.10+, VS Code with GitHub Copilot

---

## What You'll Practice

| Part | Skill | Time | Copilot Feature |
|------|-------|------|-----------------|
| **1** | Identify code smells | 10 min | Chat `/explain` + review |
| **2** | Extract repeated patterns | 15 min | Chat + inline suggestions |
| **3** | Add data validation (Pydantic) | 15 min | Chat with explicit prompt |
| **4** | Migrate Flask → FastAPI | 15 min | Agent Mode |
| **5** | Verify migration with tests | 5 min | Chat `/tests` |

---

## Setup

```bash
cd lab3
pip install -r requirements.txt
```

---

## The Scenario

You have `task_api.py` — a Flask REST API for task management. It works, but it's full of **code smells**:

- **Repeated DB connection logic** in every route (no connection management)
- **Repeated row→dict mapping** copy-pasted across routes
- **No input validation** beyond checking if title exists
- **No error handling** for DB failures
- **Raw SQL everywhere** — no ORM or query builder

Your job: progressively refactor it using Copilot.

---

## Part 1 — Identify Code Smells (10 min)

### Your tasks

1. Select the entire file → Chat → *"Review this code and list all code smells, anti-patterns, and areas for improvement"*
2. Copilot should identify at least:
   - Repeated `sqlite3.connect()` / `conn.close()` in every route
   - Repeated row-to-dict mapping logic
   - No input validation
   - No error handling
   - Magic numbers (column indices `row[0]`, `row[1]`, etc.)
3. Ask: *"Prioritize these issues — what should I fix first?"*

---

## Part 2 — Extract Repeated Patterns (15 min)

### Your tasks

1. **DB connection context manager:** Ask Chat: *"Create a context manager or helper function to manage database connections, replacing the repeated connect/close pattern"*

2. **Row mapping helper:** Ask Chat: *"Extract the row-to-dict mapping into a reusable function"*

3. **Apply to all routes:** Use inline Copilot to refactor each route to use the new helpers

<details>
<summary>💡 Hints</summary>

- A context manager with `yield` works great for DB connections
- The row mapper can use column names from `cursor.description`
- Let Copilot inline suggestions do the repetitive refactoring

</details>

---

## Part 3 — Add Data Validation with Pydantic (15 min)

### Your tasks

1. Ask Chat: *"Add Pydantic models for task creation and update requests with proper validation"*
2. Add validation rules:
   - `title`: required, 1-200 characters
   - `status`: must be one of `pending`, `in_progress`, `completed`
   - `priority`: integer 0-5
3. Apply the models to the POST and PUT routes

```bash
pip install pydantic
```

<details>
<summary>💡 Hints</summary>

- Use `pydantic.BaseModel` with `Field()` for constraints
- For status, use `Literal["pending", "in_progress", "completed"]`
- Flask + Pydantic: validate in the route with `TaskCreate(**data)`

</details>

---

## Part 4 — Migrate Flask → FastAPI (15 min)

Now for the big one: use **Agent Mode** to migrate the entire API from Flask to FastAPI.

### Your tasks

1. Open Agent Mode in VS Code
2. Prompt: *"Migrate task_api.py from Flask to FastAPI. Keep the same routes and behavior. Use Pydantic models for request/response. Add proper async support and dependency injection for the DB connection."*
3. Agent Mode should:
   - Create a new `task_api_fastapi.py` (or modify in place)
   - Convert `@app.route` to `@app.get`, `@app.post`, etc.
   - Use `Depends()` for DB connection injection
   - Add Pydantic response models
4. Install FastAPI: `pip install fastapi uvicorn`

<details>
<summary>💡 Hints</summary>

- FastAPI uses `@app.get("/tasks")` instead of `@app.route("/tasks", methods=["GET"])`
- `request.get_json()` becomes a Pydantic model parameter
- `Depends()` is the FastAPI pattern for dependency injection (replaces the connection helper)

</details>

---

## Part 5 — Verify with Tests (5 min)

### Your tasks

1. Ask Chat: *"Write pytest tests for the FastAPI task API using TestClient"*
2. Run: `pytest tests/ -v`
3. Ensure all CRUD operations still work

---

## Solutions

- `solutions/task_api_refactored.py` — Parts 1-3 (refactored Flask)
- `solutions/task_api_fastapi.py` — Part 4 (migrated to FastAPI)

---

## Lab Complete!

- ✅ Identified code smells with Copilot Chat review
- ✅ Extracted repeated patterns into reusable helpers
- ✅ Added Pydantic validation models
- ✅ Migrated Flask → FastAPI using Agent Mode
- ✅ Verified migration with tests
