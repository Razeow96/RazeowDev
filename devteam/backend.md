# Dev team — Senior Backend Developer (BE)

Loads when the task writes server-side logic: endpoints, query code, external/paid API calls, event consumers, error paths. The backend developer owns "the process behaves correctly"; schema and write-integrity are the database engineer's (DB) — this seat builds on those, cite DB-n where they meet. Structured as a competency map: each section is one thing a senior backend developer owns.

**Your scope** — you work only inside this seat. Work belonging to another seat goes back to the team lead naming that seat, never done here (LEAD-19).

**Sections**
1. API surface — categorize, prove, compose whole, speak human
2. Data access & indexes — safe queries; the DB-seat junction
3. External calls & spend — the gate, the budget, the ladder, the timeout
4. Async & consumers — at-least-once reality: idempotency, claims, requeue law
5. Failure discipline — silent is forbidden
6. Observability — the run record + id spine on every path
7. Code quality & reuse — lean, earned abstraction, boring is good
8. The kit & state — the shared foundation everything composes

## 1 · API surface

- **BE-26** Every endpoint declares its use-case category at creation — `screen-payload` (frontend interface, one aggregated call) · `pull/poll` (periodic fetch) · `real-time push` (live logs/status → WebSocket/SSE/subscription, NEVER faked with tight polling) · `webhook` (no-retry caller) · `worker` (retryable caller) · `m2m`. The category decides its transport, aggregation, status-code semantics, and what the tests assert. An endpoint with no category is an endpoint nobody designed.
- **BE-22** Transport is re-validated at build time: the architect sets the contract (ARCH-13), the builder hits reality — payload size, latency, streaming need may prove the transport wrong (request/response → REST · client-composed varied reads → GraphQL only with real need · server-push/live → WebSocket/SSE · simple machine-to-machine → plain HTTP+JSON). A mismatch goes BACK to the architect as a versioned contract change (ARCH-10) — surfaced like any conflict, never a quiet transport swap mid-build.
- **BE-27** The screen payload is composed server-side — one call, whole screen (the server half of FE-2): every `screen-payload` endpoint returns the FULL interface payload in one response; adding a widget EXTENDS the payload, never adds a round-trip. Back-and-forth calls for one screen is a backend design defect, not just a frontend one.
- **BE-23** Prove the contract with one real call before the consumer builds: against ANY endpoint — a vendor's or our own — fire one real request (curl/Postman/harness) and capture the actual full-body JSON. The captured payload is the contract artifact: field maps are written from it (QA-5) and the shape assertion goes into the suite (TEST-4). "The spec says" and "the endpoint returns" are different facts.
- **BE-28** One error envelope, human-readable by design: every error response follows the repo's single envelope (built once in the kit, BE-24) — a machine field (code/class) + a human-readable message naming what failed, the value tried, and what was expected/available ("Insufficient credits: need 400, have 120" — never `ERR_0x2F`). The UI prints it VERBATIM (FE-1) and tests assert the exact string (TEST-4): the message IS the contract.
- **BE-21** Status codes are caller contracts — the code states which caller semantics apply: the no-retry webhook path answers 200-with-error-flag (BE-13); the worker path's caller CAN retry, so 400/409/500 there are real control signals. A report response answers two questions separately — did the job succeed, and was the report recorded (`ok:false, recorded:true` = "logged, stop"; `recorded:false` = "send again").

## 2 · Data access & indexes

