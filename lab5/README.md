# Lab 5 — Agents

**Duration:** ~1 hour  
**Prerequisites:** Python 3.10+, VS Code with GitHub Copilot, [HVE Core](https://marketplace.visualstudio.com/items?itemName=ise-hve-essentials.hve-core) extension

---

## What You'll Practice

| Part | Skill | Time | Copilot Feature |
|------|-------|------|-----------------|
| **1** | Using built-in Copilot agents | 10 min | `@workspace`, `@terminal`, `@vscode` |
| **2** | HVE Core agents | 15 min | `task-researcher`, `rpi-agent` |
| **3** | Custom chat participants | 15 min | Chat participant API |
| **4** | Agent-assisted feature development | 15 min | Agents + Chat combined |
| **5** | Agent comparison | 5 min | Side-by-side evaluation |

---

## Setup

```bash
cd lab5
pip install pytest
```

Install [HVE Core](https://marketplace.visualstudio.com/items?itemName=ise-hve-essentials.hve-core) from the VS Code Marketplace if you haven't already.

---

## The Scenario

You have `ecommerce.py` — a multi-model e-commerce backend with customers, carts, orders, and an order service. You'll use various Copilot agents to research, plan, and implement new features.

---

## Part 1 — Built-in Copilot Agents (10 min)

VS Code has built-in agents (chat participants) you can invoke with `@`:

### Your tasks

1. **`@workspace`** — Ask about the full codebase:
   ```
   @workspace What classes and services are in ecommerce.py? Draw a dependency diagram.
   ```

2. **`@workspace`** — Cross-file questions:
   ```
   @workspace How does OrderService handle order cancellation? What happens to the status?
   ```

3. **`@terminal`** — Get CLI help:
   ```
   @terminal How do I run pytest with verbose output and stop on first failure?
   ```

4. **`@vscode`** — Editor features:
   ```
   @vscode How do I set up a Python debugger launch config for this project?
   ```

### Takeaway

Each agent has a specific scope: `@workspace` knows your code, `@terminal` knows shell commands, `@vscode` knows editor features.

---

## Part 2 — HVE Core Agents (15 min)

HVE Core ships with 49 specialized agents. Let's try the most useful ones.

### Your tasks

1. **`task-researcher`** — Research before coding:
   Open Chat → select `task-researcher` from the agent picker → prompt:
   ```
   Research the ecommerce.py codebase. What patterns does it use?
   What are the extension points? Where would I add a discount/coupon system?
   ```
   Notice: it researches first, then gives a structured answer.

2. **`rpi-agent`** — Research → Plan → Implement:
   Select `rpi-agent` → prompt:
   ```
   Add a coupon/discount system to the e-commerce backend.
   Support: percentage discounts, fixed amount discounts, and minimum order thresholds.
   ```
   Watch the three-phase workflow:
   - **Research:** reads the codebase
   - **Plan:** proposes the design (new classes, modified methods)
   - **Implement:** writes the code

3. **`memory`** — Persistent context:
   Select `memory` → prompt:
   ```
   Remember that this project uses dataclasses for models and the OrderService
   pattern for business logic. All money values are floats rounded to 2 decimals.
   ```
   Then start a new chat and ask it to add a feature — it should recall the conventions.

<details>
<summary>💡 Hints</summary>

- HVE agents appear in the agent picker dropdown in Copilot Chat
- The RPI workflow is three explicit phases — let each phase complete before the next
- The memory agent persists across chat sessions

</details>

---

## Part 3 — Custom Chat Participants (15 min)

You can create your own agents using prompt files.

### Your tasks

1. **Create an e-commerce domain expert:** Create `.github/copilot/prompts/ecommerce-expert.prompt.md`:
   ```markdown
   ---
   description: E-commerce domain expert for this project
   ---
   You are an e-commerce domain expert working on this codebase.

   When answering questions or generating code:
   - Follow the existing patterns (dataclasses for models, Service classes for logic)
   - All monetary values are floats rounded to 2 decimals
   - Use Enum classes for fixed option sets
   - Include shipping and tax calculations where relevant
   - Reference existing classes (Customer, CartItem, EcomOrder, OrderService)
   ```

2. **Test your agent:** Use it to generate:
   - A `ReturnService` for handling product returns
   - An `InventoryService` for stock management
   - A `CustomerLoyaltyService` for points/rewards

3. **Create a test expert:** Create `.github/copilot/prompts/test-expert.prompt.md` that generates tests following your conventions.

---

## Part 4 — Agent-Assisted Feature Development (15 min)

Combine agents for a real feature workflow.

### Your task: Add a complete wishlist feature

1. **Research** (use `task-researcher`):
   ```
   How should a wishlist feature integrate with the existing Customer and CartItem models?
   ```

2. **Plan** (use `rpi-agent`):
   ```
   Plan a Wishlist feature: customers can save items, move items to cart, share wishlists.
   ```

3. **Implement** (use your custom e-commerce expert prompt):
   ```
   Implement the Wishlist class and WishlistService following the plan from the previous step.
   ```

4. **Test** (use your test expert prompt):
   ```
   Write comprehensive tests for the Wishlist and WishlistService classes.
   ```

---

## Part 5 — Agent Comparison (5 min)

### Your task

Ask the **same question** to three different agents and compare:

**Question:** *"Add an email notification system that sends order confirmation, shipping updates, and delivery confirmation."*

Try with:
1. Regular Copilot Chat (no agent)
2. `rpi-agent` from HVE Core
3. Your custom e-commerce expert prompt

Compare: depth of research, quality of the plan, and how well the code follows existing patterns.

---

## Lab Complete!

- ✅ Used built-in agents (`@workspace`, `@terminal`, `@vscode`)
- ✅ Used HVE Core agents (`task-researcher`, `rpi-agent`, `memory`)
- ✅ Created custom chat participants via prompt files
- ✅ Combined agents for a full feature development workflow
- ✅ Compared agent outputs for quality and context-awareness
