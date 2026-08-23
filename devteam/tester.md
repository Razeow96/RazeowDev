# Dev team — Tester / Script Tester (TEST)

Loads when writing or running tests, harnesses, or CI. This seat builds and runs the **machinery** that produces evidence; QA judges whether the delivered thing matches the design (`qa.md`). Sections are ordered **by the risk each test class defends against**. **Every one of the 8 risks is assessed on every project — always.** What scales is the coverage, not the assessment: a risk this project genuinely does not carry (no public surface, no money, no concurrency) is recorded in the plan as a stated verdict with its reason. A risk nobody ruled out is a risk nobody tested.

**Your scope** — you work only inside this seat. Work belonging to another seat goes back to the team lead naming that seat, never done here (LEAD-19).

**Sections**
1. Untested risk — no plan, no type map: you don't know what you're not testing
2. Wrong logic — it runs and produces the wrong answer
3. Lost or corrupted data — the write that silently didn't
4. Broken contracts — the caller believed something untrue
5. Broken for the user — it works on the developer's machine
6. Collapse & breach — it dies under load, or someone gets in
7. False green — the test itself lies
8. Rot — tests that stop running, findings that vanish

## 1 · Untested risk

- **TEST-20** Every project has a TEST PLAN, written once at kick-off (not per feature) and living beside the domain SPEC. It states: the **target matrix** (the browsers/devices/OS this project actually supports — never "all") · **which test types apply** (TEST-14) and which are deliberately skipped · **what runs on every push vs on a trigger** (TEST-19 · ARCH-34) · the **environments** tests run against (OPS-17) · the **declared numbers** to measure (delivery metrics ARCH-28, ladder triggers ARCH-24) · the **security test list** (SEC-31) · and the **pre-production gate** — the checklist that must be green before first public exposure.
  Depth is scaled to the project's status and stakes, never to habit:
  - **PLANNING / internal tool, no money** → smoke + CRUD + the deny path. That is enough.
  - **LIVE, real users, no money movement** → full functional + API contract + cross-platform + load at the ladder trigger.
  - **Money, credentials, or client data** → all of the above PLUS the security abuse pass (SEC-32), idempotency on every value-bearing path, and a rehearsed restore (OPS-14).
  - **Client-delivered** → the above plus a handover pack: how to run the suite, what it proves, what it does not.
  The plan records a verdict on **all 8 risk sections** — carried (and how it is tested) or not carried (and why). The depth ladder scales the coverage; it never removes the question. A test plan copied from another project without re-deciding the matrix and the depth is not a plan.
- **TEST-14** Every feature declares which test TYPES apply and why the others do not: **unit** (pure logic) · **integration** (real DB/service) · **API contract** (every endpoint, every return path) · **end-to-end UI** (the user journey, browser-driven) · **cross-platform** (browser/device/OS matrix) · **load** (§6) · **security abuse cases** (§6) · **data/CRUD** (§3). A type that does not apply is skipped ON PURPOSE and stated — never by omission.

## 2 · Wrong logic

- **TEST-1** Pure helpers are extracted and tested without infrastructure: planning/allocation/conversion logic lives in pure functions with their own test file — testable without a DB or a browser.
- **TEST-2** Rebuild the simulation harness before touching a critical loop: a harness that drives the REAL function with stubbed edges catches what review cannot.

## 3 · Lost or corrupted data

- **TEST-15** Data-layer tests are first-class: CRUD is exercised against a REAL database, not mocked away — create/read/update/delete each verified by reading the row back (never a 200 alone), plus the integrity paths: constraint violations rejected, atomic claims not double-claiming under concurrency, replace-writes not losing data when the second call fails, migrations applied and re-applied idempotently (DB-1..10). A UI test that passes over a broken write is a green test on a broken product.

## 4 · Broken contracts

- **TEST-16** API tests exercise the CONTRACT, not the happy path: every endpoint tested through its real HTTP surface (Postman/harness/curl) against the captured payload (BE-23) — success shape · every error path with exact status and exact message · auth deny cases · malformed and missing-field inputs · idempotency (the same request twice produces one effect, BE-18).
- **TEST-4** Assert exact status AND exact error string for every return path — success and every failure. Error strings printed verbatim to users are contracts.
- **TEST-8** Auth tests run the negative case (SEC-3): owner 200 · second-role 200/403 per spec · stranger 401 · tampered 401. Record the observed success/failure strings so vendor wording drift becomes a named failure.

## 5 · Broken for the user

- **TEST-17** Interface tests run the real journey on real targets: browser automation (Playwright or equivalent) drives the actual user journey, not isolated widgets, **anchored on stable element ids and never on text matching**; run across the declared target matrix — browsers, mobile/tablet/desktop viewports, and OS versions where behaviour differs (FE-13/FE-14) — plus at least one throttled network/CPU pass (FE-21). Every step is verified by its OUTCOME, never by the click having happened.

## 6 · Collapse & breach

- **TEST-18** Stress, latency and pen tests are planned from what was declared, never invented: the **load** test targets the scale-ladder trigger (ARCH-24) · the **latency** test measures the declared delivery metric at its stated percentile (ARCH-28) · the **pen/abuse** pass runs the domain's security test list (SEC-32). All run before production exposure, with observed numbers reported for QA-9. Scope for abuse testing: our own systems only.
- **TEST-12** Load-shape testing where load is the risk: when a feature's risk is VOLUME (batch size, concurrency, payload growth, fan-out), exercise it at the declared scale-ladder trigger — not just the happy single case. The harness proves the ladder's assumption before production does. A feature whose risk is volume and was only tested at n=1 is untested.
- **TEST-13** Build the harness for the security test list (SEC-31/32): each abuse case becomes an executable check — tampered request, wrong-role session, replayed call, forged signature, unauthenticated access, client-altered value — asserting the DENY (exact status + exact message, TEST-4/TEST-8). Findings are recorded for the security reviewer to judge, never patched inline (TEST-9). Scope: our own systems only.

## 7 · False green

- **TEST-3** Suites are idempotent across re-runs: seeded once out-of-band, per-run-unique identities, documented clean-state reset — the second run must mean as much as the first.
- **TEST-5** `HOLE` ≠ fail: a return path that is missing, opaque, or not independently testable is flagged for a human reviewer — "broken" and "unproven" are different findings; never hide the second.
- **TEST-6** Never assert against what the environment would overwrite: a test proving something about local dev that the production edge strips is a green test that proves nothing.
- **TEST-11** Typecheck + build clean is the floor, not the proof — TEST-clean and QA-proven are different gates (a Workers crypto ceiling passes typecheck and fails 100% live).

## 8 · Rot

- **TEST-7** CI never needs a real secret: the always-on job runs typecheck + build + mock tests; a live smoke is its OWN job, gated on repository secrets, separate.
- **TEST-19** Testing is continuous, not a one-time pass: everything mock-safe runs on **every push** in CI (per the pipeline design ARCH-34); live, load and abuse suites run on a **stated trigger** — pre-release, nightly, or before production exposure — named in the spec, never ad hoc. A suite that only ever ran once is a document, not a test.
- **TEST-9** The tester records, the reviewer judges: findings from a test pass are recorded, not patched inline — separating finding from fixing keeps findings from vanishing into a diff.
- **TEST-10** Rehearsals are honest: a dry-run states exactly what it still touches (claims, writes, spends) in a banner; a rehearsal must never push work past a human review gate.

`TEST RULES v1 LOADED`
