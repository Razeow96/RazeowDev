# Dev team — QA reference

Type: Law (reference templates for `qa.md`) · pulled through rulesindex when proposing or filling the QA table, issuing the go/no-go, or writing a finding row. Update: replace a row in place.

## The QA table (QA-17 · QA-26, owner ruling 2026-10-28)

QA proposes it per project from the approved board; the owner approves it before it is filled; Measured comes only from a test seat's log, never from a run by QA.

| # | Quality attribute | Measurable metric | Assurance class | Required standard (this project) | Trade-off (why this level) | Evidence (which seat's log) | Measured | Pass/Fail |
|---|---|---|---|---|---|---|---|---|

Assurance classes: **A must hold** (a miss blocks go-live) · **B should hold** (a miss blocks unless the owner accepts it as a named risk, LEAD-29) · **C reported** (measured, never blocks).

Attributes always considered, one row each at least (a non-applicable one stays as a row with its reason): functional correctness (tester 1) · contract fit (tester 2) · end-to-end journeys (tester 2) · security (security tester) · performance (tester 2's elapsed times) · load capacity (tester 2's load sizes) · data integrity (tester 1's rows read back) · recoverability · alerting (tester 2's broken-alarm scenario row) · cost.

## The documented-controls rows (QA-11)

Each is a QA table row filled from the log of the seat that exercised it:

- cache headers are actually set and purge on deploy (OPS-19)
- degradation actually degrades instead of 500ing (OPS-20)
- rate limits actually limit (SEC-15)
- backups actually restore (OPS-14)
- the failure map's guards actually guard (ARCH-29)

## The go/no-go green checklist (QA-18)

Read from the approved QA table before first exposure:

- every gate green (Gates 1–4)
- every class A row Pass; every class B row Pass or owner-accepted as a named risk (LEAD-29); class C rows reported
- release suite green against the live surfaces (REL-9)
- **fallbacks and degradation proven by a test seat exercising them**, not by reading the code (OPS-20 · SEC-22's rotation drill · OPS-14's restore)
- the rollback path confirmed (OPS-18)
- every known gap (LEAD-29)

## The finding row (QA-15)

Every reported item carries six fields:

| Field | Carries |
|---|---|
| gate | the gate it failed |
| address | the domain/subdomain §6 code it belongs to (LEAD-1) |
| seat | who owns the fix — database · backend · frontend · uxui · devops · security |
| rule | the rule ID it violates |
| location | file:line, or the screen |
| evidence | what the build does versus what the design or rule requires (global card §3) |

Findings are ordered most-severe first, and each states whether it blocks Done.


