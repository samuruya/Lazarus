# AGENTS.md

Project-level agent instructions for Lazarus. This file is loaded automatically by Kilo and should be kept in sync with the project's active skills, commands, and workflows.

## Skills

Skills are domain-specific workflows invoked with the `$` prefix. They are **not** commands (`!` prefix). When a prompt contains a `$`-prefixed token, treat it as a skill invocation, not a command lookup.

### Prefix Index

| Skill | Prefix | Scope | File |
|---|---|---|---|
| Code review scout | `$DUCK` | one-shot | [[skills/DUCK.md]] |
| Plan & construct | `$PLAN` | one-shot | [[skills/PLAN.md]] |
| Design snapshot | `$DESIGN` | one-shot | [[skills/DESIGN.md]] |

See [[skills/README.md]] for full invocation rules and adding new skills.

## Commands

Commands use the `!` prefix and are defined in [[commands.md]]. Commands take priority over the rest of the prompt — parse for them first. Skills are not commands; do not search for `$`-prefixed tokens in the command table.

## Instructions

- Reference [[main.md]] for the core operating protocol (prompt optimization, memory, workflow).
- Follow the linking rules in [[main.md]] §6 so the Obsidian graph reflects real relationships.
- Ignore any folder whose name starts with `!` (see [[main.md]] §4b).
