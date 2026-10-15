# Dev team — DevOps (OPS)

Type: Law · loads when the task provisions, deploys, restarts, schedules, supervises, serves, or monitors infrastructure. WHICH infrastructure and WHEN it scales are the architect's calls (ARCH-32 placement · ARCH-24 scale ladder); this seat provisions, runs, watches, recovers and keeps affordable what was decided, and platform mechanics live in `stacks/<name>.md`. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/devops-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 1 · Environments & release

- **OPS-17** Environments are separated and named, production is never the first place a change runs (at minimum local → production, plus staging or preview for anything user-facing or irreversible), and every deliberate environment difference is stated.
- **OPS-18** Every deploy has a rollback path known before it ships, and on a production break the seat rolls back first and debugs after (code only; migrations are append-only, DB-5).
- **OPS-5** Plan tier is confirmed and all bindings created before the first deploy, and deploy sequences state their ordering dependencies.

## 2 · Deploy & config

- **OPS-1** Deploy is part of finishing the build: every runtime surface ships in the same pass, checks first (typecheck, `deno check`, build clean); sunset: superseded when real CI/CD exists, review-by the flagship project CI Stage 1 (an internal ticket).
- **OPS-25** A GitHub workflow, CI job or deploy pipeline is proven before it runs on a push: after creating or editing one, and before any push or merge that triggers one, its steps run locally end to end against the real tree (every copy path, every gate, every script), all green; after the push, its own on a deploy approval or the owner's once reported, the seat reads the run result (`gh run list`, `gh run view --log-failed`) before the task is reported done, and the green run is its Done evidence (LIN-15), and a failed run is a bug fixed in the same sitting, never a line left in the owner's inbox.
- **OPS-2** A config change must reach the running artifact: redeploy after `secret put` and verify any config change took effect live before claiming it.
- **OPS-3** Restart a daemon after changing its code, and state whether the changed file is loaded at startup or spawned per call before claiming a fix is live.
- **OPS-4** After repointing infrastructure, grep for the old reference and fix every one (docs, config, env) in the same pass (MGR-12).

## 3 · Runtime & workers

- **OPS-6** No long-running process runs without a supervisor (restart-on-failure minimum), client work never runs as a child of an agent session, and the docs state which components survive host loss.
- **OPS-7** (global card §3) Stateful singletons get a lockfile that fails closed, disjoint workers get separate locks, and two live copies sharing credentials never run at once; confirm authority with the owner before starting or deleting either.
- **OPS-8** External actions are idempotent, SIGINT finishes the in-flight job then exits, every temp file (local and remote scratch) is removed in `finally`, and vendor-tool leftovers are swept between retries.
- **OPS-9** A worker's backend surfaces are enumerated (claim RPC, report endpoint, storage, nothing else), and a worker never writes a DB table directly (BE-2).
- **OPS-10** External binaries are verified with `--version` before anything downloads.
- **OPS-16** Never delete or modify a folder before checking the registry `runtime:` marker and for a running process from that path.

## 4 · Scheduled & headless

- **OPS-11** Every scheduled run writes a heartbeat row designed into the schema from day one, and a dead backup or monitor job alerts like a dead ingest.
- **OPS-12** Headless jobs run piloted headless, never assume MCP or interactive-auth tools in cron, pin API versions (dependencies: SEC-20), bound every pagination loop, and run small scheduled scripts zero-dependency.
- **OPS-15** Stale-claim reapers mark, never re-run, and claim windows exceed the work's proven ceiling.

## 5 · Serving & traffic

- **OPS-19** Static assets and media are served through a CDN with an explicit cache policy (immutable assets fingerprinted, HTML and API short or no-cache) and a purge on deploy; the image compression and format decision is stated (FE-15).
- **OPS-20** A dependency's failure downgrades the surface with a stated message, never 500s the whole page, and what degrades versus what is fatal is decided at design time (ARCH-18).

## 6 · Monitoring & alerting

- **OPS-23** Every live surface has external uptime and response-time monitoring, every alert has a named destination that reaches the owner, and which signals reach the phone versus wait for the next session is stated.
- **OPS-13** Generated artifacts print "data as of", a healthy check stays silent but logs its ok-row with the counts it checked, and alerts are consolidated into one.

## 7 · Recovery

- **OPS-14** A backup counts only when one restore into a scratch environment has read a row back, more than one copy exists, and the restore procedure is written; backup effort follows the data's class (DB-16).
- **OPS-24** Secrets are per-environment (SEC-4) with a stated rotation cadence and a documented rotation procedure (leak response order: SEC-22), registered with cadence and duplication places in the project's `.env.example` (CRED-2 · CRED-1).

## 8 · Cost & capacity

- **OPS-21** Monthly run cost is attributed per service, reviewed on a cadence, and a threshold crossing surfaces in the owner's briefing.
- **OPS-22** Every service declares its resource ceiling (memory, CPU, disk, concurrency) and what happens at the ceiling: refuse, queue, shed, or scale per the ladder trigger (ARCH-24).
