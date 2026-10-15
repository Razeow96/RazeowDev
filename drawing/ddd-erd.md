# Drawing law · ERD (entity relationship design)

Type: Law · ERD: how it is drawn, in every repo · pulled through rulesindex when drawing an ERD, together with index-and-legend.md. Update: one rule = one line; replace the line, never add beside it; IDs are append-only.

Answers: the domains as entities and the keys that link them. Sits as a section on the domain high-level page (FIG-2). Seat: architect, `/architecture-ddd-draw`. Altitude: domain only (DRAW-28).

- **DRAW-32 ERD.** The entities are the domains, one box each in flow order (DRAW-10), listing its main data sets (PK = the id it mints, FK = the id taken from the domain before). Native erDiagram (figma-law DRAW-2), crow's-foot lines labelled with the linking key. Never table level: tables and columns belong to the data schema sections.
