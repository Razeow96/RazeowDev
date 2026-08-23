# Manager SOP — /team/manager

How every repo is operated: boot sequence, the fixed files and folders, their templates, and the rare procedures. The law lives in the companion `manager-rules.md`. Both are authored once in HQ, junction-distributed, read-only in every repo. §1–3 apply every session; §4–6 are read only on the day you need them.

**Sections**
1. Boot — the read order every session starts with
2. The files — what each file is, and its exact contract and how files should be documented
3. folders — the repo skeleton
4. File templates — copy-paste `session.md` · `learnrules.md` · `vocabulary.md` · `M0-laneindex.md`
5. Procedures — new-repo spin-up · repo transfer/sale · junction repair · status transitions
6. Repo CLAUDE.md template — the fixed skeleton every repo copies

## 1 · Boot

- **MGR-1** Boot order: `CLAUDE.md` → `learnrules.md` → `session.md` → `M0-laneindex.md`. Nothing else at boot. A file that is empty or missing is noted, never silently skipped. (`vocabulary.md` is read on demand — when naming anything, writing schema, or drafting copy — not at boot.)
- **MGR-2** Conditional loads: coding → `team/devteam/` rules first · diagrams → `team/drawingteam/` rules first · entering a sub-folder with its own `CLAUDE.md` → read it before touching that folder.
- **MGR-3** Trust order, total: **live surface > HQ registry > M0-laneindex > session.md > memory**. Any load-bearing claim (deployed, migrated, paid, "X is live") is verified one level up before work stands on it. A disagreeing surface is fixed on sight, in the same session.
- **MGR-4** MCP gate: the session's mandatory servers are verified at boot; never work on a partial toolset. A tool that dies mid-session stops that step — it is never silently worked around.
- **MGR-5** A repo missing any of the five fixed files: the first session **creates them from §4 templates** before any other work (`vocabulary.md` starts as the template skeleton and grows at each natural touch — never a back-fill sprint).

## 2 · The files
*(five fixed files: `CLAUDE.md` · `session.md` · `learnrules.md` · `M0-laneindex.md` · `vocabulary.md`)*

