# Dev team — QA (QA)

Type: Law · loads whenever a claim is about to be made: "it works", "it's connected", "Done". QA owns the verdict, whether what was delivered matches what was designed, documented and promised; it runs no test and no live call (owner ruling 2026-10-28): the three test seats produce the evidence in their logs (`tester.md`, `release-tester.md`, `security.md` §9), QA proposes the project's QA table, fills it from those logs and reviews the code, and the Done states are Linear law (LIN-7, LIN-15, LIN-19). Sections are the gates in the order QA runs them, §0 defines what is measured before any gate is judged, and a finding names the gate it failed. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/qa-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 0 · The QA table

- **QA-17** Per project QA proposes one QA table of measurable quality-assurance metrics with the columns # · Quality attribute · Measurable metric · Assurance class · Required standard (this project) · Trade-off (why this level) · Evidence (which seat's log) · Measured · Pass/Fail; assurance classes are **A must hold** (a miss blocks go-live) · **B should hold** (a miss blocks unless the owner accepts it as a named risk, LEAD-29) · **C reported** (measured, never blocks); the attributes always considered are functional correctness · contract fit · end-to-end journeys · security · performance · load capacity · data integrity · recoverability · alerting · cost, a non-applicable one staying as a row with its reason; every project declares its own trade-offs through the required standard and the class, taken from the approved board (ARCH-28 · ARCH-33), never from QA's opinion (template: `qa-reference.md`; owner ruling 2026-10-28).
- **QA-26** The QA table's flow is fixed: QA writes its plan against the approved docs and file-system tree board (DRAW-38, `rsot/rules/drawing/docs-tree.md`) and proposes the metrics, classes and standards from it and every other approved board of the project → the owner approves the table → the test seats' logs fill Measured → QA marks Pass/Fail → go or no-go is read from the table (QA-18); QA runs no test and no live call and never re-runs a suite, and a metric no log measures is unmet, sent back to the lead naming the seat whose log should carry it (owner ruling 2026-10-28).

## 1 · Gate 1 — it ran (read from the logs)

- **QA-3** Anything spanning two or more domains or components has its id parity proven across the hop by the release tester's contract check (REL-2), read by QA from that run's log before the handoff is called wired (the end-to-end proven run is LIN-19) (owner ruling 2026-10-28).
- **QA-4** After deploy, the release suite's smoke result (REL-9) in its run log is the liveness evidence (CFW-1); QA opens no URL itself (owner ruling 2026-10-28).
- **QA-20** Before Done on anything touching shared code, schema, config or a shared component, QA checks that the logs cover the existing flows that depend on it, the consumers the contract card and laneindex name (ARCH-30/MGR-10), with shared surfaces (the kit, the component library, a table with multiple readers, a changed event payload) named explicitly; a consumer no log covers is a finding to the lead (owner ruling 2026-10-28).

## 2 · Gate 2 — it matches the design

- **QA-10** Verify the build against the approved design, not just the ticket: the flow matches the diagram (DRAW-29), the declared logic pattern was implemented (ARCH-31), the contract card still describes what the domain consumes, owns and emits (ARCH-30), and a working build that diverges is a finding, never a pass.
- **QA-12** [CI] Check every build for reuse versus duplication, mechanically first: a clone detector (jscpd) runs on the diff and any clone above threshold goes back to the builder to merge into the existing home, the code index check (`codeindex.py --check`) fails on a file or export with no row, a row with no file, or a touched file with an unwritten Provides cell (FILE-8), then the review reads what tools cannot: shared component used or a variant hand-rolled (FE-10/UX-2), kit composed or bypassed (BE-24), existing table or field extended or a near-duplicate created (DB-13).
- **QA-13** Names in code, schema, UI copy, Linear titles and documentation match `rsot/knowledge/vocabulary.md` and the global card §5 codes on all four surfaces (LEAD-1/2), folder structure and file naming align across all five builders' output (database · backend · frontend · uxui · devops), and the documentation (laneindex, SPEC, contract card) describes the structure that actually exists on disk.
- **QA-24** [review] Read the diff for settings: a new key in a settings sheet, table, JSON or env file that the approved data schema does not draw (DRAW-53), or a cap, wait, alarm, retry or gate the brief does not name (LEAD-36), fails this gate and goes back to the builder unless the brief quotes the owner's board approval for it (owner ruling 2026-10-20).

