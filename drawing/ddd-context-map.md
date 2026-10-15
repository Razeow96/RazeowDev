# Drawing law · Context map

Type: Law · context map: how it is drawn, in every repo · pulled through rulesindex when drawing a context map, together with index-and-legend.md. Update: one rule = one line; replace the line, never add beside it; IDs are append-only.

Answers: how the domains hand off to each other, events and reads. Sits as a section on the domain high-level page (FIG-2). Seat: architect, `/architecture-ddd-draw`. Altitude: domain only (DRAW-28). Hand-built (figma-law DRAW-2).

- **DRAW-33 Context map.** The domains in a ring in flow order, clockwise (DRAW-10), the infrastructure hub in the middle; solid = the crossing events, dashed = reads and ticks; the loop closes from the final receipt back into the head domain's memory.
- **DRAW-40 Every edge is labelled and matches the CDT.** A solid edge carries its event and version and appears on the CDT; a dashed edge names what is read or what ticks; a CDT event missing from the map, or a solid edge the CDT does not hold, is a disagreement (DRAW-37). Reads to and from the hub are dashed doors, never events; where the final receipt has no receiver yet, the closing edge is drawn red, never left out.
