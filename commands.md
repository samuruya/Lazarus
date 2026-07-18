# Commands

Commands used inside prompts to Kilo Code. These take priority over the rest of the prompt — parse for them first.

## General Rules

These rules apply to every command unless that command's **Exemptions** field overrides them.

| ID  | Rule                                                                                                                                                                                                                                                                                        | Exemptions |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| R1  | **Target format (`NAME OR PATH`).** Any command that takes a target accepts either a bare `NAME` or a `PATH`. See *Path format notes* below.                                                                                                                                                | —          |
| R2  | **Optional argument / bare form.** If a target argument is omitted (e.g. just `!NREP` with no `:"..."`), the command still applies using its defined default (commonly the current workspace folder). The quotes/argument are never required — only include them to name a specific target. | `!laz`     |
| R3  | **Optional quotes.** Quotes around an argument are optional unless the name/path contains spaces. `!ignore:src` and `!ignore:"src"` are equivalent.                                                                                                                                         | —          |
| R4  | **Command priority.** Commands outrank the rest of the prompt; honor them before any other instruction.                                                                                                                                                                                     | —          |
| R4b | **Scope state is top-priority memory.** The set of currently-active scoping commands (`!laz`, `!only`, `!ignore`, etc.) MUST be retained in working memory and honored across **every** subsequent prompt in the session — no matter how many prompts pass — until explicitly dismissed by `!q` (or replaced by another scope command). Treat the active scope as a standing, high-priority instruction that supersedes any later prompt that appears to contradict it. Never "forget" or assume a scope expired just because several prompts went by without mentioning it. **Silent maintenance:** keep the scope active and enforce it, but do **not** mention/announce the active scope in your answer unless (a) the user asks about it, (b) the scope changes (set/replaced/cleared), or (c) a prompt would act outside the scope and you must flag the conflict. | — |
| R5  | **Persistence of scope.** Any scoping command (`!laz`, `!only`, `!ignore`) stays in effect for the **entire session** — across every subsequent prompt — until it is explicitly cleared. It does not expire at the end of one prompt. The only ways to lift it are: `!q` (clears all command scoping) or an explicit replacement scope command in a later prompt. A later non-scoping prompt does **not** cancel an earlier scope. | — |
| R6  | **Strict scope isolation.** When a scope is active, the agent must not read, reference, modify, create, move, sync, or otherwise act on **anything outside** that scope — even if a later prompt seems to imply it. The scope is a hard boundary; it overrides the rest of the prompt wherever they conflict. Inside the scope, `!`-prefixed quarantine folders ([[main.md]] §4b) remain off-limits. | — |

### Path format notes (apply wherever R1/R2 reference a target)

- `NAME` may be a bare file or folder name; it matches anywhere in the working directory tree.
- `PATH` may be relative or absolute. A leading `.\NAME` (e.g. `.\src`) is a valid relative path that references a folder from the current working directory — treat it the same as `NAME` resolved against the working folder.

### Scope lifecycle (R5/R6 in practice)

Scoping commands establish a **session-long boundary**, not a one-prompt limit. The active scope is a **standing, top-priority instruction** (R4b) that must be kept in mind across many prompts until dismissed:

1. `!laz` / `!only:"X"` / `!ignore:"X"` → scope is set and **persists** into every later prompt, however many occur.
2. While a scope is active, all work is confined to it (R6). A later prompt that doesn't mention scope does **not** cancel it, and passing several prompts without restating it does **not** expire it.
3. A later scope command **replaces** the previous one (e.g. `!laz` then `!only:"src"` narrows to `src`; `!only:"src"` then `!laz` widens back to Lazarus).
4. `!q` is the only command that **explicitly clears** all command scoping, returning to full-workspace context (subject to permanent `!`-prefixed-folder isolation in [[main.md]] §4b). `!q:NAME,NAME...` clears **only** the named active scopes (e.g. `!q:laz,ignore`), leaving others intact. If `!q` (bare or targeted) finds nothing active to clear, tell the user — do not silently no-op.

If a prompt asks for something outside the active scope, flag the conflict and either skip it or ask the user to lift/change the scope — never silently act outside it.

## Skills vs Commands

Skills use the `$` prefix (e.g. `$DESIGN`, `$DUCK`, `$PLAN`). They are **not** commands and must not be matched against the command table below. Parse for `!`-prefixed commands first; if none are found, check for a `$`-prefixed skill invocation.

See [[skills/README.md]] for the full skills index and [[main.md]] for how skills fit into the overall workflow.

## Command Table