## 3 · Gate 3 — it meets the documented promises

- **QA-9** The design's declared delivery metrics (ARCH-28: p95 latency, throughput, error rate, cost per run, memory or payload ceiling) are rows of the QA table, measured from the release tester's run logs (elapsed time, load size, output, REL-13); an unmeasured metric is unmet, and an undeclared one is a design gap sent back, never a pass (owner ruling 2026-10-28).
- **QA-11** The infra, caching, degradation and security requirements as documented are rows of the QA table, each filled from the log of the seat that exercised it, never assumed (the documented-controls rows: `qa-reference.md`; owner ruling 2026-10-28).
- **QA-14** Every failure path in the failure map produces its stated signal, fail direction and blast radius (ARCH-18/29 · BE-33), errors carry the human-readable envelope (BE-28), and nothing fails silently (BE-7/BE-9), read from tester 1's error-handling tests and the end-to-end scenario rows (REL-13), a deliberately broken alarm included (owner ruling 2026-10-28).
- **QA-6** Done requires the runtime proof and, for every setting the approved data schema draws (DRAW-53), a lifecycle a human can create, edit and disable from a surface; a feature with no drawn setting needs none (owner ruling 2026-10-20).

## 4 · Gate 4 — it was tested to the plan's depth

- **QA-16** Judge testing against the project's test plan (TEST-20) and its three owner-approved tables (REL-13 · SEC-44 · QA-17): from the logs, every expected result in the brief (LEAD-34) has a test asserting that exact metric with its red run on record (TEST-24 · SEC-40), no test was weakened, skipped or edited without a brief revision since the red run, gaps are named both ways (brief line with no test, test with no brief line), and every deviation is recorded as accepted risk (LEAD-29), never discovered afterwards (owner ruling 2026-10-28).

## 5 · Gate 5 — readiness & reporting

- **QA-18** Before first exposure the go or no-go is read from the approved QA table: every class A row Pass, every class B row Pass or accepted by the owner as a named risk (LEAD-29), class C rows reported, with the release suite green against the live surfaces as a class A row (REL-9) and evidence named on every row (the green checklist: `qa-reference.md`; owner ruling 2026-10-28).
- **QA-15** Every finding is addressed and actionable, so the reader can verify and act on it without a follow-up question (the six fields, ordering, blocks-Done flag: `qa-reference.md`).
- **QA-5** "VERIFIED" carries a date and a sample size, and never generalizes from a null.
- **QA-7** Prove the fix, name what is still unproven, label OBSERVED and INFERRED in every finding, and when evidence is gone say so and fix the evidence gap in the same pass.
- **QA-8** Every check states what it does NOT prove.
- **QA-21** Verify client-delivered work against the criteria the client agreed, never our internal definition of good, and hand over an evidence pack stating what was tested, what was observed, what is explicitly out of scope and any accepted risk with its disclaimer posture (learnrule #30).

## 6 · When a gate fails

- **QA-19** A failed gate goes to the seat that owns it (QA-15) with the fix expectation, on return QA re-reads the logs for the finding and anything the fix could have touched (QA-20), a disputed finding is settled by the lead against the approved architecture design (LEAD-21.5) and recorded in the ticket with both positions, never escalated to the owner or quietly abandoned by either side, and the same gate failing three rounds running stops iteration and escalates as a design or brief problem.
- **QA-22** When the same class of finding appears a second time, across features or across repos, QA raises it as a learnrule candidate instead of filing the same finding again.
