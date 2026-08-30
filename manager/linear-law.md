# Linear law

`LIN LAW v1 LOADED`

The one home for how every seat reads and writes Linear. Act-scoped law (structure-law §2): loaded by any skill or step that touches Linear — /start, /startceo, /status, /dump, /routetask, /weekreview(ceo), close sweeps, Laura's Linear surface. Skills cite LIN IDs and never restate them. Consolidated 2026-08-30 from learnrule #38 + six skills (an internal ticket).

## Reading

- **LIN-1 · Pull by team + cycle, never filter the query by project.** Sprint sessions are routed to their destination projects, so a project-filtered pull silently hides them. Scope is applied client-side after the pull (LIN-11).
- **LIN-2 · The cycle number comes from the live pull, never from memory.** Spoken sprint numbers are retired; Linear's machine counter is not argued with. Headers print `cycle N (from Linear)`.
- **LIN-3 · Carry-over compresses.** An issue in the active cycle whose `createdAt` predates the cycle's start is carry-over — one count line (`CARRY-OVER: N open`), never table rows.
- **LIN-4 · Done rows age out at 3 days** in sprint tables — older Done collapses to one count line. UNPROVEN and BLOCKED are never age-filtered.
- **LIN-8 · Linear beats `session.md`.** session.md is rewritten only at close, so mid-session it is stale; on any disagreement Linear wins and the drift is noted in one line.
- **LIN-9 · Mechanics:** `list_cycles` needs the team UUID; the big backlog pull has no cycle field — pull sprint issues by cycle id.
- **LIN-10 · "Today" = Raze's local day (UTC+8)**, derived from Linear timestamps (`completedAt`/`updatedAt`), never from chat memory.
- **LIN-11 · Scope by seat:** HQ surfaces (CEO skills, Laura) read org-wide; a subsidiary repo's surfaces keep only that repo's project rows.
- **LIN-12 · The five reads** (ages always printed): Backlog routed (waiting N days) · reroute queue = Backlog with no project · In Progress (since startedAt) · In Review (pending N days) · Done. Todo is kept as a state but is NOT a read surface — cycle membership answers "planned".

## Writing

- **LIN-5 · Born with its cycle.** This week's work → the active cycle; filed at Sunday planning → the `next` cycle. Type label at creation. Due dates only for real external deadlines.
- **LIN-6 · Carry-over moves** to the next cycle at planning — never duplicated.
- **LIN-7 · Verdict → state map (routing):** move to the destination project, never duplicate · do-now = Todo + active cycle · hold = Backlog + `hold` label · drop = Canceled, **only on Raze's explicit call, never deleted** · Done **only with the proven-run evidence link as a comment** · In Review = built, unproven · In Progress = started · Duplicate marks the surviving issue. States reconcile at every session close.
- **LIN-13 · Labels:** issue type rides the label (`build` / `plan` / `maint` / `learn`); objective labels are `obj-*` carrying the full objective names as maintained in the Brain; brain-dump capture is verbatim and **unlabelled** — labelling happens at /routetask, a session Raze calls, never on a schedule.

## Model record (settings, ruled 2026-08-28)

Statuses: the existing 7 (Backlog · Todo · In Progress · In Review · Done · Canceled · system Duplicate) — unchanged. Auto-archive: 1 month (Raze's UI setting). Done ≤ 14 days applies to **Laura's reads only** — manager/CEO surfaces keep their own rendering rules so pending work is never hidden from humans. Canceled + Duplicate: hidden from Laura's reads immediately, physically archived at 1 month. Auto-add active issues ON · timezone MYT · auto-close OFF.
