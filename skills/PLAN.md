# $PLAN — Plan & Construct Mode

**Purpose:** Enter a deliberate planning mode that produces a senior/lead-developer-grade execution plan before any code is written.

**Scope:** one-shot (applies only to the prompt it is called in).

## Lazarus Exclusion (MANDATORY)

- This skill is **only** for creating planning artifacts in the **respective project** being worked on (the app/project under development), never inside the Lazarus project itself.
- **Never** read from, write to, create files in, or otherwise act inside the **Lazarus folder**. Do not place any `PLAN/`, `.laz/`, `.md`, or `.canvas` output anywhere under the Lazarus project directory.
- All output goes to `.laz/PLAN/` in the **current project folder** (i.e. the inspected/target project), which must not be the Lazarus folder. If the current workspace *is* the Lazarus folder, stop and tell the user that `$PLAN` does not operate inside Lazarus — it requires a separate project target.

## Invocation

`$PLAN:"NAME OR PATH"`

- Argument rules mirror commands (see [[commands.md]]): `NAME OR PATH`, bare `$PLAN` → current workspace, quotes optional unless spaces.
- The target, if given, scopes what the plan is about (a file, folder, or feature area). Bare form plans the whole prompt's intent.

## Behavior

This is a **plan and construct** mode — think first, build later. Do not write implementation code unless the user explicitly moves past planning.

### 1. Prepare the PLAN folder

- Check `.laz/PLAN/` in the current project folder.
- **Only if it does not already exist**, create it.
- Never wipe or recreate an existing `PLAN/` folder.

### 2. Analyze the prompt (lead-dev + senior-engineer depth)

Reason at a high standard:

- Restate the core goal in one sentence (per [[main.md]] §1) — the thing that must not be lost.
- Decompose into concrete steps/milestones; identify files, modules, and systems affected.
- Surface tradeoffs, risks, dependencies, sequencing, and rollback/abort points.
- Flag ambiguities and the decisions made to resolve them.
- Call out testing/verification and how "done" is proven.
- Note where existing patterns/conventions in the codebase should be followed.

### 3. Document the plan

Inside `.laz/PLAN/`, create a **one-word-named folder** (e.g. `auth/`, `sync/`, `ui/`) whose name summarizes the plan. Inside it, create:

- **`<oneword>.md`** — the written plan. Sections:
  - `## Goal` — one-line restated intent.
  - `## Approach` — step-by-step execution, ordered.
  - `## Files affected` — table of file → change.
  - `## Tradeoffs & risks` — what could go wrong and mitigations.
  - `## Verification` — how to confirm success (tests, manual checks).
- **`<oneword>.canvas`** — a visualization of the plan (readable, well-spaced; see layout rules below).

Use `[[wikilinks]]` from the `.md` and `.canvas` back to [[main.md]] and [[commands.md]] so the graph stays connected.

### 4. Canvas layout rules (good-looking & readable)

- Group related steps into **cards**; one concern per card. Keep card text concise — a card should hold a single idea, not a wall of prose.
- If a step genuinely needs more text, keep **all of it readable**: give that card a generous `width`/`height`, size the canvas so cards don't overlap, and increase spacing between nodes (≥140px vertically, columns ≥240px apart).
- Use a **title card** at the top describing the plan in one line.
- Color-code by phase/type (e.g. analyze = one palette index, implement = another, verify = another) for quick scanning.
- Connect cards with **edges** showing real execution order (top→bottom or left→right). Cross-links for shared dependencies.
- Prefer short labels on cards and put detail in the linked `.md`.

### 5. Output to user

Summarize the plan briefly (approach + key files + risks) and state that the full plan is saved under `.laz/PLAN/<oneword>/`. Do not begin implementation unless the user asks.

Back to [[main.md]]
