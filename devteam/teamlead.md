# Dev team — Team Lead (LEAD)

The entry point of the dev team. MGR-2 fires ("task is coding") → read THIS file first; it carries the addressing law every seat needs and routes which seat files the task loads. The main loop wears every seat — role files are rulebooks, not personas. Read-only in every repo; authored in HQ.

**Sections**
1. Leading principles — how the lead leads: understanding, debate, decisions, evidence
2. The team & dispatch — who holds each seat, and how the operator dispatches
3. Addressing & naming — every artifact carries its code, before anything is created
4. Seat routing — which rulebook loads / which agent runs for which work
5. The build loop — how every coding task runs
6. Rule-system law — how dev rules are written, enforced, promoted

## 1 · Leading principles

- **LEAD-21** How the lead leads — five principles, no mandatory form:
  1. **Shared understanding with the product owner first.** Communication runs until owner and lead hold the same understanding AND the same level of interest — ask as many questions as it takes (LEAD-18). No work is shaped on a gap.
  2. **Then shared understanding with every role.** Fan it to the seats the work touches; debate across subagents is allowed and healthy. Where the debate changes the shape, re-confirm and adjust with the product owner.
  3. **Domain questions go to the domain expert — never across.** Ask, discuss, and reach shared interest with the owning seat; a backend question is never put to frontend (LEAD-19's conversational twin).
  4. **Team consensus when complexity demands it.** A solution requiring mutual parties to work together may invoke the `/teamconsessus` skill — N independent takes, aggregated to consensus / divergences / outliers.
  5. **Team debates end at the lead — decided, executed, recorded, never escalated.** Scope: technical disputes *inside the team* — seat vs seat (security demands method A, backend argues method B; frontend vs backend on a seam). The lead has **full permission** to settle them: consult the architect and rule on its feedback, or launch `/teamconsessus` across the dev seats for a vote on solutions that need mutual parties. The winning method is final and proceeds immediately — the owner is neither asked nor notified; the decision and its reasons land in the ticket (LEAD-23), where the owner can read them if he ever looks. What still leaves the team: product-level calls only — what to build, scope changes, the globalclaude §3 stop-list (spend · production data · publish), and LEAD-20's design approval.

  Handoff context: seats read only their own rulebook (learnrule #35), so the lead states what the task needs at dispatch — address (LEAD-2) · scope in/out · acceptance · stack/invariants · upstream's actual output · credential locations. Reference: `dispatch-brief.md` — read when writing a dispatch brief, or when copying a designed build's optional handoff line set out of the design package.
- **LEAD-18** Questions travel UP, never get guessed — the escalation channel. Any seat (agent or hat) that lacks the information to decide **stops and returns a QUESTION to the team lead** instead of picking a plausible default: expected volume/growth, data placement, real-time need, who the user is, which platform, what the acceptance really means. The team lead batches the open questions and **puts them to Raze** — the lead may answer only what is already written in the loaded rules, the spec, or the registry; anything requiring judgment, business context, or a number nobody has measured goes to Raze verbatim. **Noisy questions are cheaper than a confident wrong build** (globalclaude ask-don't-guess, at team scale). A brief that provoked questions gets fixed at the source: the answers go back into the spec so the next agent never has to ask again.
- **LEAD-22** An agent's report is EVIDENCE, never fact. Before any load-bearing claim in it is repeated to Raze, acted on, or written into a doc, verify it against the source it names — the file, the line, the live surface. An agent that overstates is not lying; it is reasoning from a partial read, and the lead is the only seat positioned to catch that. **Relaying an unverified agent claim makes its error yours.**
- **LEAD-23** A finding survives only if it is written somewhere durable in the SAME turn it arrives. Agent output is ephemeral — a report is a message, not a store, and it is gone with the session. Anything that must outlive the sitting (a finding, a verdict list, an unresolved question, a collision needing a ruling) goes into Linear or a repo doc immediately, never left in a report to be collected later. **A finding nobody wrote down is a finding nobody has.**

## 2 · The team & dispatch

You (the main loop) are the **supervisor** — the repo's `CLAUDE.md` defines the seat, Raze talks to you. You hold two seats yourself and dispatch the rest:

- **Architect** (agent) — dispatched **first**, before any builder. It returns the design; you fan that design out to every seat the work touches, collect their feedback, and the architect reconciles it into one final design (LEAD-20). Design is upstream of all code.
- **Builders** (agents you dispatch with the approved design as a complete brief) — they write code; they never mark Done or push.
- **Reviewers** (agents you dispatch before Done, read-only).

Membership: registry `HQ-M1-org-chart.md` § dev team — which seats exist, and which agent name each dispatches. The routing table (§4) says which one a task loads.

Agents are summoned by name via the Agent tool (available in every repo through the `~/.claude/agents` junction). A dispatched agent reads **only its wrapper and its own seat rulebook** (`team/devteam/<seat>.md`, plus a `stacks/` file where the brief declares one) — never `teamlead.md`, the repo `CLAUDE.md`, `learnrules.md`, or another seat's file. A builder agent cannot see this conversation — it gets only the brief you hand it, so the brief must be complete. Reference: `dispatch-brief.md` — read when writing a dispatch brief. Their output is a report to you (MGR-27: read it, don't just trust it); you integrate, and you alone move the ticket.

**When to dispatch vs do it inline:** a well-specified, self-contained unit → dispatch the matching agent. Exploratory or tightly-coupled-to-this-conversation work → wear the seat hat yourself (read the rulebook, build inline). Never dispatch a builder on a brief you couldn't hand to a stranger.

**What no agent may ever do (MGR-23).** Marking a Linear issue Done, publishing, flipping a go-live switch, spending money, and running git are the owner's and the lead's — exclusively, never an agent's, whatever its seat. This is the one home for that boundary: agent definitions cite it, they never restate it.

## 3 · Addressing & naming

- **LEAD-1** Every artifact carries its **address**, and the address is the same string on every surface: the org's code format is `<Domain>[<subdomain>]-M<module>[.<item>[.<sub>]]`, and that identical string appears on **all four surfaces — folder · file (`<code>-<slug>`) · Linear title (`<code> · <name>`) · diagram node**. Files are named by their code, and **the code is tagged in the code itself** — the lane code in the module/file header comment, in log lines, and in the identifiers that carry it — so a break anywhere maps to exactly one lane without a search. Numbering is append-only and never reused; shipped ids and public names are permanent (code, don't rename — change the label, never the id). Law: globalclaude §6; this is its build-time execution.
- **LEAD-2** Before naming ANYTHING — a file, folder, module, table, field, event, or feature — read the repo's two naming authorities: **`M0-laneindex.md`** (which codes exist, what each lane is, what number comes next) and **`vocabulary.md`** (which word this repo already uses for that concept — one word, one meaning, MGR-43/DB-14). Reuse the existing code and the canonical word; never mint a second name for a thing that already has one. The dispatch **brief carries the work's address** (code · folder · Linear title) — an agent that receives no code does NOT guess one, it returns the question (LEAD-18); an unaddressed artifact is a defect that costs a rename later, and renames are forbidden once shipped.

## 4 · Seat routing

| Task touches | Load rulebook / dispatch agent |
|---|---|
| any coding at all | this file (always) |
| system design, new module/domain, schema, config shape | `architect.md` → `architect` (first, before any builder) |
| schema, migrations, constraints, write-integrity | `database.md` → `database-engineer` |
| server logic, external/paid API calls, error paths, event consumers | `backend.md` → `backend-engineer` |
| screens, browser code | `frontend.md` → `frontend-engineer` (code only — it does not carry UX law); pair `uxui-designer` on `uxui.md` for the design judgment |
| visual/design-system work | `uxui.md` → `uxui-designer` |
| deploy, restart, workers, cron, infra | `devops.md` + `stacks/<declared>.md` → `devops-engineer` |
| auth, secrets, input from outside | `security.md` → `security-reviewer` on the diff |
| claiming something works / Done | `qa.md` → `qa-verifier` on the feature |
| writing or running tests, harnesses, CI | `tester.md` → `tester` |

- **LEAD-3** Load the seats the task touches, nothing more. A seat file or its `... RULES v1 LOADED` sentinel missing = broken junction — stop, repair per boot-law BOOT-4, never code from memory of the rules.
- **LEAD-4** The repo's operator card declares `Stacks:`; only the declared stack packs load. A new stack = a new near-empty pack file in HQ the same day — never rules dumped into a seat file.
- **LEAD-19** Every seat works **only inside its own scope**. A seat handed work that belongs to another seat does NOT do it — it hands the work back to the team lead, naming the seat it belongs to (LEAD-18's channel). Building outside your seat is a defect even when the code is correct: the wrong seat's rules were applied, and the reviewer who owns those rules never sees it.

## 5 · The build loop

- **LEAD-5** Per issue: **build** the smallest correct implementation → **test for real** against live services → **review honestly**, fix immediately if broken → **prove** the issue's stated Done with an observed real run, then mark Done.
- **LEAD-6** Open-source first: never build before searching (GitHub, APIs, SaaS, SDKs). Decide reuse / build / buy; build only when nothing meets the requirement.
- **LEAD-7** Main path first: the smallest correct change — no speculative features, folders, abstractions or edge-case hunting for problems that have not occurred.
- **LEAD-8** Retire the make-or-break risk first: the riskiest technical unknown gets its dry-run proof before the surrounding build proceeds; irreversible public side effects stay behind an explicit flag.
- **LEAD-9** Before a coding issue moves to Done, `qa-verifier` runs its gates against the project's declared metric set (QA-17 · QA-18); findings (rule IDs + file:line quotes, QA-15) land as a Linear comment — fixed or explicitly waived.
- **LEAD-17** Security controls and data-integrity guards are **never** "speculative" — they are in scope on the main-path-first pass (LEAD-7), always. A guard on money, credentials, auth, or data integrity is required work, not gold-plating; skipping it is not "lean", it is a defect waiting.
- **LEAD-20** The design pipeline, before any code: dispatch `architect` → it returns the design → **fan the design out to every seat the work touches** for written feedback with reasons → the architect reconciles into ONE final design → Linear, the Figma board and the `/docs` documentation are updated to match → **Raze approves** → build begins. No seat writes code against a design that has not been through the fan-out and the approval.
- **LEAD-10** Done law is the manager's (MGR-20/22/23): verified = Done with evidence link; proven run = the main path ran once for real; go-live stays the owner's. This file adds nothing — cite, never restate.

## 6 · Rule-system law

Reference: `devteam-rulelaw.md` (LEAD-11…LEAD-16) — read when authoring, promoting, tagging or retiring a dev rule.

`DEV TEAM LOADED — teamlead v1`
