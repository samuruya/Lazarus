# Skills

Reusable, domain-specific instructions and workflows for this project. Skills are invoked with the `$` prefix (e.g. `$build`, `$deploy`). Each skill lives in its own `.md` file under `skills/` and is loaded on demand when a task matches its description.

## Listing skills

| Skill | Invoke | File | Scope |
|---|---|---|---|
| Skills index | `$skills` | [[skills/README.md]] | one-shot |
| Code review scout | `$DUCK:"NAME OR PATH"` | [[skills/DUCK.md]] | one-shot |
| Plan & construct | `$PLAN:"NAME OR PATH"` | [[skills/PLAN.md]] | one-shot |
| Design snapshot | `$DESIGN:"NAME"` | [[skills/DESIGN.md]] | one-shot |

> **Scope** — `persistent` means the skill's instructions stay active from its `$`-call onward for the rest of the session; `one-shot` means it applies only to the single prompt it was called in.

> **Output** — `$DUCK` writes to `.laz/DUCK/`, `$PLAN` writes to `.laz/PLAN/` in the **current project folder**. Neither skill ever operates inside the Lazarus folder — see each skill's *Lazarus Exclusion* rule.

> Add a row here whenever a new `skills/<name>.md` is created. The **Invoke** column uses the `$` prefix; the **File** column wikilinks to the source; set **Scope** to `persistent` or `one-shot`.

Back to [[main.md]]

## Adding a skill

1. Create `skills/<skill-name>.md`.
2. Start with a one-line purpose, then the workflow/instructions.
3. Add a row to the table above using the `$<skill-name>` invocation and a **Scope** (`persistent` or `one-shot`).
4. Link it back to [[main.md]] so it stays connected in the vault graph.
