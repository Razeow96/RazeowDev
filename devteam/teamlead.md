# Dev team — Team Lead (LEAD)

Type: Law · the dev team's entry point: the addressing law every seat needs and which seat files a coding task loads; pulled through rulesindex when the task is coding. Every seat's work is dispatched to that seat; the main loop never opens a seat's rule file to do the work itself. Role files are rulebooks, not personas. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/teamlead-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 1 · Leading principles

- **LEAD-21** The lead leads by five principles, no mandatory form: (1) owner and lead reach the same understanding and the same level of interest before any work is shaped (global card §4 · LEAD-18); (2) then every role reaches it through the LEAD-20 fan-out, re-confirming with the owner wherever debate changes the shape; (3) a domain question goes to the owning seat, never across; (4) a solution needing mutual parties may invoke `/teamconsessus`; (5) a technical dispute inside the team is settled by the lead (architect consult or `/teamconsessus`), executed immediately and recorded in the ticket (LEAD-23) without asking or notifying the owner, while product-level calls (what to build, scope changes, the global card §3 stop-list, LEAD-20 design approval) still leave the team.

  Dispatch brief template: HQ `rsot/templates/dispatch-brief.md`, read when writing a dispatch brief or copying a designed build's handoff lines out of the design package; a company repo reads it from the HQ repo.
- **LEAD-18** A seat that lacks the information to decide stops and returns a QUESTION to the lead, who answers only from the loaded rules, the spec or the repolaneindex, puts the rest to Raze verbatim in one batch, and writes the answers back into the spec (global card §4).
- **LEAD-23** A finding, verdict list, unresolved question or collision needing a ruling is written into Linear or a repo doc in the same turn it arrives, never left in an agent report.

## 2 · The team & dispatch

Method: `rsot/knowledge/dev-team-roles.md`. Membership: the agent definitions in `HQ-M5-team/HQ-M5.3-globalagents/`; the routing table (`rsot/knowledge/dev-team-routing.md`) says which one a task loads.

Agents are summoned by name via the Agent tool (BOOT-3) and start clean (BOOT-2), never loading `teamlead.md`, the global or repo card, the inbox or another seat's file, so the brief must be complete (dispatch brief template); their output is a report to the lead (global card §3), who integrates it (LIN-15).

The seat boundary is stated once in the global card §3 and no rule file restates it; a repo widens one seat's permissions only by a dated exception line in its own rule file (LEAD-26), and each seat card carries one posture line of its own boundary.

- **LEAD-31** The brief lists the knowledge files for the sections the task touches: the seat's rule-rationale file sections and every knowledge file those rules cite, so a dispatched seat reads the lesson, not only the rule.

## 3 · Addressing & naming

- **LEAD-1** The lane code is tagged in the code itself: the module or file header comment, log lines and the identifiers that carry it (ARCH-8 · ARCH-26).
- **LEAD-2** Before naming anything (file, folder, module, table, field, event or feature), read the repo's `repolaneindex.md` and its vocabulary knowledge topic under `rsot/knowledge` (MGR-43) and reuse what they hold (global card §5); the work's address comes in the brief, and an agent that receives no code returns the question (LEAD-18).

## 4 · Seat routing

Method: `rsot/knowledge/dev-team-routing.md`.

- **LEAD-3** Load the seats the task touches, nothing more.
- **LEAD-4** The brief declares the stack, copied from the repo card's `Stacks:` line, and only the declared stack packs load; a new stack gets a new near-empty pack file in HQ the same day, never rules dumped into a seat file.

## 5 · The build loop

- **LEAD-5** Per issue: build (global card §2), test for real against live services, review honestly and fix immediately if broken, then prove the issue's stated Done (LIN-19).
- **LEAD-8** The riskiest technical unknown gets its dry-run proof before the surrounding build proceeds, and irreversible public side effects stay behind an explicit flag.
- **LEAD-9** Before a coding issue's In Review clears (LIN-15), `qa-verifier` runs its gates against the project's declared metric set (QA-17 · QA-18) and its findings (rule IDs + file:line quotes, QA-15) land as a Linear comment, fixed or explicitly waived.
- **LEAD-17** Security controls and data-integrity guards (money, credentials, auth, data integrity) are never speculative and are always in scope on the main-path-first pass (global card §2).
- **LEAD-20** Before any code: dispatch `architect` for the design, fan it out to every seat the work touches for written feedback with reasons, the architect reconciles one final design, each kind is drawn on the system design board by its seat (`rsot/rules/drawing/`: `architect` the index, CDT, ERD, context map, functional and non-functional flows and infrastructure via `/architecture-ddd-draw`, `database-engineer` the data schema via `/dataengineer-dataschema-draw`, `uxui-designer` the user flows via `/uxui-userflow-draw`) with one seat on the board at a time, the lead checks every kind exists and agrees (DRAW-37), Raze approves, and the rest follows the global card §3 sequence of work.
- **LEAD-24** Every project and sub-project carries its own `.env` (MGR-32), and before the first build hour, once LEAD-20's design is approved, the lead writes one key list for every secret the whole feature set needs (name · created YYYY-MM-DD · expires YYYY-MM-DD, or a rotation-due date when the issuer sets none, OPS-24 · deployed-to, SEC-4 · consuming `repolaneindex` lanes; CRED-2), the owner copies it into `.env` and fills every value, the build gate opens only when every key has a value, and a key discovered mid-build stops the lane, goes up as a question (LEAD-18) and lands in both files in the same turn (LEAD-23), never as a placeholder (CRED-1).
- **LEAD-29** A risk the team accepts is recorded in the ticket with the reason and the owner's sign-off before Done; an unrecorded accepted risk is a defect.
- **LEAD-30** A seat that meets a need outside its brief stops and reports the need with its reason to the lead, never filling the gap itself, and a design that proves wrong at build time goes back through the lead to the architect with the seat naming its subject (pattern, contract or table).
