# $DESIGN — Capture the Current Project's Visual Language

**Purpose:** Snapshot the current project's look-and-feel — color scheme, border radius, buttons, cards, and overall styling — into a reusable design entry in the Lazarus `DESIGN/` library.

**Scope:** one-shot (applies only to the prompt it is called in).

## Invocation

`$DESIGN:"NAME"`

- Argument rules mirror commands (see [[commands.md]]): `NAME` (no path), bare `$DESIGN` → name the entry after the current project, quotes optional unless spaces.
- The `NAME` sets the design entry's filename. The entry **always** goes in the fixed library `Lazarus/DESIGN/` — see **Fixed save location** below. Do not interpret `NAME` as a path.

## Fixed save location (MANDATORY)

- **Always** save the design entry to `Lazarus/DESIGN/<NAME>.md` (the `DESIGN/` folder inside the Lazarus project, i.e. the sibling of this `skills/` folder).
- **Never** create, write to, or otherwise place a `DESIGN/` folder in the current working directory, the inspected project's directory, or any other location.
- If `Lazarus/DESIGN/` does not exist, create it there only — never anywhere else.

## Behavior

### 1. Inspect the current project's styling

Read the active stylesheets and component code (within the current scope) and extract, concretely (with real values, not vague descriptions):

- **Color scheme** — background, panel, surface, border, text, accent/muted, and any CSS variables that define them.
- **Border radius** — the radius used on buttons, cards, inputs, chips, avatars.
- **Buttons** — padding, border, background, hover/active states, font weight/size, shadow.
- **Cards** — background, border, shadow, gap, padding, header styling.
- **Inputs / pills / chips / tabs** — their look for completeness.
- **Typography & spacing** — font family, sizes, and the spacing scale in use.
- **Motion** — notable transitions/animations that define the feel.

### 2. Document it

Save to `Lazarus/DESIGN/<NAME>.md` (the fixed library location — see **Fixed save location**). Use this template:

```md
# <NAME> — Design Snapshot

## Overview
<one-line feel of the style>

## Colors
- Background: ``
- Panel / surface: ``
- Border: ``
- Text: ``
- Accent: ``
- Muted: ``
- (any other tokens)

## Border Radius
- Buttons: ``
- Cards: ``
- Inputs: ``
- Chips/avatars: ``

## Buttons
- Padding / border / background
- Hover & active states
- Font: weight ``, size ``

## Cards
- Background / border / shadow
- Padding / gap
- Header style

## Inputs, Pills, Tabs
<concise notes per component>

## Typography & Spacing
- Font: ``
- Scale: ``
- Spacing: ``

## Motion
<transitions/animations that define the feel>

## Notes
<what makes this style cohesive; when to reuse it>
```

### 3. Link & register

- Add a wikilink back to [[main.md]] and to the [[DESIGN/README|DESIGN library]].
- If the DESIGN library index exists, note the new entry there.

Do not modify the project's styles — this skill only **documents** the current look.

Back to [[main.md]]
