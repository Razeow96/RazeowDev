# Dev team — Architect reference

REFERENCE class (structure-law): read when picking an architecture style or a logic pattern, or when filling the failure-map / contract-card templates; the law lives in `architect.md`.

## Style → problem (ARCH-5)

- Multi-domain, independent evolution, history/audit matters → **DDD + event-driven** (e.g. a multi-domain content platform).
- Simple admin/CRUD tool, one bounded context → **layered CRUD**, no event bus — never build a distributed monolith for one domain.
- Content / landing / marketing site → **static / JAMstack**, no backend to design.
- Data / batch job → **pipeline-stage** design, not aggregates.
- Unproven MVP with one payer to prove → **modular monolith first**; split to services only when the load exists (ARCH-2).

## The logic-pattern catalogue (ARCH-31)

- **Waterfall / pipeline** — stages whose output causally feeds the next. Chosen only when order is real, never because it is easiest to write.
- **Parallel / fan-out** — independent consumers judge independently; each shared resource gets one owner or a lock.
- **Rotation** — round-robin over a pool on a PERSISTED global cursor (a per-run index collapses to the same first item every time).
- **Shared-context window** — knowledge shared across a time/session scope (TTL cache, day plan, dedup window); the per-window state lives in DATA, never in the invocation.
- **Checklist / all-judge** — N consumers each record their OWN verdict against the shared item; terminal disposal only when every scope has refused, or a stated scope-independent invariant applies.



## The failure-map table (ARCH-29)

One table, in the domain SPEC, one row per failure mode:

`failure mode · prevention · loud signal · fall direction · GUARD (the test/check that catches it) · VERIFIER SEAT (tester / qa-verifier / security-reviewer)`

## The domain contract card (ARCH-30)

Four blocks, in the domain's doc:

- `CONSUMES` — inputs: events/APIs + the ids they carry
- `OWNS / CREATES` — its tables, the ids it mints
- `EMITS` — outputs: events/APIs + payload ids
- `STATE TRANSITIONS` — its entities' states, ERD-clear

`ARCH REFERENCE LOADED`
