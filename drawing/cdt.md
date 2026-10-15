# Drawing law · CDT (cross domain transmission)

Type: Law · CDT: how it is drawn, in every repo · pulled through rulesindex when drawing a CDT, together with index-and-legend.md. Update: one rule = one line; replace the line, never add beside it; IDs are append-only.

Answers: which events cross a domain boundary, key by key. Sits as a section on the domain high-level page (FIG-2). Seat: architect, `/architecture-ddd-draw`. Altitude: event key (DRAW-28). How to design what a payload carries: knowledge file `rsot/knowledge/event-payload-design.md`.

- **DRAW-31 CDT.** Only the events that cross a domain, at most five, one box per event: a header (event · version · origin domain → destination domain · form of hand-off) over a table with columns origin data set · event key · destination data set · status. Every key is listed as `origin table.column → event key → destination table.column` (column-level lineage); a key with no designed destination is red, and the red rows are the build list. Rendered from a table, never hand-coloured.
- **DRAW-41 Everything else is named, not boxed.** A hand-off that is a table read or a record is not an event and gets no box; the section note lists those reads in one line, so the reader sees that every boundary was classified.
- **DRAW-42 Additive versus breaking change.** Adding a key bumps the event's schema version and keeps the event name; removing, renaming or retyping a key is breaking and mints a new event type. The header shows the live version; a header and section name that disagree on the version is a defect fixed on sight.
