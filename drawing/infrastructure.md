# Drawing law · Infrastructure system design

Type: Law · infrastructure system design: how it is drawn, in every repo · pulled through rulesindex when drawing an infrastructure system design, together with index-and-legend.md. Update: one rule = one line; replace the line, never add beside it; IDs are append-only.

Answers: how code reaches production and what serves users. Sits as a section on the domain high-level page (FIG-2). Owner-drawn; the architect keeps it current with `/architecture-ddd-draw`. Hand-built boxes (figma-law DRAW-2).

- **DRAW-35 Two drawings.** The infrastructure system design is the owner's drawing, in two parts: the deployment flow (repos → CI → staging → canary → production, with roll back) and the architecture (traffic in front → load balancer → app servers → stores, workers, monitoring). Never a provider topology. Approved additions are drawn into these two drawings (figma-law DRAW-7), never parked beside them.
- **DRAW-45 Boxes name roles.** Each server box names its role (staging · canary · production · workers), the tag it runs and its share of traffic; each store names the class of data it holds (sensitive · working · media · events); a rule that governs every release (example: migrations only add) is a box wired to the deploy edge; the roll back edge names its target.
