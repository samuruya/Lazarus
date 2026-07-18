# Canvas Quality Rules

Rules to follow when creating or editing any `*.canvas` file (Obsidian canvas JSON) in this vault. The goal: a map that is legible, organized, and pleasant to read at any zoom. Apply these on every canvas change.

> Linked from [[../../main.md]]. Companion to [[../../memory/Workflow.md]].

## 1. Layout & Spacing

- **Group by function** into vertical columns (e.g. Core, Memory, Tasks, Excluded). One logical group per column.
- **Leave generous gutters** between columns (≥ 100px of empty space) so edges route through gaps and never cross over cards.
- **Give text-heavy cards room.** Any card whose content is long (READMEs, workflow docs, multi-line notes) gets a larger `width` (≥ 320) and `height` (≥ 180) so text wraps comfortably with padding around it — never cramped.
- **No overlaps.** No two nodes may share space. Before saving, mentally (or via a quick check) confirm boxes don't intersect and edges don't cut through other cards.
- **Consistent rhythm.** Align cards within a column to the same `x` and use a steady vertical step (`y` gap ≥ 40px) so stacks read as tidy lists.

## 2. Visual Style ("prettier & fancier")

- **Use group-header cards** (`"type":"text"`) above each column with a short emoji label (e.g. `## 🧠 Memory`). Pick a distinct `color` per group for instant scanning.
- **Color-code by category**, not at random — same color for sibling items in a group, different colors across groups.
- **Title block** at top-center: a `# ` heading plus a short `*italic*` subtitle describing the vault.
- Prefer clear, short card text; let wikilinks and file links do the navigation work rather than dumping prose into the canvas.

## 3. Edges (connections)

- Connect a group header to its first child, and chain children top→bottom with `top`/`bottom` edges inside a column.
- **Cross-column edges** use `left`/`right` sides so lines travel through the empty gutters, never over a card.
- Give every edge a stable, unique `id`; reuse the source's group `color` so relationships are visually traceable.

## 4. Maintainability

- **Keep it updated** whenever major files/folders are added/removed — add a card + edge, don't let orphans appear.
- Every significant node should be reachable from the title via edges (no disconnected islands).
- Prefer a single source of truth: the canvas is a navigation aid, not documentation — link out instead of duplicating content.

## 5. Validation Checklist (run before finishing)

- [ ] No two cards overlap.
- [ ] No edge crosses over a card body.
- [ ] Text-heavy cards have enough width/height to read without scrolling the card.
- [ ] Columns separated by clear gutters.
- [ ] Group headers + colors present and consistent.
- [ ] All major files/folders represented; no orphan nodes.
- [ ] Valid JSON (parseable by Obsidian).
