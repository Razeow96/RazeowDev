# Dev team — The dispatch brief

TEMPLATE class (structure-law): read when writing a dispatch brief for a builder or reviewer agent, or when copying a designed build's handoff lines out of the design package; the law lives in `teamlead.md` (LEAD-21 · §2).

## What a complete brief carries

A dispatched agent cannot see the lead's conversation — it gets only the brief. A complete brief carries:

- the locked spec / Linear ticket
- the files in scope
- the acceptance criteria
- **the work's address per LEAD-2** (code · folder · Linear title)
- every piece of repo context the seat needs — declared stack, project invariants, seams, accepted risks, and any repo `learnrules.md` gotcha that touches the work

## The designed-build line set — an OPTIONAL checklist

For a build that came through the design pipeline (LEAD-20), the lead may copy this line set out of the design package. It is **never authored at dispatch time and never a mandatory form** — an empty line means the design package did not carry it, which is itself a finding to send back, not a blank to invent.

| Line | Carries |
|---|---|
| ADDRESS | the §6 code · folder · Linear title (LEAD-1/2) |
| DESIGN | the approved diagram/spec this build implements (ARCH-1) |
| SCOPE | what is in, and explicitly what is out |
| ACCEPTANCE | the stated Done, verbatim from the ticket |
| PATTERN | the declared logic pattern (ARCH-31) |
| FAILURE | the failure-map rows this work owns (ARCH-29) |
| SECURITY | the abuse cases / critical-function rows that touch it (SEC-30/31) |
| CONTRACT | the API/event/data contract at the boundary (ARCH-10/13) |
| NUMBERS | the declared delivery metrics and their targets (ARCH-28) |
| UPSTREAM | the upstream seat's ACTUAL output, captured — not its promise |
| CREDENTIALS | where each needed credential lives (never the value) — every key the lane consumes already has a row in `.env.example` and a value in `.env` (LEAD-24), else the brief is not sent |

`DISPATCH BRIEF TEMPLATE LOADED`
