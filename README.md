# Lazarus

A personal operating system for a coding agent — the instructions, commands, skills, design library, and memory that shape how the agent works on this and other projects.

Lazarus is not an application; it is the **brain/configuration** layer. It defines how the agent should think, plan, review, and push code, and it ships a reusable library of visual designs other projects can draw from.

## What's inside

| Path | Purpose |
|---|---|
| `main.md` | Core operating instructions for the coding agent (init, workflow, prompt optimization, scope, version-control exclusions, linking). |
| `commands.md` | Custom `!`-prefixed commands (`!laz`, `!ignore`, `!only`, `!NREP`, `!UP`, `!q`, `!List`, `!F`) parsed before everything else. |
| `skills/` | On-demand `$`-prefixed workflows: `$PLAN` (plan & construct), `$DUCK` (code review scout), `$DESIGN` (design snapshot). |
| `DESIGN/` | A library of design/visual-language snapshots (e.g. `Charon`) the agent reuses when building or restyling UIs. |
| `memory/` | Long-term memory: error log (`errors/`), the mandatory execution `Workflow.md`, and `canvas-rules.md`. |
| `AGENTS.md` | Project-level agent instructions, indexing the available skills and commands. |
| `Visual.canvas` / `TODO.canvas` | Vault maps / open-task boards (Obsidian canvas format). |

## How it works

- **Commands (`!`)** take priority over the rest of a prompt. They scope the agent (e.g. `!laz` confines work to the Lazarus folder) or perform actions (e.g. `!NREP` / `!UP` push the workspace to git).
- **Skills (`$`)** are one-shot workflows loaded on demand. `$PLAN` and `$DUCK` operate only on a separate target project — they never write anything inside the Lazarus folder; `$DESIGN` snapshots a project's look into `DESIGN/`.
- **Memory** keeps a rolling log of errors and solutions, and a strict execution loop the agent follows on every task.

## Version control notes

`!NREP` and `!UP` push the workspace to git but deliberately exclude:

- the `.obsidian/` folder, and
- `app.json`, `appearance.json`, `core-plugins.json`, `graph.json`, `workspace.json`.

These are ignored via `.gitignore` and are never staged or pushed.

## Getting started

1. Read `main.md` — it is the entry point and defines the startup checklist.
2. Review `commands.md` and `skills/README.md` to learn the available `!` commands and `$` skills.
3. Use `!laz` to scope the agent to the Lazarus folder, or point `$PLAN` / `$DUCK` at another project to plan/review it.
