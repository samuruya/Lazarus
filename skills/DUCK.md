# $DUCK — Code Review & Refactor Scout

**Purpose:** Review code for bugs, anti-patterns, and improvement opportunities, then document findings in the `.laz/DUCK/` folder.

## Invocation

`$DUCK:"NAME OR PATH"`

- Target follows the same argument rules as commands (see [[commands.md]]):
  - **NAME OR PATH** — a bare file/folder name (matches anywhere in the working tree) or a path (relative/absolute; `.\NAME` is valid from the working dir).
  - **Bare form** — `$DUCK` with no `:"..."` defaults to the current workspace folder.
  - **Quotes optional** unless the name/path contains spaces.

## Lazarus Exclusion (MANDATORY)

- This skill is **only** for reviewing code and documenting findings in the **respective project** being worked on (the app/project under development), never inside the Lazarus project itself.
- **Never** read from, write to, create files in, or otherwise act inside the **Lazarus folder**. Do not place any `DUCK/`, `.laz/`, or `.md` output anywhere under the Lazarus project directory.
- All output goes to `.laz/DUCK/` in the **current project folder** (i.e. the inspected/target project), which must not be the Lazarus folder. If the current workspace *is* the Lazarus folder, stop and tell the user that `$DUCK` does not operate inside Lazarus — it requires a separate project target.

## Behavior

1. **Scan** the referenced target (file, folder, or whole workspace if bare).
2. **Hunt** for:
   - Bugs: logic errors, off-by-one, unhandled promises/rejections, null/undefined access, race conditions, leaks.
   - Smells: duplicated code, oversized functions, unclear names, missing error handling, fragile coupling.
   - Better solutions: simpler/cleaner approaches, performance wins, safer patterns, dead code to remove.
3. **Document** every finding in `.laz/DUCK/<project-name>.md` (create the `.laz/DUCK/` folder if missing). Use the project's name from its root (e.g. `package.json` name, or the folder name) for `<project-name>`.
4. Each finding uses this format:

```md
### [DUCK] short title
**Location:** file:line
**Issue:** what's wrong / what could be better
**Suggestion:** concrete fix or refactor
**Severity:** low | medium | high
```

5. Do not auto-apply fixes unless the user explicitly asks — this skill is review + documentation only.
6. Append to the existing `.laz/DUCK/<project-name>.md` if it already exists; do not overwrite prior findings.

Back to [[main.md]]
