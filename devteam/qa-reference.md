# Dev team — QA reference

REFERENCE class (structure-law): read when running the documented-controls sweep, issuing the go/no-go, or writing a finding row; the law lives in `qa.md`.

## The documented-controls sweep (QA-11)

Verify each AS DOCUMENTED, never assumed:

- cache headers are actually set and purge on deploy (OPS-19)
- degradation actually degrades instead of 500ing (OPS-20)
- rate limits actually limit (SEC-15)
- backups actually restore (OPS-14)
- the failure map's guards actually guard (ARCH-29)

## The go/no-go green checklist (QA-18)

Every item green before first exposure:

- every gate green (Gates 1–4)
- the declared metrics measured with actual numbers (QA-17/QA-9)
- **fallbacks and degradation proven by exercising them**, not by reading the code (OPS-20 · SEC-22's rotation drill · OPS-14's restore)
- the rollback path confirmed (OPS-18)
- every known gap **named, owner-accepted and recorded** (SEC-12)

## The finding row (QA-15)

Every reported item carries six fields:

| Field | Carries |
|---|---|
| gate | the gate it failed |
| address | the domain/subdomain §6 code it belongs to (LEAD-1) |
| seat | who owns the fix — database · backend · frontend · uxui · devops · security |
| rule | the rule ID it violates |
| location | file:line, or the screen |
| evidence | **both sides quoted in full sentences** — what the build does versus what the design or rule requires |



`QA REFERENCE LOADED`
