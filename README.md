# Lazarus

**Lazarus is your coding agent's "operating system."** It's a folder of instructions, commands, skills, and a design library that teaches the agent how to think, plan, review code, and push your work to GitHub — consistently, every time.

This README is written **for you** (the human using Lazarus). The agent reads `main.md`, not this file.

---

## Getting a new project set up (first-time users)

1. **Create a `Lazarus.md` in your project** — add a `Lazarus.md` file at the root of the project you want to wire into Lazarus. 

2. **Add content to the .md** — Have it point back to this vault e.g. link to `main.md`. Replace `PATH-TO\Lazarus` with the real absolute path to this folder:

   ```
   go to "PATH-TO\Lazarus\main.md"
   initialize and learn it.
   make sure u know everything about Lazarus and use it for any future prompt.
   ```

3. **Let the agent Read the `Lazarus.md`** ask the agent to read the `Lazarus.md` wire everything up correctly.

4. **Run `!Info`** — ask the agent to run the `!Info` command to verify everything is wired up correctly and report the active configuration.

---

## What's inside

| Piece | What it's for |
|---|---|
| `main.md` | The agent's core rulebook (workflow, prompt handling, scoping, what never gets committed). |
| `commands.md` | Instant `!`-commands you type in a prompt (e.g. `!laz`, `!NREP`, `!UP`, `!Info`). |
| `skills/` | Deeper workflows you trigger with `$` (e.g. `$PLAN`, `$DUCK`, `$DESIGN`). |
| `DESIGN/` | A library of visual styles the agent can reuse when building UIs. |
| `memory/` | The agent's long-term memory: past errors/solutions, its execution loop, and version info (`LAZ-INFO.md`). |
| `AGENTS.md` | A short index of the available skills and commands. |
| `Visual.canvas` | A map of how the Lazarus files connect (Obsidian canvas). |

---

## How you actually use it

You interact with Lazarus by typing special tokens into your prompts.

### `!` commands (instant actions)
Type these directly in a prompt:

- **`!laz`** — keep the agent focused on the Lazarus folder only.
- **`!NREP`** — push the current project to its GitHub repo (inits git if needed, but never creates the repo on GitHub).
- **`!UP`** — stage all changes, commit, and push (the "save my work" button).
- **`!Info`** — run a self-check; the agent reports the Lazarus version and confirms everything is wired up.
- **`!ignore:"X"` / `!only:"X"`** — tell the agent to skip or stick to a specific file/folder.
- **`!q`** — clear any active scoping.
- **`!List`** — show what commands are currently active.

### `$` skills (deeper workflows)
- **`$PLAN:"project"`** — produces a structured plan before any code is written.
- **`$DUCK:"project"`** — reviews code for bugs and improvements, and writes up findings.
- **`$DESIGN:"name"`** — snapshots a project's visual style into the `DESIGN/` library.

> `$PLAN` and `$DUCK` always work on a *separate* project — they never write anything inside the Lazarus folder itself.

---

## What never gets committed

When you use `!NREP` or `!UP`, Lazarus deliberately skips your Obsidian settings:

- the `.obsidian/` folder, and
- `app.json`, `appearance.json`, `core-plugins.json`, `graph.json`, `workspace.json`.

These are kept out of git automatically.

---

## Version

Current version is tracked in `memory/LAZ-INFO.md` (run `!Info` to see it). Author: **samuruya**.
