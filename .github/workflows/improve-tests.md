---
name: improve-tests
on:
  workflow_dispatch:
permissions:
  contents: read
---

## Improve Lab02 Test Coverage

Analyze the `lab02` test suite, identify coverage gaps in `order_processor.py`,
and add focused pytest cases to cover those paths.

## Scope

- Work only in `lab02`
- Install dependencies with `pip install -r requirements.txt`
- Run `pytest tests/ --cov=order_processor --cov-report=json --cov-report=term-missing -v`

## Coverage Analysis

- Read the coverage JSON report and list the uncovered lines in `order_processor.py`
- For each uncovered line, identify the smallest missing test case that would exercise it
- Prefer adding tests to existing files under `lab02/tests`

## Test Authoring Rules

- Use `unittest.mock.patch` for external calls
- Use in-memory SQLite for DB tests
- Use `AsyncMock` for async functions
- Use the shared `make_order` factory from `conftest.py`
- Preserve intentional MCP exercise gaps unless a change is necessary for the targeted coverage work

## Verification

- Re-run `pytest tests/ --cov=order_processor --cov-report=term-missing --cov-fail-under=80 -v`
- Summarize what changed, what coverage improved, and any remaining uncovered lines
