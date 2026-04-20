# Lab 2 — GitHub CLI + Copilot CLI for Testing Workflows

**Duration:** 30 minutes  
**Prerequisites:** Lab 1 completed (or use the solutions), `gh` CLI installed, GitHub Copilot CLI extension

---

## What You'll Practice

| Part | Skill | Time | CLI Feature |
|------|-------|------|-------------|
| **1** | Explain unfamiliar code | 5 min | `gh copilot explain` |
| **2** | Get command suggestions | 5 min | `gh copilot suggest` |
| **3** | Autonomous test writing | 10 min | `gh copilot task` |
| **4** | PR workflow with CI checks | 10 min | `gh pr create` + `gh run watch` |

---

## Setup

### Install the GitHub CLI (if not already installed)

```bash
# Windows (winget)
winget install GitHub.cli

# macOS
brew install gh

# Verify
gh --version
```

### Authenticate and install the Copilot extension

```bash
gh auth login
gh extension install github/gh-copilot
```

### Verify Copilot CLI works

```bash
gh copilot --help
```

You should see `explain`, `suggest`, and `task` subcommands.

---

## Part 1 — Explain Unfamiliar Code (5 min)

Imagine you've just joined a project and need to understand the test patterns before contributing.

### Your tasks

**1a.** Ask Copilot to explain the production code:

```bash
gh copilot explain "What does the process_order function do in lab1/order_processor.py?"
```

**1b.** Ask it to explain a testing pattern:

```bash
gh copilot explain "Why does this test use @patch('order_processor.requests.post') instead of @patch('requests.post')?"
```

**1c.** Ask it to explain the async mock pattern:

```bash
gh copilot explain "What does mock_session.post.return_value.__aenter__.return_value.status = 200 do?"
```

### Takeaway

`gh copilot explain` is great for onboarding — understanding unfamiliar code, library patterns, or test techniques without leaving the terminal.

---

## Part 2 — Get Command Suggestions (5 min)

You know *what* you want to do but not the exact command. Copilot suggests the right incantation.

### Your tasks

**2a.** Ask for the pytest command to run only failing tests:

```bash
gh copilot suggest "run only previously failed pytest tests"
```

**2b.** Ask how to check coverage for a single function:

```bash
gh copilot suggest "run pytest coverage only for the validate_order function"
```

**2c.** Ask for a git command:

```bash
gh copilot suggest "create a new branch called add-tests and switch to it"
```

**2d.** Ask for a gh command:

```bash
gh copilot suggest "create a draft pull request with title 'Add unit tests' from current branch"
```

### Takeaway

`gh copilot suggest` handles shell, git, and gh commands. It asks you to pick the command type, then generates the exact command. You can run it directly or copy-paste.

---

## Part 3 — Autonomous Test Writing with `task` (10 min)

This is the CLI equivalent of Agent Mode in VS Code. You give Copilot a goal and it figures out the steps.

### Your tasks

**3a.** First, check your current coverage:

```bash
cd lab1
pytest tests/ --cov=order_processor --cov-report=term-missing -v
```

Note the uncovered lines.

**3b.** Ask Copilot to fill the gaps:

```bash
gh copilot task "Look at the coverage report for lab1/order_processor.py. The uncovered functions are load_order() and charge_customer() called directly. Write pytest tests in lab1/tests/test_coverage_gaps.py to cover them. Use in-memory SQLite for DB tests and unittest.mock.patch for HTTP tests."
```

**3c.** Verify the new tests pass:

```bash
pytest tests/ --cov=order_processor --cov-report=term-missing -v
```

**3d.** If coverage is still below 80%, iterate:

```bash
gh copilot task "Coverage is still at X%. Look at the remaining uncovered lines in order_processor.py and add tests to reach 80%."
```

### Takeaway

`gh copilot task` is powerful for well-scoped, specific tasks. The more context you give it (file paths, function names, what you've tried), the better the result.

---

## Part 4 — PR Workflow with CI Checks (10 min)

Now let's use the full GitHub CLI workflow to submit your tests as a PR and watch CI run.

### Your tasks

**4a.** Create a branch and commit your work:

```bash
git checkout -b add-unit-tests
git add lab1/tests/
git commit -m "Add unit tests for order processor"
```

**4b.** Push and create a PR:

```bash
git push -u origin add-unit-tests
gh pr create --title "Add unit tests for order processor" --body "Covers all 5 process_order paths, validation parametrize, and async notify tests."
```

**4c.** Watch the CI workflow run:

```bash
gh run watch
```

This streams the GitHub Actions output live in your terminal. You'll see the `pytest --cov-fail-under=80` gate pass (or fail!).

**4d.** If CI fails, check the logs:

```bash
gh run view --log-failed
```

Fix the issue, push again, and the PR checks re-run automatically.

**4e.** Check PR status:

```bash
gh pr status
gh pr checks
```

### Takeaway

The full loop — write tests → commit → PR → CI → fix → merge — all from the terminal, never opening a browser.

---

## Bonus: Useful `gh` Commands for Testing Workflows

```bash
# View test results from the last CI run
gh run view --log

# Re-run a failed workflow
gh run rerun <run-id> --failed

# List recent workflow runs
gh run list --limit 5

# View PR diff (to review what tests were added)
gh pr diff

# Merge PR after checks pass
gh pr merge --squash --delete-branch
```

---

## Lab Complete!

You practiced the **terminal-first** workflow with GitHub CLI:

- ✅ `gh copilot explain` — understand unfamiliar code and patterns
- ✅ `gh copilot suggest` — get exact shell/git/gh commands
- ✅ `gh copilot task` — autonomous test generation from the terminal
- ✅ `gh pr create` + `gh run watch` — full PR workflow without leaving the terminal
- ✅ `gh run view --log-failed` — debug CI failures from the CLI