- **MGR-6** `CLAUDE.md` — operator card + project invariants only (skeleton: §6). No global law, no history, no restated handbook rules.
- **MGR-7** `session.md` — **replaced, never appended.** One `# Session — YYYY-MM-DD` H1, then exactly `## What exists now` / `## Blocked` / `## Next` / `## Linear progress at this session`. Present tense, state not story; empty section = `none`.
- **MGR-8** `session.md` machine contract (HQ Brain pulls it daily): **under 6,000 characters** (silently truncated past that) and **no line starts with `STALE` or `IN REVIEW`** (those prefixes are scraped into the owner's daily briefing as flags).
- **MGR-9** `learnrules.md` — numbered, append-only, **project rules only**. A rule is registered the moment Raze confirms it AND codified in its authority home in the same step — never deferred to /close. A rule that would prevent the same mistake in another repo is flagged **with a `suggest:` destination from the org roster** (the operator knows the seats — teamlead's routing table); the CEO sweep verdicts it, stamps the line, and advances the register's `CEO-swept:` cursor — stamped rules are settled and never re-read (format + stamps: §4 T2). Global rules are never authored locally.
- **MGR-10** `M0-laneindex.md` — the module map, one line per lane with folder + Linear + board links. It wins locally; the HQ registry wins above it. Every shipped lane gets its line the day it ships.
- **MGR-11** One rule, one home. Never restate a rule in a second loaded file — cite its ID instead. Routing: binding invariants → repo `CLAUDE.md` · lessons → `learnrules.md` · decisions with trade-offs → Linear. Never all three.
- **MGR-12** Deprecation is deletion, in the same pass — from code, docs, memory and tools. A fallback that must survive carries a stated trigger and an expiry date.
- **MGR-13** Filename casing is law: `CLAUDE.md` uppercase · `session.md` `learnrules.md` `M0-laneindex.md` lowercase. Case-sensitive systems (CI, cloud agents, the Brain pull) break silently on anything else.
- **MGR-41** Every living doc — anything maintained rather than one-shot — opens with a short **Sections** pointer list (number · name · one-line of what it covers) before any content. The reader gets the map before the territory.
- **MGR-43** `vocabulary.md` — the repo's ubiquitous language, one row per term: `term · meaning · owner (the ONE file/table/surface that defines it) · notes`. It is org-wide alignment, not a dev artifact: code, marketing copy, stakeholder docs, Linear titles and diagrams all mean the same thing by the same word. One word = one meaning per repo; a word already taken is never reused for a second concept. A new term is added in the SAME pass as whatever introduces it (schema per DB-14, campaign, doc, or feature). Populate at the next natural touch of an area — never a back-fill sprint.

## 3 · folders

- **MGR-14** `/team` — junction from HQ, committed to git through the junction (never gitignored: a clone or a sold repo keeps its frozen handbook copy). Never edited here; edits happen in HQ only.
- **MGR-15** `/docs` — live SPECs only; superseded docs are deleted, not kept alongside. `docs/private/` is gitignored and holds secret values; tracked docs record what exists and where, never the value.
- **MGR-16** `.github/workflows/` — CI and project cron jobs, authored per-repo.
- **MGR-17** Skills load user-level from `~/.claude/skills` (junctioned from HQ) — repos carry no skill folders.
- **MGR-18** `README.md` is the human/buyer landing (what this is, how to run it). `CLAUDE.md` is the operator card. Neither restates the other.

## 4 · File templates

### T1 · session.md

```markdown
# Session — YYYY-MM-DD

Current state only. This file is **replaced** each session, never appended.

## What exists now
<what is live and load-bearing — one line each>

## Blocked
<what cannot move and what it waits on — or `none`>

## Next
<the first thing next session should do — or `none`>

## Linear progress at this session
<issues moved this session, one line each: ID · new state — or `none`>
```
Contract (MGR-7/8): under 6,000 chars · no line starts with `STALE` or `IN REVIEW` · present tense.

### T2 · learnrules.md

```markdown
# Learned rules — <CODE>

CEO-swept: through #0 · never

Project-scoped register. One numbered entry per approved rule, append-only, never deleted.
Each rule is also codified in its authority home the same step — the home is the law, this is the index.
Global candidates carry the flag inline, WITH a suggested destination from the org roster.

A rule must be written general enough to apply beyond the one incident that caused it — extract
the mistake and prevent the class of it. A rule narrowed to one specific case is not a lesson,
it is documentation.

1. YYYY-MM-DD — <rule, one or two lines, imperative> · Home: <file> · suggest: [<seat/global doc>] [CEO — global candidate]
```

Lifecycle (the CEO sweep): the `CEO-swept:` header is the cursor — the weekly sweep reads ONLY rules
above it, then advances it; stamped rules are settled and never re-read (flat token cost forever).
Verdict stamps, written on the rule line: `→ promoted <ID> [CEO ✓ date]` (text moved to the seat,
tombstone stays) · (stays project-only) · (rejected/superseded).

### T3b · vocabulary.md

```markdown
# Vocabulary — <CODE>

The repo's ubiquitous language. One word = one meaning, org-wide: code, marketing copy,
stakeholder docs, Linear titles and diagrams all use these terms the same way.
A word already taken is never reused for a second concept — pick a different word.
New terms are added in the same pass as whatever introduces them (MGR-43 · DB-14).

| Term | Meaning | Owner (the one surface that defines it) | Notes |
|---|---|---|---|
| <term> | <one line> | <table.column / doc / screen> | <collisions, retired synonyms> |
```
Populate at the next natural touch of an area — never a back-fill sprint. A known collision
is written down as a row (with both meanings) rather than left to be rediscovered.

### T3 · M0-laneindex.md

```markdown
# <CODE>-M0 — Lane index

Authority for every <CODE> code. If any surface disagrees, this table wins locally; the HQ registry wins above it.

| Code | Lane | Folder | Linear | Board |
|---|---|---|---|---|
| <CODE>-M1 | <name> | <path> | <issue/project link> | <Figma link or —> |
```
Rows are append-only: renamed → update the path, code unchanged · killed → mark retired, never delete the row or reuse the code.

## 5 · Procedures

### P1 · New-repo spin-up (empty folder → compliant, ~30 min)

1. Create the repo under `github.com/<your-account>`, folder under `~\Documents\GitHub\`.
2. Junction the team folder (no admin needed):
   `New-Item -ItemType Junction -Path "<repo>\team" -Target "$HOME\Documents\GitHub\<your-hq-repo>\team"`
   (The dev-team agents are user-level, not per-repo — a one-time `~/.claude/agents` junction to HQ `HQ-M5-team/globalagents/` covers every repo; see `HQ-M5-team/globalagents/README.md`. No per-repo step.)
3. Copy `.gitignore` baseline: `docs/private/` · `.claude/settings.local.json` · `*.env`.
4. Create the five fixed files: `CLAUDE.md` from §6 (fill the operator card — Status almost always PLANNING) · `session.md`, `learnrules.md`, `vocabulary.md`, `M0-laneindex.md` from §4.
5. Add the company section to the HQ registry (`HQ-M1-org-chart.md`) and the repo to the Brain's `REPOS` list (`HQ-M0-brain/HQ-M0.4-ingest.mjs`) — update BOTH in the same pass.
6. Create the Linear project; link it in the operator card.
7. Boot check: fresh session shows `OPERATOR HANDBOOK v2 LOADED` + the card. Raze pushes the first commit.

### P2 · Repo transfer / sale

1. Freeze: Status → FOR-SALE, feature freeze (rules §1).
2. Delete `## H · HQ contract` from the repo's committed `/team/manager/manager-rules.md` copy and cut the junction — the committed `/team` snapshot stays as the buyer's frozen handbook.
3. Rotate EVERY credential the repo touches; hand over fresh values outside git. Remove its rows from HQ `accounts-inventory.md`.
4. Registry: status → `sold`, section kept for history, removed from active list. Remove repo from Brain `REPOS`. Both in the same pass.
5. Strip HQ links from the operator card (Linear, registry row). Codes stay retired forever.
6. Verify: `Depends on` empty · no sibling-repo paths in the tree · buyer can boot from the README + CLAUDE.md alone.

### P3 · Junction repair

Symptom: `OPERATOR HANDBOOK v2 LOADED` missing at boot, or a dev-team agent reports its seat sentinel missing.
`Get-Item "<repo>\team" | Select LinkType` — if not `Junction`, delete the stale folder (first: no live process from that path, registry `runtime:` clear) and re-run P1 step 2. Same procedure for `~/.claude/skills` (target `<your-hq-repo>\globalskills\<name>`) and `~/.claude/agents` (target `<your-hq-repo>\HQ-M5-team\globalagents`).

### P4 · Status transitions

- PLANNING → KICKED-OFF: portfolio gate approved by Raze (recorded in registry + operator card, same pass).
- KICKED-OFF → LIVE: first main path proven for real.
- Any → FOR-SALE / RETIRED: Raze's call; registry + card + Linear project state updated together.

## 6 · Repo CLAUDE.md template

Every company repo's `CLAUDE.md` follows this skeleton exactly. Target: under one screen for a PLANNING repo, ≤120 lines for the richest LIVE system. Stamp `template: v2` so stale forks are one grep away.

```markdown
# <CODE> — <Company Name>

@team/manager/manager-rules.md
@team/manager/managerSOP.md
<!-- template: v2 · if "OPERATOR HANDBOOK v2 LOADED" did not appear above,
     the /team junction is broken — stop and repair per managerSOP.md §5 -->

You are the operator of **<Company (CODE)>** — <one line: what this company is and how it makes money>. Global law loads from `~/.claude/CLAUDE.md`; the handbook loads above; this file is only what is <CODE>-specific.

## Operator card
- **Company:** <CODE> · <name>
- **Status:** <PLANNING / KICKED-OFF / LIVE / FOR-SALE / RETIRED>
- **Operator seat:** Claude (operator) · Raze (owner)
- **Owned KR:** <org KR line, or "none">
- **Money path:** payer <who> · kill gate <date + metric> <PLANNING: "gate not written — no build">
- **Runtime:** <what runs where — process, host, deploy command — or "nothing runs">
- **Stacks:** <declared stacks whose dev-rule packs load when coding — e.g. windows-host · supabase-deno · cloudflare-workers · python-worker; every repo on this machine carries windows-host>
- **Depends on:** <none — anything listed here is transferability debt, flagged>
- **Registry row:** <your-hq-repo>/HQ-M1-org-chart/HQ-M1-org-chart.md § <Company>
- **Linear project:** <name> — <link>
- **Numbering authority:** <doc path holding the module codes>

## What this repo actually is
<one paragraph: codebase / docs-and-paste-source / skeleton — so the operator
treats it as what it is. State the test reality honestly, including "there is none".>

## Hard warnings
<only if any exist — two tiers:>
- **Traps that have already cost a session:** <the one operational trap + its first diagnostic>
- **Hard warnings:** <credential surfaces (list the secret-bearing paths + assert the gitignore),
  identity splits, live-legacy do-not-touch lines>

## Project invariants
<numbered, only what is true of THIS system and nowhere else. Architecture laws,
domain tables, per-repo proven-run definition, per-repo Definition of Done.
Anything needing >3 lines points to a /docs SPEC instead.>

## Pointers
<paths only, never content: nested CLAUDE.mds, drawing-rules doc, canonical Figma board.>
```

**Never in a repo CLAUDE.md:** global law restated · handbook rules restated · communication/naming law · OKR beyond the owned-KR line · history or changelog (git holds it) · boot-order instructions (MGR-1 owns it) · lesson narratives (learnrules holds them) · credential values (nowhere, ever).
