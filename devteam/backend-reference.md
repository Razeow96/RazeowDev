# Dev team — Backend reference

REFERENCE class (structure-law): read at review time, or when consulting the endpoint-category / kit inventory tables and the pattern-feedback protocol; the law lives in `backend.md`.

## Endpoint use-case categories (BE-26)

| Category | What it is |
|---|---|
| `screen-payload` | frontend interface, one aggregated call |
| `pull/poll` | periodic fetch |
| `real-time push` | live logs/status → WebSocket/SSE/subscription, NEVER faked with tight polling |
| `webhook` | no-retry caller |
| `worker` | retryable caller |
| `m2m` | machine-to-machine |

## Status codes — the worked example (BE-21)

A report response answers two questions separately — did the job succeed, and was the report recorded: `ok:false, recorded:true` = "logged, stop"; `recorded:false` = "send again".

## The backend kit — what is built once (BE-24)

- one error envelope + single top-level catch (the AppError pattern)
- the timeout/retry wrapper (BE-20)
- the gated fetch (BE-2)
- auth middleware + permission check
- input coercers (SEC-1)
- the run-log helper (BE-19)



## Pattern feedback, in both directions (BE-32)

- **Before building** — if the declared pattern does not fit the real judgment structure, push back with the reason BEFORE writing the loop.
- **After hitting reality** — a build-time discovery (a starved pool, a collapsed rotation, a shared resource with two owners) goes back to the architect as a pattern change with the better pattern named, never a quiet reshape mid-build.



## The boring-code review checklist (BE-31)

Good backend code is BORING. The review test:

- composed from the kit (BE-24)
- every branch returns or raises
- config read, never contained
- pure logic extracted and testable without infra (TEST-1)
- ids carried, never re-minted
- every literal owned by a constant/config
- one home per behavior

Bad code announces itself: a hand-rolled variant of something the kit has · a silent catch · a magic number · a copy-pasted twin · state held in the process · a fetch outside the gate · a failure without a name. Cleverness that adds a moving part is a net loss.

`BE REFERENCE LOADED`
