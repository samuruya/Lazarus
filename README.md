# Lazarus

A personal operating system for a coding agent — the instructions, commands, skills, design library, and memory that shape how the agent works on this and other projects.

Lazarus is not an application; it is the **brain/configuration** layer. It defines how the agent should think, plan, review, and push code, and it ships a reusable library of visual designs other projects can draw from.

## New to Lazarus? (getting a project set up)

These steps wire a new project into Lazarus. They're for you, the human — the agent follows its own internal checklist.

1. **Create a `Lazarus.md` in your project** — add a `Lazarus.md` file at the root of the project you want to wire into Lazarus. Have it point back to this vault (e.g. link to `main.md` and note that Lazarus governs the agent's behavior here).
2. **Create a copy-pasteable bootstrap message** — keep a block like the one below handy to paste into any future chat so the agent loads Lazarus. Replace `PATH-TO\Lazarus` with the real absolute path to this folder:

   ```
   go to "PATH-TO\Lazarus\main.md"
   initialize and learn it.
   make sure u know everything about Lazarus and use it for any future prompt.
   ```

3. **Run `!Info`** — ask the agent to run the `!Info` command to verify everything is wired up correctly and report the active configuration.

## What's inside

| Path | Purpose |
|---|---|
| `main.md` | Core operating instructions for the coding agent (init, workflow, prompt optimization, scope, version-control exclusions, linking). |
| `commands.md` | Custom `!`-prefixed commands (`!laz`, `!ignore`, `!only`, `!NREP`, `!UP`, `!q`, `!List`, `!F`) parsed before everything else. |
| `skills/` | On-demand `$`-prefixed workflows: `$PLAN` (plan & construct), `$DUCK` (code review scout), `$DESIGN` (design snapshot). |
| `DESIGN/` | A library of design/visual-language snapshots (e.g. `Charon`) the agent reuses when building or restyling UIs. |
| `memory/` | Long-term memory: error log (`errors/`), the mandatory execution `Workflow.md`, and `canvas-rules.md`. |
| `AGENTS.md` | Project-level agent instructions, indexing the available skills and commands. |
| `Visual.canvas` | Vault map (Obsidian canvas format). |

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
