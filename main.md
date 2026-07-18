# Coding Agent — Operating Instructions

> Reference this file at the start of every chat with the coding agent (e.g. "Read `main.md` before starting."). It defines how you work in this project, not what to build.

## 0. Initialization

When this file is first loaded for a project, ensure a `.laz` folder exists in the current project folder. If it does not exist, create it.

---

## 0b. Execution Workflow (mandatory ordering)

[[memory/Workflow.md]] defines the mandatory step-by-step execution loop (clarification → deconstruction → planning → atomic execution → self-correction → regression check → iterate/finalize). It is the **operative procedure** for actually carrying out a task and must be taken into account on every prompt execution.

How it relates to the rest of this file:
- **§1 Prompt Optimization** is the *pre-pass* that clarifies and rewrites the prompt; it feeds into [[memory/Workflow.md]] Phase 1 (Deconstruction).
- **§3 Workflow (Think and Plan)** is the architectural-level guidance (plan-first, no spaghetti, design for scale); [[memory/Workflow.md]] is the finer-grained, per-task execution loop that implements it.
- When executing any non-trivial task, run [[memory/Workflow.md]]'s phases, applying §1 and §3 as governing constraints.

---

## 1. Prompt Optimization Protocol

Before executing any task, silently optimize the prompt first:

1. Restate the core task/goal in one sentence to yourself — this is the thing that must not be lost.
2. Identify ambiguity, missing constraints, or inefficient phrasing in the original prompt.
3. Rewrite the prompt internally to be clearer, more specific, and more efficient — without adding scope the user didn't ask for and without dropping any part of the original intent.
4. Execute against the optimized version.
5. If the optimization changes what will be built in any meaningful way (not just clarity), show the user the optimized prompt in one line before proceeding. If it's just clarity/structure, no need to show it — just execute.

Never let "optimizing" become "reinterpreting." The core task is fixed; only the path to it is optimized.

---

## 2. Long-Term Memory (Errors & Solutions)

Maintain a memory folder: `memory/errors/`

**Structure:**
```
memory/
  errors/
    README.md          <- index/instructions (this protocol)
    <topic-or-area>.md <- one file per recurring area (e.g. build.md, api.md, deploy.md)
```

**Each logged entry follows this format:**
```md
### [ERROR] short descriptive title
**Context:** where/when this happens
**Symptom:** exact error message or behavior
**Root cause:** why it happens
**Solution:** what fixed it
**Tags:** keywords for matching
```

**Protocol:**
- Whenever you hit an error, **first search `memory/errors/` for a matching symptom or tag** before attempting any fix.
- If a match is found, apply that known solution first. Only fall back to fresh debugging if the known solution doesn't apply or fails.
- Whenever an error is resolved (whether from memory or newly solved), log it immediately in the relevant file under `memory/errors/` — don't wait to be asked.
- Do not log trivial/one-off typos. Log anything that cost real debugging time or could recur.

---

## 3. Workflow: Think and Plan Before Acting

You are acting as a senior software architect, not a code generator. For any non-trivial task:

1. **Plan first.** Before writing code, lay out: the approach, the files/modules affected, and any tradeoffs. Present this plan briefly before implementing — don't ask permission for small tasks, but do surface the plan for anything architectural.
2. **No spaghetti code.** Favor clear structure, single-responsibility functions/modules, and naming that explains intent. If a quick hack is tempting, name it as a hack and note the proper fix instead of silently shipping it.
3. **Design for scale**, not just for the immediate ask — but don't over-engineer for hypothetical future requirements that weren't asked for. Practical scalability, not speculative abstraction.
4. **Efficient writing.** Prefer the smallest amount of clear code that solves the problem correctly. Don't pad with unnecessary boilerplate or premature abstraction layers.

---

## 4. Custom Commands (highest priority in any prompt)

Commands are defined in **[[commands.md]]** as a table — that file is the single source of truth for what each command does. **Parse for `!`-prefixed commands before anything else**, including before skills (`$`) and before the main prompt body. Commands outrank the rest of the prompt (rules R1–R6 in [[commands.md]]).

Mandatory command behavior when reading a prompt:
1. **Scan for `!`-prefixed commands first.** Honor them before any other instruction (R4). A `!`-prefixed folder is always off-limits (§4b).
2. **Track scope across the whole session.** Any scoping command (`!laz`, `!only`, `!ignore`) persists until cleared by `!q` (R5) and is a hard boundary (R6) — keep it in working memory across every later prompt, even if many go by without restating it (R4b).
3. **Don't shadow scope.** If a later prompt seems to ask for something outside the active scope, flag the conflict and do not act outside it (R6).

The live command list (see [[commands.md]] for exact syntax/behavior): `!ignore`, `!only`, `!laz`, `!NREP`, `!q`, `!List`, `!F`. Add or change commands only in [[commands.md]]; this file always defers to it.

---

## 4b. `!`-Prefixed Folders Are Off-Limits

