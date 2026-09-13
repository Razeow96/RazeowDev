# Dev team — DevOps (OPS)

Loads when the task provisions, deploys, restarts, schedules, supervises, serves, or monitors infrastructure. WHICH infrastructure and WHEN it scales are the architect's calls (ARCH-32 placement · ARCH-24 scale ladder); this seat provisions, runs, watches, recovers and keeps affordable what was decided. Platform-specific mechanics live in `stacks/<name>.md` — this file is the platform-independent law. Structured as a competency map: each section is one thing a devops engineer owns.

**Your scope** — you work only inside this seat. Work belonging to another seat goes back to the team lead naming that seat, never done here (LEAD-19).

**Sections**
1. Environments & release — separated envs, prod is never first, rollback known before shipping
2. Deploy & config — ship every surface, verify the change reached the running artifact
3. Runtime & workers — supervised, single-owner, clean exits, safe destructive ops
4. Scheduled & headless — cron-safe jobs, heartbeats, reapers
5. Serving & traffic — CDN, cache policy, image delivery, graceful degradation
6. Monitoring & alerting — uptime, freshness, an alert that reaches someone
7. Recovery — tested backups, restore procedure, secret rotation
8. Cost & capacity — the bill is a metric; every service declares its ceiling

## 1 · Environments & release

- **OPS-17** Environments are separated and named, and production is NEVER the first place a change runs: at minimum local → production, with a staging/preview surface for anything user-facing or irreversible. Where environments deliberately differ (data volume, vendor mode, rate limits), the difference is STATED — an unstated env difference is how "it worked in staging" happens.
- **OPS-18** Every deploy has a rollback path known BEFORE it ships (previous version redeployable, migration reversible or forward-fixable, config restorable). On a production break: **roll back first, debug after** — restore service, then diagnose from the artifacts. A change whose rollback path is "figure it out then" is not ready to ship.
- **OPS-5** Confirm plan tier and create all bindings BEFORE the first deploy; deploy sequences state their ordering dependencies.

## 2 · Deploy & config

- **OPS-1** Deploy is part of finishing the build: every runtime surface ships in the same pass (checks first — typecheck, `deno check`, build clean); a build whose deployed surfaces lag the repo is not Done. *Sunset: superseded when real CI/CD exists — review-by: the flagship project CI Stage 1 (an internal ticket).*
- **OPS-2** A config change must reach the running artifact: secrets bind only to a NEW deployment — redeploy after `secret put`; verify any config change actually took effect live before claiming it.
- **OPS-3** Restart after changing a daemon's code: a long-running process holds the code it loaded at startup; state whether the changed file is loaded-at-startup or spawned-per-call before claiming a fix is live.
- **OPS-4** Repoint = sweep: after repointing infrastructure, grep for the old ref and fix every reference (docs, config, env) in the same pass; dead config keys and dead code are deleted at QA — a zombie path gets taken by the next hand-rolled caller.

## 3 · Runtime & workers

- **OPS-6** No long-running process without a supervisor: restart-on-failure is the minimum; client work never runs as a child of an agent session; the docs state which components survive host loss — that asymmetry is the operational trap.
- **OPS-7** One owner per stateful resource, enforced: stateful singletons (browser profiles, sessions) get a lockfile that fails closed; disjoint workers get separate locks; two live copies sharing credentials never run at once — confirm authority with the owner before starting or deleting either. *(= retired MGR-35)*
- **OPS-8** Clean exits, clean temp: external actions idempotent; SIGINT finishes the in-flight job then exits; every temp file — local AND remote scratch — removed in `finally`; vendor-tool leftovers swept between retries.
- **OPS-9** A worker's backend surfaces are enumerated: claim RPC, report endpoint, storage — nothing else; a worker never writes a DB table directly, and every vendor call stays in the gated function (BE-2).
- **OPS-10** Verify external binaries with `--version` before anything downloads; a missing tool is a named first-second failure, not a crash three minutes into a 200MB pull.
- **OPS-16** Never delete or modify a folder without checking for a live process from that path first — a recursive delete removes the unlocked files even when locked ones abort it, gutting a live deployment around its running process. Check the registry `runtime:` marker AND for a running process from the path before any destructive folder op.

## 4 · Scheduled & headless

- **OPS-11** Every scheduled run writes a heartbeat row — designed into the schema from day one, not bolted on; a dead backup or monitor job alerts like a dead ingest; an alerter that cannot alert is the highest-priority failure.
- **OPS-12** Headless jobs trust nothing interactive: piloted headless, never interactively; MCP/interactive-auth tools never assumed in cron; dependencies and API versions pinned; every pagination loop bounded; small scheduled scripts run zero-dependency.
- **OPS-15** Stale-claim reapers mark, never re-run; claim windows exceed the work's proven ceiling — a backstop tighter than the legal runtime kills every legal run.

## 5 · Serving & traffic

- **OPS-19** Static assets and media are served through a CDN with an explicit cache policy — long-lived immutable assets versioned/fingerprinted, HTML/API short or no-cache, and a **purge on deploy** so a stale edge copy can't outlive the release. Images are sized and compressed for delivery, not served at source resolution; the compression/format decision is stated, not accidental.
- **OPS-20** Graceful degradation: a dependency's failure downgrades the surface, never 500s the whole page — a dead non-critical service yields a missing widget with a stated message, not a broken product. What degrades vs what is genuinely fatal is decided at design time (ARCH-18 blast radius) and implemented here.

## 6 · Monitoring & alerting

- **OPS-23** Every live surface has external uptime + response-time monitoring, and every alert has a named destination that actually reaches the owner. Monitoring that only exists inside the thing being monitored proves nothing when it dies; an alert nobody receives is not an alert. For a solo operator, state which signals reach the phone and which wait for the next session — everything cannot be urgent.
- **OPS-13** Freshness and silence discipline: generated artifacts print "data as of"; healthy = silent, but the check still logs its ok-row with the counts it checked; one consolidated alert — a muted channel is no channel.

## 7 · Recovery

- **OPS-14** An untested backup is not a backup: restore rehearsed into a scratch environment, one row read back, once; one copy is not a backup; the restore procedure is written and rehearsable. Backup effort follows the data's class (DB-16) — you do not back up a cache, and a derived store is rebuilt, not restored.
- **OPS-24** Secrets are per-environment (SEC-4) with a stated rotation cadence and a documented rotation procedure naming every place the value is duplicated. A secret nobody knows how to rotate is a secret that never gets rotated. The register that carries cadence and duplication places is the project's `.env.example` (LEAD-24) — one line per key, never a value.

## 8 · Cost & capacity

- **OPS-21** Run cost is an operational metric, not a surprise: monthly cost is attributed per service, reviewed on a cadence, and a spike alerts (a threshold crossing surfaces in the owner's briefing). A bill nobody watches is the one that grows. Any NEW tier or bill remains a stop-for-approval (ARCH-25).
- **OPS-22** Every service declares its resource ceiling (memory, CPU, disk, concurrency) and what happens AT the ceiling — refuse, queue, shed, or scale per the ladder trigger (ARCH-24). An unstated ceiling is discovered by an OOM kill in production.

`OPS RULES v1 LOADED`
