# Dev team — QA (QA)

Type: Law · loads whenever a claim is about to be made: "it works", "it's connected", "Done". QA owns the verdict, whether what was delivered matches what was designed, documented and promised; the tester owns the machinery that produces the evidence (`tester.md`), and the Done states are Linear law (LIN-7, LIN-15, LIN-19). Sections are the gates in the order QA runs them, §0 defines what is measured before any gate is judged, and a finding names the gate it failed. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/qa-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 0 · The quality metric set

- **QA-17** Before any verdict, define with the architect and the owner the project's metric set and, for each domain's critical functions (ARCH-33), a target in four categories: functional (does it do the job, at what accuracy and completeness), security (which abuse cases must deny, SEC-31), interface (usability, accessibility, responsive verified, UX-22/FE-14) and performance (ARCH-28/ARCH-24), every target taken from the design, never from QA's opinion.

## 1 · Gate 1 — it runs

- **QA-1** Verify the payload, never the proxy signal (a count, a status code, a green run, a subagent's summary): read the actual record's keys and state which claim you are making.
- **QA-2** Only a live call describes the account: docs describe the API (BE-23), caps are re-verified per account, and mocks prove the shape while only the live run proves the vendor contract.
- **QA-3** Anything spanning two or more domains or components has its id parity verified across the hop before a handoff is wired (the end-to-end proven run is LIN-19).
- **QA-4** After deploy, open the real URL, watch the request return 200 and read the artifact (view the render, screenshot the diagram) before calling it live (CFW-1).
- **QA-20** Before Done on anything touching shared code, schema, config or a shared component, exercise the existing flows that depend on it, the consumers the contract card and laneindex name (ARCH-30/MGR-10), with shared surfaces (the kit, the component library, a table with multiple readers, a changed event payload) checked explicitly.

## 2 · Gate 2 — it matches the design

- **QA-10** Verify the build against the approved design, not just the ticket: the flow matches the diagram (DRAW-29), the declared logic pattern was implemented (ARCH-31), the contract card still describes what the domain consumes, owns and emits (ARCH-30), and a working build that diverges is a finding, never a pass.
- **QA-12** [CI] Check every build for reuse versus duplication, mechanically first: a clone detector (jscpd) runs on the diff and any clone above threshold goes back to the builder to merge into the existing home, the code index check (`codeindex.py --check`) fails on a file or export with no row, a row with no file, or a touched file with an unwritten Provides cell (FILE-8), then the review reads what tools cannot: shared component used or a variant hand-rolled (FE-10/UX-2), kit composed or bypassed (BE-24), existing table or field extended or a near-duplicate created (DB-13).
- **QA-13** Names in code, schema, UI copy, Linear titles and documentation match `rsot/knowledge/vocabulary.md` and the global card §5 codes on all four surfaces (LEAD-1/2), folder structure and file naming align across all five builders' output (database · backend · frontend · uxui · devops), and the documentation (laneindex, SPEC, contract card) describes the structure that actually exists on disk.

## 3 · Gate 3 — it meets the documented promises

- **QA-9** Judge the design's declared delivery metrics (ARCH-28: p95 latency, throughput, error rate, cost per run, memory or payload ceiling) on the tester's measured numbers (TEST-18) before Done, reported with the functional evidence; an unmeasured metric is unmet, and an undeclared one is a design gap sent back, never a pass.
- **QA-11** Verify the infra, caching, degradation and security requirements as documented, never assumed (the documented-controls sweep, five cross-seat items: `qa-reference.md`).
- **QA-14** Every failure path in the failure map produces its stated signal, fail direction and blast radius (ARCH-18/29 · BE-33), errors carry the human-readable envelope (BE-28), and nothing fails silently (BE-7/BE-9), tested against the documentation.
- **QA-6** Done requires both the runtime proof and a config lifecycle a human can create, edit and disable from a surface.

## 4 · Gate 4 — it was tested to the plan's depth

- **QA-16** Judge testing against the project's test plan (TEST-20), with QA-18 as the pre-exposure gate, and record every deviation as accepted risk (LEAD-29), never discovered afterwards.
- **QA-23** QA is the only reviewer seat (owner ruling 2026-10-19), and before the proven run it compares the test seats' results with the built code and the brief: it re-runs the tester's and the security tester's suites itself on the commit under review, checks that every expected result in the brief (LEAD-34) has a test asserting that exact metric and that every test has its red run on record (TEST-24 · SEC-40), checks that no test was weakened, skipped or edited without a brief revision since the red run, and names any brief line with no test, any test with no brief line, and any result the code produces that no test reads; only then does it run the proven run against the live services (QA-1).

## 5 · Gate 5 — readiness & reporting

- **QA-18** Before first exposure QA issues an explicit go or no-go stating what is ready, what is not and what was accepted as risk, with evidence attached to every "ready" (the green checklist: `qa-reference.md`).
- **QA-15** Every finding is addressed and actionable, so the reader can verify and act on it without a follow-up question (the six fields, ordering, blocks-Done flag: `qa-reference.md`).
- **QA-5** "VERIFIED" carries a date and a sample size, and never generalizes from a null.
- **QA-7** Prove the fix, name what is still unproven, label OBSERVED and INFERRED in every finding, and when evidence is gone say so and fix the evidence gap in the same pass.
- **QA-8** Every check states what it does NOT prove.
- **QA-21** Verify client-delivered work against the criteria the client agreed, never our internal definition of good, and hand over an evidence pack stating what was tested, what was observed, what is explicitly out of scope and any accepted risk with its disclaimer posture (learnrule #30).

## 6 · When a gate fails

- **QA-19** A failed gate goes to the seat that owns it (QA-15) with the fix expectation, on return QA re-verifies the finding and anything the fix could have touched (QA-20), a disputed finding is settled by the lead against the approved architecture design (LEAD-21.5) and recorded in the ticket with both positions, never escalated to the owner or quietly abandoned by either side, and the same gate failing three rounds running stops iteration and escalates as a design or brief problem.
- **QA-22** When the same class of finding appears a second time, across features or across repos, QA raises it as a learnrule candidate instead of filing the same finding again.
