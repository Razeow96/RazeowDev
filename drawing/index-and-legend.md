# Drawing law · Index and legend

Type: Law · index and legend: how it is drawn, and the conventions every kind obeys, in every repo · pulled through rulesindex when drawing any kind, together with that kind's file. Update: one rule = one line; replace the line, never add beside it; IDs are append-only. The reasoning and the incident behind every DRAW rule, across all nine drawing files: `rsot/knowledge/drawing-law-rationale.md`, same rule IDs; the measured shape of each kind on the approved boards: `rsot/knowledge/figma-drawing-recipes.md`.

Board-wide law (which boards, page order, approval, links) is figma-law. Cited, never redefined here: FIG-2 page model · DRAW-2 native objects first · DRAW-3 screenshot, then hand over · DRAW-4 one name, four surfaces · DRAW-5 board link · DRAW-6 latest only · DRAW-7 the drawing moves with the change · DRAW-29 the board wins, edits proposed as a table, the board holds state only (no update or change notes, no history).

## The kinds and where they sit

- **DRAW-1 Classify before drawing.** Every request resolves to one kind: index (page 0) · ERD · context map · CDT · infrastructure (sections on the domain high-level page) · functional & non-functional flow · data schema · user flow (sections on the module's page, per FIG-2) · docs tree (its own pages, see docs-tree.md). **Clear** → the kind's seat draws it with its skill, no question. **Unclear** → ask one question: *"Which kind — index, ERD, context map, CDT, infrastructure, functional & non-functional flow, data schema, user flow or docs tree?"* Never guess the kind and draw.
- **DRAW-26 Names are fixed.** Every page and section is named `<n> · <Kind> — <name>`: page 0 is the index, page 1 the domain high level, then one page per module numbered in flow order; a kind section on a page takes `<n>.<k> · <Kind> — <name>`. Before creating a page or section, name its kind; one that is no kind is not created.
- **DRAW-27 One section, one question.** A section answers one question and its name is that question. A section never mixes kinds; a mechanism lives on the page of the module that owns it, never duplicated.
- **DRAW-28 One altitude per kind.** Domain (ERD, context map) → event key (CDT) → feature step (flow) → column (schema) → screen and button (user flow). A kind drawn one level too deep (a table-level ERD, a column-level CDT) is the most common drift and is always rejected; unsure → propose the altitude in chat first.
- **DRAW-37 Complete = every kind exists and agrees.** Together the pages are the system requirements & design; no separate prose SRD. A page that disagrees with another is a defect on the newer page. Completeness is checked kind by kind before a build is baselined.
- **DRAW-49 Copy the shape.** The first section of a kind on a board sets its shape (labels, geometry, card layout); every later section of that kind copies it from the board, read through the MCP, with the kind's law file and `rsot/knowledge/figma-drawing-recipes.md` pulled first, never redrawn from memory; a section whose shape differs from its siblings is a defect on the newer section.

## Page 0 · Index

- **DRAW-30 Index recipe.** Page 0 holds, in this order: the project title and one-line purpose · a table of what each page answers, in reading order · the domains or modules in flow order (code from the lane index · what it holds · writer · health today) · the flow-order and ID-spine line · the legend (DRAW-20, DRAW-39) · the naming law (codes, table prefix, event change rule) · links (lane index, vocabulary, Linear project, this law).

## Flow order and the spine

- **DRAW-10 Flow order is law.** On every drawing that has one, flow order is the repo's domain order (lane index), left → right or clockwise. Edges go forward only; a line that skips a domain is wrong even where a foreign key exists, because the downstream domain receives through the hand-off. Lookups are a tag on the box, never a line.
- **DRAW-14 The ID spine rides every crossing.** Each crossing edge or port is labelled with the event, its version and the repo's ID spine; a hop that drops the spine is drawn as a break, and the spine is shown stamped onto the final output.

## Colour and honesty

- **DRAW-20 One legend.** Green = built and observed working today · blue = approved: built and not live yet, or not built yet, the box's own last line saying which (`not built` · `built, not live`) · yellow = built but thin, or an open owner decision · red = missing, broken, or a key leaving a domain with nobody wired to receive it · grey = deleted by the approved design, shown only to mark what goes · white = an actor or a decision · purple = a port or an outside system. Headers in the saturated fill, rows in the light variant (board legend approved 2026-10-11, the law aligned 2026-10-15).
- **DRAW-39 Legend marks beyond colour.** White box = an actor (owner, visitor) · solid arrow = an event crossing a domain · dashed arrow = a read, a tick or a poll · arrow label = event · version · what rides on it · a drop edge ends with its blast radius (DRAW-12). Every mark used on the board is in the page 0 legend; a mark not in it is not drawn.
- **DRAW-22 Colour is a claim.** One colour system per board. Green means observed working, red means observed or reasoned break; a guessed colour is a lie.
- **DRAW-16 Target only.** Design pages hold the target the owner approved and the open proposals, nothing else; the shipped shape is not drawn into them. AS-IS is drawn only when the owner asks for a gap drawing, on its own page of a separate file, from live-verified state (database, triggers, deployed code), never docs. Nothing is built from a target until it is approved and its gap written to Linear.
