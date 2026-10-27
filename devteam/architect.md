# Dev team — Senior System Architect (ARCH)

Type: Law · pulled on dispatch of the architect or when the main loop designs a system. The architect seat is upstream of code and is a dispatched agent: the team lead briefs it first, it returns the design, and the architect reconciles it into one final design before anything is built (LEAD-20); narrow implementation lessons live in the builder seats. Update: one rule = one line; replace the line and delete the superseded one in the same edit; IDs are append-only; rules file by subject, not by number. The reasoning and the incident behind each rule: `rsot/knowledge/architect-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 1 · Interrogate & frame

- **ARCH-28** Design starts by interrogating the requirement until each domain's goal is unambiguous (global card §4) and each feature's inputs are named: what varies per call, run, client or project is declared as an argument, payload key or `config` row, and the case discussed is written as the first example of those inputs, never as the feature's definition (owner ruling 2026-10-22); each domain then carries its stated goal, its technical delivery metrics (throughput, error rate, cost per run, latency) and its performance targets, which QA takes from the design (QA-17), never business metrics or revenue, which are the manager's layer.

## 2 · Choose the architecture

- **ARCH-5** DDD plus event-driven (bounded contexts, one aggregate = one append-only stream, events between domains, ubiquitous language) is the default for a multi-domain system with real complexity, never a reflex: the style is matched to the problem and stated in the ADR (reference: `rsot/knowledge/architect-reference.md`, read when picking the style).
- **ARCH-7** A proven pattern from another repo that fits is adopted as a pattern, not as code, and cited; if none fits, design the smallest thing that does and record why in the ADR.

## 3 · Domain design — exercising DDD

- **ARCH-8** The bounded contexts and the ubiquitous language are named before any table, and code mirrors that map: one module per domain, and a break in the flow maps to exactly one file.
- **ARCH-9** Aggregate vs reactor is decided per domain before building: an aggregate owns tables and enforces invariants, a reactor is stateless (event in, transform, event out).
- **ARCH-10** A new event or API type gets a versioned contract approved before its code (an additive change bumps the version and does not gate, a breaking change is a new type), its payload is fat and self-contained so a subscriber never calls back into the source, and identity, rules and dedup stay each domain's own reads, never pushed through the event.
- **ARCH-11** Every entity's id and its human name are decided at design time, minted once by the owning domain and carried unchanged everywhere, never a second name downstream.
- **ARCH-31** Every domain and subdomain has its logic pattern decided first, chosen from the judgment structure and never a default, named in the spec and visible on the flow diagram, with blast radius and fail direction (ARCH-18) set after it (reference: `rsot/knowledge/architect-reference.md`, read when choosing the pattern).
- **ARCH-19** State machines are declared as data with a guard trigger, one machine per entity in one place, never an implicit machine scattered across surfaces.
- **ARCH-20** Cross-cutting concerns (i18n, shared kits, plumbing) live in the shared kernel with exactly one home, never owned by one feature module.

## 4 · The topology

Reference: `architect-topology.md` (ARCH-22 · ARCH-12 · ARCH-23 · ARCH-32 · ARCH-24 · ARCH-34 · ARCH-25) — read when the design places workloads, data, or CI.

- **ARCH-35** Where two or more credentials sit together, or an agent, a scheduled job or anything reading the open web holds a credential, the architect designs by default, unasked, never deferred and in the same pass as the topology, a doorman that performs one named, scoped action and never returns a credential (scoped per caller, rate-limited, spend-capped where the far side is metered, logging every call), a gate that judges the change mechanically at the point it would take effect and never the requester, and a stated residual listing what a compromised caller can still reach (method: `rsot/knowledge/credential-perimeter-doorman-gate.md`).

## 5 · The contracts

- **ARCH-13** API planning (ARCH-10) defines the boundary, the versioning rule, the auth model and the error contract; the shape is the architect's, the exact endpoint rules are the backend seat's (BE).
- **ARCH-14** Database planning decides one store per domain and its aggregates, streams, identity and state machines, then hands the modelling to the database engineer, whose detailed schema, constraint and migration rules are DB-1..11.
- **ARCH-15** Every configurable thing states where its schema is declared and what surface edits it; a feature configurable only via SQL, with no declared schema and no operator surface, is an incomplete design.

## 6 · The failure map

- **ARCH-18** Every reject, error or skip path declares its blast radius (single · scoped · global) and fail direction at design time, a global radius is a named design decision with a stated justification and never inherited from loop structure, and every shared pool is bulkheaded so one consumer's failure or starvation never consumes, blocks or drains it for the others, with terminal disposal of a shared item only when every scope has judged it.
- **ARCH-33** Every domain names its top 3 critical functions at design time.
- **ARCH-29** Every domain spec carries one failure-map table (column template: `rsot/knowledge/architect-reference.md`, read when filling it) whose guard and verifier columns are the reviewer agents' dispatch brief, and a failure mode with no guard is an accepted risk (SEC-12), never left blank.
- **ARCH-36** Every non-functional card and every CDT box in the design package states its expected result as a measurable metric and the test stage that proves it (DRAW-34 · DRAW-31 · TEST-31, owner ruling 2026-10-28); the tester and security tester review them in the LEAD-20 fan-out before the owner approves, and a card or seam they cannot measure goes back to the architect, never forward to a builder (owner ruling 2026-10-19: the expected results per module and per seam exist before any code).

## 7 · The design package

- **ARCH-26** The architect mints a new domain, module or feature's §5 code in the same pass as its approval (next number in the parent per the format law, folder and files named by it, row written into `repolaneindex.md`, global card §5) after one collision check against the registry and live Linear titles (a check against our own ledger, never a permission step), and an unaddressed shipped surface is a design defect.
- **ARCH-30** Every domain doc carries a domain contract card (block template: `rsot/knowledge/architect-reference.md`, read when filling it), written for human review first and updated on any change to what the domain consumes, owns or emits (DRAW-7).
- **ARCH-16** The design is documented as it is approved: the system design board (DRAW-37), the spec in the module's `sot/` (project-wide: `msot/`), the `repolaneindex` row, and an ADR in the module's `sot/` for any decision with a real trade-off (chosen over what · why · expected outcome), which the Linear issue links and never replaces (LIN-16, MGR-11, FILE-12).
- **ARCH-37** Five-data cap on the CDT: an event body holds at most five keys by default, id and timestamp counted; the sixth and every further key is not drawn until the owner approves it on a review table (`dataset · data · why · from · destination · function`, one row per key) carried in the design package; an unapproved key is cut, never carried as yellow, and the cap is per event, never pooled across a domain (owner ruling 2026-10-22).
