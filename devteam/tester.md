# Dev team — Tester / Script Tester (TEST)

Type: Law · loads when the task writes or runs tests, harnesses or CI. This seat builds and runs the machinery that produces evidence; QA judges whether the delivered thing matches the design (`qa.md`); sections are ordered by the risk each test class defends against, and every one of the 8 risks is assessed on every project. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/tester-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 1 · Untested risk

- **TEST-20** Every project has one test plan, written at kick-off beside the domain spec and never copied from another project without re-deciding matrix and depth, stating the target matrix (never "all"), the test types that apply and are skipped (TEST-14), what runs per push vs on a trigger (TEST-19 · ARCH-34), the environments (OPS-17), the declared numbers (ARCH-28, ARCH-24), the security test list (SEC-31), the pre-production gate (QA-18), a carried-or-not-carried verdict with reason on all 8 risk sections, and depth by stakes: planning or internal tool with no money → smoke + CRUD + deny path; live with real users, no money movement → full functional + API contract + cross-platform + load at the ladder trigger; money, credentials or client data → all of that plus the security abuse pass (SEC-32), idempotency on every value-bearing path and a rehearsed restore (OPS-14); client-delivered → plus a handover pack (how to run the suite, what it proves, what it does not).
- **TEST-14** Every feature declares which test types apply — unit · integration · API contract · end-to-end UI · cross-platform · load (§6) · security abuse (§6) · data/CRUD (§3) — and states why each other type is skipped, never by omission.

## 2 · Wrong logic

Method: `rsot/knowledge/test-harness-patterns.md`.

## 3 · Lost or corrupted data

- **TEST-15** CRUD is tested against a real database, never mocked, each operation verified by reading the row back (never a 200 alone), plus the integrity paths: constraint violations rejected, atomic claims not double-claiming under concurrency, replace-writes not losing data when the second call fails, and migrations applied and re-applied idempotently (DB-1..10, QA-1).

## 4 · Broken contracts

- **TEST-16** Every endpoint is tested through its real HTTP surface against the captured payload (BE-23): success shape, every error path with exact status and exact message, auth deny cases, malformed and missing-field inputs, and idempotency, the same request twice producing one effect (BE-18).
- **TEST-4** Assert exact status and exact error string for every return path, success and every failure (BE-28).
- **TEST-7** CI runs on mocks with zero real secrets; a live smoke test is its own gated job, never mixed into the mock suite.
- **TEST-8** Auth tests run the negative case (SEC-3) and record the observed success and failure strings.

## 5 · Broken for the user

- **TEST-17** Interface tests drive the real user journey with browser automation, anchored on stable element ids and never on text matching, across the declared target matrix (browsers, viewports, OS versions where behaviour differs, FE-13/FE-14) plus at least one throttled network/CPU pass (FE-21), every step verified by its outcome, never by the click having happened (QA-1).

## 6 · Collapse & breach

- **TEST-18** Load, latency and pen tests are planned from what was declared, never invented — load at the scale-ladder trigger (ARCH-24) for every volume risk, latency at the declared delivery metric's stated percentile (ARCH-28), the abuse pass on the domain's security test list (SEC-32) — and all run before production exposure (QA-18) with observed numbers reported for QA-9.
- **TEST-13** Every abuse case on the security test list (SEC-31/32) becomes an executable check asserting the deny with exact status and exact message (TEST-4, TEST-8, TEST-9).

## 7 · False green

- **TEST-3** Suites are idempotent across re-runs: seeded once out-of-band, per-run-unique identities, and a documented clean-state reset.
- **TEST-5** A return path that is missing, opaque or not independently testable is flagged `HOLE` for a human reviewer, never reported as a fail and never hidden (QA-8).
- **TEST-6** Never assert against what the environment would overwrite.
- **TEST-11** Typecheck plus a clean build is the floor, never the proof; TEST-clean and QA-proven are separate gates.

## 8 · Rot

- **TEST-19** Live, load and abuse suites run on a stated trigger named in the spec — pre-release, nightly or before production exposure — never ad hoc (ARCH-34).
- **TEST-9** Findings from a test pass are recorded for the reviewer to judge, never patched inline.
- **TEST-10** A dry-run states in a banner exactly what it still touches (claims, writes, spends), and a rehearsal never pushes work past a human review gate.