| Command | Syntax | Behavior | Exemptions |
|---|---|---|---|
| Ignore | `!ignore:"NAME OR PATH"` | Fully ignore the file or folder given (by name or path) for the rest of the task — do not read, reference, or modify it. Other files/folders are unaffected. If bare `!ignore`, behavior is undefined — supply a target. **Persists for the session (R5) and is a hard boundary (R6).** | R6 (ignore narrows, doesn't broaden) |
| Only | `!only:"NAME OR PATH"` | Restrict the entire task to the file or folder given (by name or path). Do not read, reference, modify, or act on anything outside that target — other files/folders (including any `!`-prefixed folders) are ignored for this task. **Stays in force until `!q` or a later scope command (R5); hard boundary (R6).** | — |
| Lazarus | `!laz` | Restrict the entire task to the Lazarus folder only. Equivalent to `!only:"PATH/TO/LAZARUS"` — do not read, reference, modify, or act on anything outside the Lazarus folder (including any `!`-prefixed folders within or outside it). **Once called, `!laz` stays active for the whole session (R5) until `!q` or an explicit replacement scope is given. While active, nothing outside Lazarus is touched (R6).** | R1, R2 (takes no target — always scopes to the Lazarus folder) |
| Push Repo | `!NREP:"NAME OR PATH"` | Push the current workspace folder to its GitHub repository. This is a push-convenience command: it does **not** change scope and does **not** create or modify any project folders. If no name/path is given (bare `!NREP`), use the current workspace folder. If that folder is **not yet a git repository**, initialize one automatically (`git init`) — but only when none exists; never re-init an existing repo. Then run `git push` (adding the remote only if one is already configured/missing, but never creating the GitHub repo itself) from that folder. **Excluded from all git operations ([[main.md]] §4d):** the `.obsidian/` folder, and the files `app.json`, `appearance.json`, `core-plugins.json`, `graph.json`, `workspace.json`. Never stage, add, or push these. | — |
| Stage & Push | `!UP:"NAME OR PATH"` | Sub-command of `!NREP`. Stage all changes (`git add -A`), commit them with an auto-generated message, and push to the existing GitHub repository. Does **not** change scope and does **not** create or modify any project folders. If no name/path is given (bare `!UP`), use the current workspace folder. If that folder is **not yet a git repository**, initialize one automatically (`git init`) — but only when none exists; never re-init an existing repo. If no remote is configured, report it and stop (never creating the GitHub repo itself). **Excluded from all git operations ([[main.md]] §4d):** the `.obsidian/` folder, and the files `app.json`, `appearance.json`, `core-plugins.json`, `graph.json`, `workspace.json`. Never stage, add, or push these. | — |
| Quit scope | `!q` | **Reset command-driven scoping** from earlier prompts in this session. <br>• **`!q` (no argument):** clear *all* active scoping (`!laz`, `!ignore`, `!only`, etc.) and return to full-workspace context (subject to permanent `!`-prefixed-folder isolation, [[main.md]] §4b). If **no** scoping command is currently active, do nothing and **tell the user** that nothing was cleared because no scope is set. <br>• **`!q:NAME,NAME...`:** clear only the named, currently-active scoping commands. `NAME` is the command keyword without the `!` (e.g. `laz`, `only`, `ignore`). One or many, comma-separated, no spaces required (e.g. `!q:laz,ignore`). If a named command isn't active, skip it (don't error). If *none* of the named commands are active, tell the user that none of them were active so nothing was cleared. <br>After clearing, remaining scopes (if any) stay in force per R5; only the named/all scopes are lifted. This is the explicit off-switch for R5. | R1, R2 (matches by command keyword, not file path; takes optional `:NAME` list) |
| List scopes | `!List` | List every command currently **in effect** for this session (active scoping from R5: `!laz`, `!only`, `!ignore`, etc., with their targets/paths). If **no** command is currently active, tell the user that no commands are in effect. Read-only — does not change any scope. Takes no argument. | R1, R2 (takes no target) |
| Make Folder | `!F:"NAME"` | Create a folder with the name given inside `""` (relative to the current working/scoped directory). If a folder with that name already exists, do not overwrite or error — leave it as is. Quotes optional unless the name contains spaces. | — |
| Info | `!Info` | Diagnostic / self-check command. Verify the Lazarus setup is wired up correctly and report the active configuration: confirm `main.md`, `commands.md`, `skills/`, `memory/`, and `AGENTS.md` are present and readable; read `memory/LAZ-INFO.md` and report the **Lazarus version** (Version, Commit, Date); list any active scoping commands (R5) and their targets; confirm git state (repo present?, remote configured?, tracked vs excluded files per [[main.md]] §4d); and surface any missing/broken pieces. Read-only — changes nothing. Takes no argument. | R1, R2 (takes no target) |

Add new rows here as you come up with more commands. Keep the "Behavior" column precise enough that there's no ambiguity in how Kilo should act on it. For any new command, list the rule ID(s) it breaks in its **Exemptions** field.

See [[main.md]] for how these fit into the overall workflow.

Related: [[memory/errors/README.md]] for the error-logging protocol, [[Visual.canvas]] for the vault map.
