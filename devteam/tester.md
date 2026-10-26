# Dev team — Tester / Script Tester (TEST)

Type: Law · loads when the task writes or runs tests, harnesses or CI. This seat is a **builder**: it writes every test script and owns CI (owner ruling 2026-10-19), from the lead's brief of expected inputs and outputs, in parallel with the code builder; devops owns CD (`devops.md`); QA judges whether the delivered thing matches the design and runs the proven run (`qa.md`); the release tester owns the project's standing live contract and journey suite and its contract files (`release-tester.md`, owner ruling 2026-10-23); §1–§8 are ordered by the risk each test class defends against, and every one of the 8 risks is assessed on every project. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/tester-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 0 · The test builder

- **TEST-21** This seat, the security tester (SEC-40, security tests only) and the release tester (REL-1, the standing release suite and contract files only) are the only authors of test code; this seat writes unit, integration, API/contract, per-build end-to-end, property and load tests, their fixtures, conftest files and the CI workflows; every other seat builds code only and is blocked from test paths by the HQ-M2.8 test guard hook [CI] (owner ruling 2026-10-19; release tester 2026-10-23: a builder testing his own code only tests what he already knows).
- **TEST-22** Tests are written black-box from the lead's brief (LEAD-34) and the public interface, never from the implementation: the first test set is written before the seat opens the code under test, and no expected value is ever copied from the code's output.
- **TEST-23** The brief states every expected output result as a technical metric the seat can measure — a count (asked 100 → stored ≥ 100, duplicates = 0), an exact value or string, a status code, a payload shape, a time limit (p95 < 90 s), a cost ceiling, a row read back — never a described outcome ("works", "looks right", "sends the drafts"); a brief with an unmeasurable result, or lacking inputs, error cases, idempotency or the real-service check, is returned as `BRIEF INCOMPLETE` naming each line (LEAD-18), never filled with an invented expectation.
- **TEST-24** Every new test is shown failing against the code before the change, for the stated reason, and the red run is kept as evidence beside the green one; a test that cannot fail proves nothing.
- **TEST-25** A failing test goes back through the lead to the builder as `FAIL <test> · expected <x> · got <y> · repro <command>`; this seat never patches product code, never edits an expected value to match the output, never weakens, skips or deletes a failing test, and changes a test only when the lead changes the brief, recording the brief revision the test came from.
- **TEST-26** Any count, quantity or limit in the brief is asserted as a standing check (asked N, got N, or a named short-result signal, never a silent pass), functions that parse, page, dedupe or count get property-based tests that generate inputs nobody listed, and every feature test runs the brief's example plus at least one second input set with different identities (client, campaign, sheet, ids); a test green only on the brief's own example is incomplete (owner ruling 2026-10-22).
- **TEST-27** This seat owns CI: every push runs all suites and the static checks the release tester names (REL-5), and fails on a red suite, a skipped or xfail test without a ticket, a test or CI file changed by an author other than the tester, the security tester or the release tester (each only in its own paths), and a new test with no recorded red run; coverage is reported but never the gate; mutation testing on the changed code (mutmut, Stryker or the stack's equivalent) is the measure that tests catch bugs, and surviving mutants become new tests.
- **TEST-28** Every outside service is checked for real once per feature, small and inside the lead's approved spend, and the response is recorded as a scrubbed fixture with its recorded-on date that CI replays; fixtures are re-recorded on the trigger the test plan names (TEST-19), because a recording cannot see the service change.
- **TEST-29** Every seam between systems (engine ↔ n8n, Worker ↔ store, app ↔ provider) gets a contract test on both sides of the payload, both sides reading the release tester's contract file for that seam, never a local copy of the shared values (REL-4), and a workflow tool (n8n, Actions) is tested by executing it against a test input, never by reading its definition.
- **TEST-31** Every expected result is proved at a named layer — static · module · seam · integration · journey · smoke, with property-based and mutation as techniques (`rsot/knowledge/test-harness-patterns.md` § Test layers) — the test plan (TEST-20) states which layer proves each board card and each CDT crossing, and CI runs them on their cadence: static, module, seam, property and fast integration on every push (merge blocked on red); mutation weekly as a report (owner ruling 2026-10-19); the journey and smoke layers against the live surfaces are the release tester's suite (REL-9, owner ruling 2026-10-23).
- **TEST-32** A higher-level test that fails with no lower-level test failing gets the lower test added at the lowest layer that can show the failure; a production bug gets its failing test first, then the fix (Fowler/Vocke; owner ruling 2026-10-19).
- **TEST-33** The seat opens the approved board itself (LEAD-33) and writes the tests from its non-functional cards' expected results (DRAW-34) and its data schema, checking the brief's expected results against the cards and returning any difference as `BRIEF INCOMPLETE` (TEST-23); every build carries one settings test that fails when the repo's settings keys (sheet tab, table, JSON, env) differ in either direction from the setting keys in the schema's text copy (DRAW-53); live settings keys and values are the release suite's (REL-7) (owner ruling 2026-10-20).
- **TEST-30** After the first red/green round the seat may read the code for missed branches and adds tests in a labelled second pass; it adds tests only for correctness and the brief's requirements, never for every imaginable case.

## 1 · Untested risk

- **TEST-20** Every project has one test plan, written at kick-off beside the domain spec and never copied from another project without re-deciding matrix and depth, stating the target matrix (never "all"), the test types that apply and are skipped (TEST-14), what runs per push vs on a trigger (TEST-19 · ARCH-34), the environments (OPS-17), the declared numbers (ARCH-28, ARCH-24), the security test list (SEC-31), the release suite's contract list and journeys (REL-1), the pre-production gate (QA-18), a carried-or-not-carried verdict with reason on all 8 risk sections, and depth by stakes: planning or internal tool with no money → smoke + CRUD + deny path; live with real users, no money movement → full functional + API contract + cross-platform + load at the ladder trigger; money, credentials or client data → all of that plus the security abuse pass (SEC-32), idempotency on every value-bearing path and a rehearsed restore (OPS-14); client-delivered → plus a handover pack (how to run the suite, what it proves, what it does not).
- **TEST-14** Every feature declares which test types apply — unit · integration · API contract · end-to-end UI · cross-platform · load (§6) · security abuse (§6) · data/CRUD (§3) — and states why each other type is skipped, never by omission.

## 2 · Wrong logic

Method: `rsot/knowledge/test-harness-patterns.md`.

## 3 · Lost or corrupted data

- **TEST-15** CRUD is tested against a real database, never mocked, each operation verified by reading the row back (never a 200 alone), plus the integrity paths: constraint violations rejected, atomic claims not double-claiming under concurrency, replace-writes not losing data when the second call fails, and migrations applied and re-applied idempotently (DB-1..10, QA-1).

## 4 · Broken contracts

- **TEST-16** Every endpoint is tested through its real HTTP surface against the captured payload (BE-23): success shape, every error path with exact status and exact message, auth deny cases, malformed and missing-field inputs, and idempotency, the same request twice producing one effect (BE-18).
- **TEST-4** Assert exact status and exact error string for every return path, success and every failure (BE-28).
- **TEST-7** CI runs on mocks with zero real secrets; live checks never enter it, they are the release suite (REL-9).
- **TEST-8** Auth tests run the negative case (SEC-3) and record the observed success and failure strings.

## 5 · Broken for the user

- **TEST-17** Interface tests drive the real user journey with browser automation, anchored on stable element ids and never on text matching, across the declared target matrix (browsers, viewports, OS versions where behaviour differs, FE-13/FE-14) plus at least one throttled network/CPU pass (FE-21), every step verified by its outcome, never by the click having happened (QA-1).

## 6 · Collapse & breach

- **TEST-18** Load, latency and pen tests are planned from what was declared, never invented — load at the scale-ladder trigger (ARCH-24) for every volume risk, latency at the declared delivery metric's stated percentile (ARCH-28), the abuse pass on the domain's security test list (SEC-32) — and all run before production exposure (QA-18) with observed numbers reported for QA-9.
- **TEST-13** Security tests (abuse cases, deny paths, injection, secret leaks) are the security tester's (SEC-40); this seat's CI runs them as their own suite on every push (TEST-27).

## 7 · False green

- **TEST-3** Suites are idempotent across re-runs: seeded once out-of-band, per-run-unique identities, and a documented clean-state reset.
- **TEST-5** A return path that is missing, opaque or not independently testable is flagged `HOLE` for a human reviewer, never reported as a fail and never hidden (QA-8).
- **TEST-6** Never assert against what the environment would overwrite.
- **TEST-11** Typecheck plus a clean build is the floor, never the proof; TEST-clean and QA-proven are separate gates.

## 8 · Rot

- **TEST-19** Load and abuse suites run on a stated trigger named in the spec — pre-release, nightly or before production exposure — never ad hoc (ARCH-34); the live contract and journey suite runs on REL-9's fixed triggers.
- **TEST-9** A finding in product code goes back to the builder through the lead (TEST-25), never patched by this seat.
- **TEST-10** A dry-run states in a banner exactly what it still touches (claims, writes, spends), and a rehearsal never pushes work past a human review gate.
