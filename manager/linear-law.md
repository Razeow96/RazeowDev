# Linear law — one home for reading, writing, and keeping it lean

Org-wide. Every skill and session that touches Linear follows this file and never restates it. Born 2026-08-28 from learnrule #38 (whose row now points here). Authority for structure: the org registry; authority for Linear mechanics: this file.

## 1 · Structure

- **One team — `Razeow`** — holds every issue in the org. **Project = company/venture** (Executive, Content OS, Zeow, Creator Fleet, …). **Cycle = the week** (1 week, Monday start MYT, no cooldown). Docs live in their project.
- Labels: **type** (`build` / `plan` / `maint` / `learn`) · `laura` (her proposal inbox) · `hold` (deliberately parked) · objective labels (taxonomy pending regeneration to the an internal ticket tree).

## 2 · Read law

- **LIN-R1** The sprint pull is **team + `isActive` cycle**. Never filter a sprint pull by project — routed sessions vanish silently.
- **LIN-R2** The cycle number is **printed from the pull, never remembered**. Linear's counter is machine-fixed; nobody argues with it or carries a sprint number in their head.
- **LIN-R3** Board = **counts per state only**. Titles are listed in exactly one place: the sprint table.
- **LIN-R4** In the sprint table, issues created before the active cycle started are carry-over → one `CARRY-OVER: N open` line, never rows. Done rows age out after 3 days.
- **LIN-R5** Every **automated** consumer declares a bounded slice and stays inside it: Laura's brief = one current-cycle page; M0.4 ingest = aggregates only (counts + stale + in-review), never issue bodies. Growth lands on the archive, never on the daily pull.

## 3 · Write law

- **LIN-W1** An issue is **born with its cycle**: active cycle if it's this week's work, `next` when filed at Sunday planning, none if it's a pool/backlog item. Type label at creation. Due dates only for real external deadlines — day-slots ride the title.
- **LIN-W2** States tell the truth **as work happens**; every session close reconciles every touched issue (Done · In Review · In Progress · Duplicate · Canceled — MGR-20/21, learnrule #25).
- **LIN-W3** **Done requires the proven-run evidence** as a comment on the issue. No evidence, no Done.
- **LIN-W4** Route, never duplicate. Verdict map: do-now → Todo + current cycle · project → move · hold → Backlog + `hold` · drop → Canceled. **Never delete.**
- **LIN-W5** A parked block (e.g. a deferred launch) is parked **explicitly**: Backlog + `hold`, reason in the description — never left "In Progress" to rot. A board that lies taxes every brief.

## 4 · Lean settings (Team settings — the standing configuration)

| Setting | Value | Why |
|---|---|---|
| Auto-archive closed issues | **3 months** | archived issues drop out of every pull — ingest stays flat forever; still searchable/restorable |
| Cycles → auto-add active issues to current cycle | **ON** (started + completed) | work that starts or finishes belongs to the week it happened — the setting enforces LIN-W2 |
| Cycles → rollover | automatic (Linear default) | open issues roll forward; the Sunday review is where they're killed or kept (LIN-R4 keeps them out of briefs) |
| Auto-close stale issues | **OFF** | the Backlog is a deliberate idea pool (/dump); staleness gets a human verdict at /routetask, not a robot's |
| Team timezone | Asia/Kuala_Lumpur | cycle boundaries = Raze's Monday |

## 5 · Capabilities deliberately NOT adopted (revisit only on proven need)

Triage inbox (solo — /dump is the inbox) · estimates & SLAs (no team to coordinate) · sub-teams (5 projects fit one team) · releases pipelines (no CI/CD yet — an internal ticket first) · recurring issues (Sunday review recreates its own skeleton) · issue templates (adopt when issue shapes repeat enough to be worth it).

`LINEAR LAW v1 LOADED`