- **BE-1** Parameterized queries only: bound params / builder filters; user text only ever in JSON bodies; never string-built SQL or URLs.
- *Seams into the DB seat (cite, don't restate):* every hot query path gets its derived index in the migration that ships it — **DB-12** · a write verifies its effect, ownership repeated in the WRITE filter — **DB-9** · ordering is monotonic, pagination keys unique — **DB-6** · integrity lives in schema — **DB-1..10**.

## 3 · External calls & spend

- **BE-2** No raw fetch: every outbound call goes through the gated wrapper; no budget row = DENIED; adding the budget row IS the approval step; vendor calls live in gated functions, never in unmonitored workers.
- **BE-3** Budgets are records, not req/sec: cap discovery per job (an uncapped job cannot even be STORED — CHECK constraint), insert the job row BEFORE the vendor trigger, dispatch keys make a double-click cost one job, and CANCEL at the vendor on give-up — walking away doesn't stop the bill.
- **BE-4** Never pay a model to refuse what code can refuse: code gates (dedup matched to the material's lifetime, depth floors, caps) run BEFORE every paid model call; hard output constraints validated in code AFTER generation (one auto-revise, then flag); facts re-forced from evidence, never model guesses.
- **BE-5** Idle must be free: an idle tick makes ZERO outbound calls; shared fetches cached across a burst; a failing gate falls to another gate, never silently through to a paid call.
- **BE-6** The cost ladder: every paid capability gets a written cheapest-first ladder; escalate only on OBSERVED failure of the free rung (record what you observed); gates sit where the money is spent — they don't refund acquisition. The binding ladder outranks the operator's ordering.
- **BE-20** Timeouts and bounded retries: every outbound call carries a timeout; retries are bounded, backed off, and only on idempotent operations; on a deliberate pause, ack + requeue fresh — never `retry()` toward the DLQ (burns delivery attempts on live work); a job still running at its timeout means the SPEC is unbounded — diagnose the spec, never re-fire with a longer window.

## 4 · Async & consumers

- **BE-18** Idempotent consumers: delivery is at-least-once everywhere (webhooks, queues, retries) — every externally-triggered handler dedups on the event/request id via a seen-log, and each consumer keeps its OWN seen-log: two consumers sharing one means whichever arrives first marks it seen for both, and the second silently never runs.
- **BE-10** Systemic errors HALT: an account/config-level fault freezes ALL dispatch (never burns the queue item-by-item); transient releases for retry; per-item terminal fails alone. Auto-resume only where the probe exercises the broken path — else manual resume.
- **BE-11** Nothing auto-requeues past a possible side effect: died-mid-action → failed / needs_human, never → queued; a human decides. Report-after-success failure: retry, then PARK the report and exit loudly — never re-do the work.
- **BE-13** A transport error and a declared miss are different answers: misses are declared results that walk on; transport errors throw. Fail-loud-not-lose on no-retry paths: catch, dead-letter, log ERROR, answer 200-with-error-flag; if the dead-letter itself fails, THROW. A failed consumer RELEASES its seen-log claim — a claim held over a failure makes the failure permanent AND invisible.

## 5 · Failure discipline

- **BE-7** No silent swallow: every branch returns or raises; an unrecognised external result RAISES and logs the string verbatim — a loud false alarm is cheaper than a silent false success.
- **BE-8** Unknown input = 400: an unrecognised mode/action/enum is a named failure, never a fall-through to a default that does real work; sentinels (`needs_setup`) are markers, never guessed real values; partitions are exhaustive.
- **BE-9** Zero output is never `ok`: a run producing less than target logs WARN and is visible; zero-survivor answers state their counts ("0 of 30: 22 failed constraints, 8 used").
- **BE-12** Guard the guarded block: resources acquired INSIDE the try; dead cached resources evict themselves; loops carry seen-guards; work is marked at dispatch, not plan time.
- **BE-33** Every guard declares its failure direction in its own code: a gate, validator, quota or rate limiter protecting a SCARCE or COSTLY resource (money, quota, credentials, capacity) fails CLOSED; a quality-only guard may fail open but MUST then alarm — a fail-open guard looks healthy while the protected thing silently stops being protected. Radius (ARCH-18) and direction are the two axes every error path declares.

## 6 · Observability

- **BE-19** Every execution path logs: every path emits its invocation record — source, action, status, correlation id — and a feature that runs without logging is NOT Done; a config/editorial change is an execution path too and gets its log line. This is a security control, not just debugging: the run log is one of the only surfaces the org audits for what ran, from where, and what it spent. Every log line carries the id spine (BE-15).

## 7 · Code quality & reuse

- **BE-32** Implement the DECLARED logic pattern faithfully, and push back when it is wrong — the pattern is the architect's decision (ARCH-31); this seat owns fidelity and feedback. Fidelity: the loop shape must match the declared pattern — if the code makes a wider blast radius the natural write (a global flag inside a per-consumer loop), the shape is wrong, not the rule. Feedback, in both directions: **before building**, if the declared pattern does not fit the real judgment structure, push back with the reason BEFORE writing the loop; **after hitting reality**, a build-time discovery (a starved pool, a collapsed rotation, a shared resource with two owners) goes back to the architect as a pattern change with the better pattern named — never a quiet reshape mid-build.
- **BE-29** Lean code — abstraction is earned, never speculative: the smallest correct implementation; no speculative helpers, no utils graveyard, no might-need-later layers (LEAD-7 in code form). A helper earns extraction at the SECOND real use — first use inline, second = extract or flag as a watch item, third = must extract. Abstracting at the first imagined use is how parent classes get invented for children that never arrive.
- **BE-30** Promotion to parent/shared passes three tests, all of them: (a) ≥2 subdomains use the SAME behavior — same, not similar-looking (similar twins diverge at the first edit); (b) the piece is PURE or plumbing — zero domain knowledge; domain rules never climb into the shared kernel, and a parent class carrying one child's special case is the tell it wasn't ready; (c) one home, one import path — no re-export shims; consumers keep thin instantiations. Layering is fixed: entry/worker → domain adapter → shared services; dependencies point down, never sideways.
- **BE-31** Good backend code is BORING — the review test: composed from the kit (BE-24) · every branch returns or raises · config read, never contained · pure logic extracted and testable without infra (TEST-1) · ids carried, never re-minted · every literal owned by a constant/config · one home per behavior. Bad code announces itself: a hand-rolled variant of something the kit has · a silent catch · a magic number · a copy-pasted twin · state held in the process · a fetch outside the gate · a failure without a name. Cleverness that adds a moving part is a net loss.

## 8 · The kit & state

- **BE-24** The backend kit — compliance by construction: cross-cutting concerns are built ONCE per repo as the shared kit and every endpoint composes it, never hand-rolls its own — one error envelope + single top-level catch (the AppError pattern) · the timeout/retry wrapper (BE-20) · the gated fetch (BE-2) · auth middleware + permission check · input coercers (SEC-1) · the run-log helper (BE-19). An endpoint with bespoke error handling or its own auth check is a defect.
- **BE-25** Auth is assembled from certified parts, never improvised: implement the provider's documented flow with proven libraries/platform primitives — never hand-rolled token crypto, session logic, or a homemade OAuth dance; runtime ceilings checked first (SEC-7), hardening per SEC, deny path proven live (SEC-3). The first working implementation becomes the repo's auth module in the kit (BE-24); a per-protocol how-to (`stacks/oauth2.md` etc.) is written from the first PROVEN implementation, never speculatively.
- **BE-14** The store is the only state — the process holds nothing: every status change written the moment it happens; live state (queue, mode) re-read before every unit of work; orphaned in-flight rows re-queued at boot; a documented in-memory exception names its fallback.
- **BE-15** Thin wrapper per external system: one wrapper per dependency, ALL calls through it, every log line carries the id spine.
- **BE-16** Adapter → normalize → common record: per-platform difference lives in declared adapters + field maps; downstream consumes ONE normalized shape; never per-site branches in shared code.
- **BE-17** Config is data the code READS, never contains: catalog entries, thresholds, allowlists, selectors, per-source headers live in tables/JSON; a knob is declared in the same pass as the code that reads it; a knob nobody reads is deleted.

`BE RULES v1 LOADED`
