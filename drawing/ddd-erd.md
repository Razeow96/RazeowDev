# Drawing law · ERD (entity relationship design)

Type: Law · ERD: how it is drawn, in every repo · pulled through rulesindex when drawing an ERD, together with index-and-legend.md. Update: one rule = one line; replace the line, never add beside it; IDs are append-only.

Answers: the domains as entities and the keys that link them. Sits as a section on the domain high-level page (FIG-2). Seat: architect, `/architecture-ddd-draw`. Altitude: domain only (DRAW-28).

- **DRAW-32 ERD.** The entities are the domains, one box each in flow order (DRAW-10), listing its main data sets (PK = the id it mints, FK = the id taken from the domain before). Hand-built entity boxes grouped by store in sections (figma-law DRAW-2; shape: `rsot/knowledge/figma-drawing-recipes.md`), lines labelled `<A> 1—n <B> <key>`, solid for a real key and dashed for a soft key that is never joined across stores. Never table level: tables and columns belong to the data schema sections.
