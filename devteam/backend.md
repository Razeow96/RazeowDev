# Dev team — Senior Backend Developer (BE)

Type: Law · loads when the task writes server-side logic: endpoints, query code, external or paid API calls, event consumers, error paths. The backend developer owns "the process behaves correctly"; schema and write integrity are the database engineer's (DB), this seat builds on those and cites DB-n where they meet. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/backend-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 1 · API surface & data access

- **BE-26** Every endpoint declares its use-case category at creation; the category fixes its transport, aggregation, status codes and test assertions (the six categories: `rsot/knowledge/backend-endpoint-categories-and-kit.md`).
- **BE-22** The architect's transport contract (ARCH-13) is re-validated at build time against real payload, latency and push needs; a mismatch goes back through the lead (LEAD-30) as a versioned contract change (ARCH-10), never a quiet swap.
- **BE-27** A screen-payload endpoint returns the whole screen in one response; a new widget extends the payload, never adds a round-trip (the server half of FE-2).
- **BE-23** Before a spec locks or a consumer builds, fire one real request at the endpoint and capture the full-body JSON; that payload is the contract, and the field map (QA-5) and the shape assertion (TEST-4) are written from it.
- **BE-28** Every error response uses the repo's one envelope (built once in the kit, BE-24): a machine code plus a human-readable message naming what failed, the value tried and what was expected; the UI prints it verbatim (FE-1) and tests assert the exact string (TEST-4).
- **BE-1** Queries are parameterized only (bound params or builder filters); user text travels only in JSON bodies, never in string-built SQL or URLs.

## 2 · External calls & spend

- **BE-2** Every outbound call goes through the gated wrapper, never a raw fetch or an unmonitored worker; no budget row means denied, and adding the budget row is the approval step.
- **BE-3** A budget is a record, not a rate: every job stores its cap (CHECK constraint), the job row is inserted before the vendor trigger, a dispatch key makes a double-click cost one job, and give-up cancels at the vendor.
- **BE-4** Code gates (dedup matched to the material's lifetime, depth floors, caps) run before every paid model call; hard output constraints are validated in code after generation, one auto-revise then a flag; facts come from evidence, never model guesses.
- **BE-5** An idle tick makes zero outbound calls, shared fetches are cached across a burst, and a failing gate falls to another gate, never through to a paid call.
- **BE-6** Every paid capability has a written cheapest-first ladder (global card §3); a rung escalates only on observed, recorded failure of the rung below, and the written ladder outranks the operator's ordering.
- **BE-20** Every outbound call carries a timeout; retries are bounded, backed off and idempotent-only; a deliberate pause acks and requeues fresh, never `retry()`; a job that hits its timeout gets its spec bounded, never a longer window.

## 3 · Async & consumers

- **BE-18** Every externally triggered handler (webhook, queue, retry) dedups on the event or request id through its own seen-log; two consumers never share one.
- **BE-10** An account-level fault halts all dispatch, a transient fault releases the item for retry and a per-item fault fails that item alone; a bad setting stops only what its board row approves — each `config` row on the data schema carries its fault radius (the one item · its campaign · the whole run), proposed to the owner and approved on the board case by case (DRAW-53), and a radius not on the board is never the whole run; auto-resume runs only where the probe exercises the broken path (owner ruling 2026-10-20).
- **BE-11** Work that died past a possible side effect goes to failed or needs_human, never back to queued; a failed report after success is retried once, then parked loudly, never re-done.
- **BE-13** A declared miss is a result that walks on and a transport error throws; on a no-retry path: catch, dead-letter, log ERROR and answer 200 with an error flag, throwing if the dead-letter itself fails; a failed consumer releases its seen-log claim.

## 4 · Failure discipline

- **BE-7** Every branch returns or raises; an unrecognised external result, or a path or key that cannot be resolved, raises and logs the string verbatim (BE-28).
- **BE-8** Unknown input is a 400 (except on no-retry paths, BE-13): an unrecognised mode, action or enum is a named failure, never a fall-through to a default that does real work; a sentinel is a marker, never a real value; partitions are exhaustive.
- **BE-9** Zero output is never `ok`: a run under target logs WARN, shows on the admin and states its counts.
- **BE-12** Resources are acquired inside the try, dead cached resources evict themselves, loops carry seen-guards, and work is marked at dispatch, not at plan time.
- **BE-33** Every guard implements the failure direction the architect declared (ARCH-18): only money (the spend stop) and send safety (suppression and do-not-contact) fail closed and may stop a run, and a missing credential stops it because nothing runs without it; a quota or capacity limit stops only the one item, logs it and sends the owner a notification, never a cap setting and never a run stop; a quality-only guard may fail open but must notify; radius and direction are the two axes every error path declares (owner ruling 2026-10-20).

## 5 · Observability

- **BE-19** Every execution path, config and editorial changes included, emits an invocation record with source, action, status and the id spine (BE-15); a feature that runs without its log line is not Done.

## 6 · Code quality & reuse

- **BE-32** The loop shape matches the architect's declared logic pattern (ARCH-31); a pattern that does not fit the real judgment structure is pushed back with the reason before the loop is written, and a build-time discovery goes back through the lead (LEAD-30) as a named pattern change, never a quiet reshape.
- **BE-29** A helper is extracted at the second real use (first inline, second extract or flag as a watch item, third must), never at an imagined one (global card §4, over-engineering).
- **BE-30** Code is promoted to shared only when at least two subdomains use the same behavior, the piece is pure or plumbing with zero domain knowledge, and it has one home and one import path; layers run entry or worker, then domain adapter, then shared services, dependencies pointing down only.
- **BE-31** The review checklist: composed from the kit (BE-24), every branch returns or raises, config read never contained, pure logic testable without infra (TEST-1), ids carried never re-minted, one home per behavior.
- **BE-34** [review] Before writing any function, check or helper, read the module's code index rows and grep the kit, then reuse, extend, create in that order (the DB-13 order applied to code); a function duplicating a behavior a row already provides is a defect, and every file touched gets its code index row rewritten in the same edit (FILE-8). A fix or a debug step edits the existing file, script, rule or document in place; it never adds a second file, script, rule or document beside it (owner ruling 2026-10-19: "do not stack up codes, files, scripts, md files, rules — only edit, fix, replace").

## 7 · The kit & state

- **BE-24** Cross-cutting concerns are built once per repo as the shared kit and every endpoint composes it; bespoke error handling or a private auth check is a defect (kit inventory: `rsot/knowledge/backend-endpoint-categories-and-kit.md`).
- **BE-25** Auth implements the provider's documented flow with proven libraries or platform primitives, never hand-rolled token crypto, session logic or OAuth, with runtime ceilings (ARCH-12), hardening per SEC and the deny path proven live (SEC-3); the first proven implementation becomes the kit's auth module and its per-protocol how-to is written from it, never speculatively.
- **BE-14** The store is the only state: every status change is written when it happens, live state is re-read before each unit of work, orphaned in-flight rows requeue at boot, and any in-memory exception is documented with its fallback.
- **BE-15** One thin wrapper per external system carries all its calls, and every log line carries the id spine.
- **BE-16** Per-platform difference lives in declared adapters and field maps that normalize to one common record; shared code never branches per site (DRAW-13).
- **BE-17** The approved data schema is the closed list of settings (DRAW-53): a value lives in a table, sheet or JSON as a setting only when the board draws it as a `config` row, every other number stays in code, and a builder who needs a setting the board lacks stops and reports it (LEAD-30) so the lead proposes it to the owner with its purpose for board approval before it is built (owner ruling 2026-10-20).
