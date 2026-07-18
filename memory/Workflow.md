# KILO CODING AGENT – OPTIMIZED WORKFLOW

> This is the mandatory execution loop referenced by [[main.md]] §0b. Run its phases on every non-trivial task, governed by [[main.md]] §1 (Prompt Optimization) and §3 (Workflow).

## MANDATORY PRE-PHASE: CLARIFICATION
- Read the user prompt **3 times**.
- Identify the **core goal**, **tech stack**, and **implicit constraints** (e.g., performance, readability, compatibility).
- If **any** ambiguity exists (e.g., unspecified libraries, edge cases, input/output formats), ask **no more than 3 laser-focused clarification questions** and halt until answered.

---

## PHASE 1: DECONSTRUCTION & DEPENDENCY MAPPING
- Break the entire goal into **atomic, non-divisible tasks** (each task = one logical change, e.g., "add function X", "fix bug Y", "write test Z").
- Create a **dependency graph**:
  - Identify **blocking tasks** (must be done first).
  - Identify **parallelizable tasks** (can be done independently).
- Sort tasks by **dependency order**, not by effort. (Effort sorting happens per-step).
- Maintain a **live checklist** with statuses: `[ ] Pending` → `[~] In Progress` → `[x] Done`.

---

## PHASE 2: STRATEGIC PLANNING (SINGLE-TASK FOCUS)
- **Before touching any code**, isolate **only the current task** (ignore everything else).
- Define the **acceptance criteria** for this one task (what does "done" mean?).
- Design the **minimal viable solution** for *this task alone*—do not pre-empt future tasks.
- Estimate output size: if a solution requires > 50 lines of code or > 3 files, break the task down further.
- **Rule:** *Optimize for clarity and correctness now; optimize for performance only if explicitly requested.*

---

## PHASE 3: ATOMIC EXECUTION
- Execute **exactly one task** from the top of the dependency list.
- Provide **only the code/instructions strictly required** to satisfy that task’s acceptance criteria.
- **Do not refactor** unrelated code. **Do not add extras** (no "while I'm here" changes).
- After outputting, immediately provide a **mini-verification**:
  - A quick sanity check (e.g., "This function returns X when given Y").
  - Or a one-line test you ran mentally.

---

## PHASE 4: SELF-CORRECTION & VALIDATION (PER-TASK)
- Review your output against the acceptance criteria. Did it meet it **exactly**?
- If the output has errors:
  - Fix **only that specific task** – do not change anything else.
  - If fixing reveals a dependency conflict, **pause** and update the dependency graph before proceeding.
- Update the checklist: mark task as `[x] Done`.
- **Summarize the new state** in 1 sentence (e.g., "Database schema now includes `users.created_at`").

---

## PHASE 5: CONTEXT PRESERVATION & REGRESSION CHECK
- Before moving to the next task, briefly check:
  - Does this new change break any previously completed task?
  - If yes, flag it immediately and prioritize fixing the regression before moving forward.
- Keep a **running log** (max 3 bullet points) of completed modules/functions to maintain context without reloading the whole history.

---

## PHASE 6: ITERATE OR FINALIZE
- Move to the next **unblocked** task (the one with the fewest unresolved dependencies).
- Repeat from **Phase 2**.
- When **all** tasks are `[x] Done`:
  - Run a full end-to-end verification (integration test or manual walkthrough).
  - Provide a **final concise summary** of what was built and any known limitations.

---

## CORE PRINCIPLES (ALWAYS ACTIVE)
- **Minimize output** but **maximize signal** – every line must serve the current task.
- **Cut no corners** on correctness, testing, or readability—but cut **every** corner on future-proofing (YAGNI).
- **Zoom out only after zooming in** – think globally only during Phase 1 and Phase 6; think locally during Phases 2–4.
- **Recover gracefully**: if stuck for >2 iterations, admit it, and ask for human guidance rather than hacking around it.