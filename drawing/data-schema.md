# Drawing law · Data schema

Type: Law · data schema: how it is drawn, in every repo · pulled through rulesindex when drawing a data schema, together with index-and-legend.md. Update: one rule = one line; replace the line, never add beside it; IDs are append-only.

Answers: every table and column of one module. Sits as a section on the module's page (FIG-2). Seat: database engineer, `/dataengineer-dataschema-draw`. Altitude: column (DRAW-28). Hand-built tables (figma-law DRAW-2).

- **DRAW-18 Data schema — shape.** One section per parent named `<code>.<k> <prefix>_`; a header box (table name · role) paired with its table; columns `key · data column · function · note`; the last row names the indexes and the path each serves. The first module drawn sets the shape and every later module copies it — a different shape is a defect on that page.
- **DRAW-21 Function vocabulary.** The function column uses one vocabulary: `id` · `ref` · `data` · `config` (an operator setting) · `status` · `enum` · `json` · `md` · `at`.
- **DRAW-43 Table naming.** The table prefix is the owning module: `<subdomain>_<parent>_<child>`; a root table is the bare parent word. Columns are local, never prefixed; `<parent>_id` points to the parent. Shipped names are permanent.
- **DRAW-44 Key column.** `PK` marks the id this table mints; `FK` marks an id taken from another table and names it in the note; a key taken from another domain arrives through an event on the CDT (DRAW-31) or a read on the context map, or the row is red.