Any folder whose name starts with `!` (e.g. `!BACKUP/`) is a **quarantine/sandbox folder** and must be **completely ignored** for the entire task:

- Do not read, reference, search, or list files inside it.
- Do not modify, copy, move, or sync anything from/to it.
- Do not treat its contents as part of the codebase or use them to inform changes elsewhere.
- This applies even if the user's prompt is ambiguous — the `!` prefix alone is the signal. No per-file `!ignore` command is needed for these folders.

This is separate from the `!ignore:"PATH"` command (see §4): that command ignores a specific path for one task on request, whereas `!`-prefixed folders are ignored automatically, always, without being named.

---

## 4c. Skills (`$`-prefixed, on-demand workflows)

Skills are domain-specific workflows defined under `skills/` and indexed in **[[skills/README.md]]**. They are invoked with a `$` prefix (e.g. `$DUCK`, `$PLAN`, `$DESIGN`) — **not** commands. When a prompt contains a `$`-prefixed token that matches a skill, load that skill's file and follow its workflow for that prompt. Skills are separate from `!` commands: parse `!` commands first (§4), then check for `$` skill invocations.

Live skills (see [[skills/README.md]] for exact invocation and scope): `$DUCK` (code review scout), `$PLAN` (plan & construct), `$DESIGN` (design snapshot). `$`-skills output to `.laz/` per their definitions.

---

## 4d. Version Control Exclusions for `!NREP` / `!UP`

The `!NREP` and `!UP` commands push to git. The following are **never** committed by those commands:

- The `.obsidian/` folder in the Lazarus project — **completely ignored**. Never read, reference, commit, or push any file inside it.
- The Obsidian config files `app.json`, `appearance.json`, `core-plugins.json`, `graph.json`, and `workspace.json` (typically found inside `.obsidian/`). These must never be staged or pushed.

Treat these as excluded from every `git add`/`git push` performed by `!NREP` or `!UP`. If they are already tracked, do not modify that state unless asked — but never add them anew.

---

## 5. Visual.canvas — Vault Map & canvas rules

`Visual.canvas` is a visual map of this Obsidian vault: what exists, where it lives, and how pieces connect. The rules for creating or editing any `*.canvas` file (layout, spacing, colors, edges, validation) live in **[[memory/canvas-rules.md]]** — follow them on every canvas change.

- Keep `Visual.canvas` updated whenever new major files/folders are added to the vault.
- Group by function (e.g. planning, code areas, memory, commands) with cards linking to the actual files.
- This is a navigation aid, not documentation — keep card text short, let links do the work.
- Before saving any `*.canvas`, run the [[memory/canvas-rules.md]] validation checklist (no overlaps, no edges over cards, valid JSON, no orphans).

## 6. Linking Everything Together

Use `[[wikilinks]]` between `main.md`, `commands.md`, `skills/README.md`, `memory/errors/*.md`, `memory/Workflow.md`, `memory/canvas-rules.md`, `Visual.canvas`, and any topic/docs files so the Obsidian graph view reflects real relationships, not just folder proximity.

Minimum linking rules:
- `main.md` links to [[commands.md]], [[skills/README.md]], [[memory/errors/README.md]], [[memory/Workflow.md]], [[memory/canvas-rules.md]], and [[Visual.canvas]].
- Every file in `memory/errors/` links back to `main.md`.
- `Visual.canvas` contains a card per top-level file/folder, linked to the real file.
- New docs you create should link back to whichever of the above they extend, so nothing becomes an orphan node in the graph.

---

## 7. Startup Checklist (run this when `main.md` is first read)

To bring a new project under Lazarus, follow §7 in [[README.md]] (new-user bootstrap). After bootstrap, the standing operating loop applies:

4. **§0** — ensure `.laz/` exists in the project folder; create it if missing.
5. **§4** — scan the prompt for `!`-commands; honor them first and track scope session-long (R4b/R5/R6).
6. **§4c** — if a `$`-skill is invoked, load it from [[skills/README.md]] and follow it.
7. **§0b / §1 / §3** — optimize the prompt (§1), then run the [[memory/Workflow.md]] loop on every non-trivial task, governed by the plan-first guidance in §3.
8. **§2** — before fixing any error, search `memory/errors/`; log new solutions there.
9. **§5 / canvas-rules** — when touching any `*.canvas`, follow [[memory/canvas-rules.md]] and update `Visual.canvas`.
10. **§6** — keep wikilinks intact so the vault graph has no orphans.

---

## Related

- [[commands.md]] — custom command definitions (highest priority in prompts; parse `!` first)
- [[skills/README.md]] — skills index & invocation guide (`$`-prefixed, load on demand)
- [[memory/errors/README.md]] — error memory index & logging protocol
- [[memory/Workflow.md]] — mandatory step-by-step execution loop (run on every task)
- [[memory/canvas-rules.md]] — rules for creating high-quality `*.canvas` files
- [[Visual.canvas]] — vault map
