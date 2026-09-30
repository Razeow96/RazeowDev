# Manager templates

`TEMPLATES v1 LOADED`

Fixed file shapes (TEMPLATE class — read at the moment of creating the file, never carried). Moved out of managerSOP §4/§6, 2026-08-30 (an internal ticket). The laws behind these shapes: MGR-6/7/8 (files) · MGR-9 (register) · MGR-43 (vocabulary) · MGR-10 (laneindex).

## T1 · session.md

```markdown
# Session — YYYY-MM-DD

Current state only. This file is **replaced** each session, never appended.

## What exists now
<what is live and load-bearing — one line each>

## Blocked
<what cannot move and what it waits on — or `none`>

## Next
<the first thing next session should do — or `none`>

## Progress
<1–3 lines — what moved this session, for Laura's daily read — or `none`>
```
Contract (MGR-7/8): under 6,000 chars · no line starts with `STALE` or `IN REVIEW` · present tense · `## Progress` is the one summary exception, never a Linear-state dump.

## T2 · learnrules.md

```markdown
# Learned rules — <CODE>

next: #1 · numbers are never reused; deleted rows live in git history

The rule INBOX (MGR-9). A row lands the moment the rule is confirmed AND is codified in its
authority home the same step — the home is the law. The sweep verdicts each row, verifies the
home, then DELETES the row. This file holds only rules awaiting a home or the sweep; empty is healthy.

A rule must be written general enough to apply beyond the one incident that caused it — extract
the mistake and prevent the class of it. A rule narrowed to one specific case is not a lesson,
it is documentation.

1. YYYY-MM-DD — <rule, one or two lines, imperative> · Home: <file> · suggest: [<seat/global doc>] [CEO — global candidate]
```

## T3b · repovocabulary.md · `<CODE>-vocabulary.md`

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
Populate at the next natural touch of an area — never a back-fill sprint. A known collision is written down as a row rather than left to be rediscovered.

## T3 · repolaneindex.md · `<CODE>-laneindex.md` · `sot/<code>-register.md`

One register format at every level — repo, module, project (MGR-10, MGR-45). Same columns everywhere so a human sheet view and the AI read the same table.

```markdown
# <CODE> — Lane index        (register: "# <CODE> — <Project name> register")

Authority for every <CODE> code at this level. If any surface disagrees, this table wins locally; the level above wins above it (HQ registry at the top).
Human view: <sheet link, or — until a sheet tool is connected>

| Code | Name | Type | Path / link | Linear | Owner | Status | Last update |
|---|---|---|---|---|---|---|---|
| <CODE>-M1 | <name> | module | <folder>/ · <CODE>-M1-laneindex.md | <link or —> | Raze / Claude / <seat> | live | YYYY-MM-DD |
```
Type: `module · project · doc · template · sop · agent · skill · key · figma · url`. Status: `current · live · active · parked · archive · retired`. A `key` row records where it is stored and its expiry, never the value. Code numbers are append-only; a retired row stays.
Over the size guard → spine + leaf split (structure-law §1 REFERENCE): the domains table stays as the spine with a `Lanes` file-path column; each domain's lane rows become their own file addressed by filename.

## T4 · Repo CLAUDE.md (template: v2)

Every company repo's `CLAUDE.md` follows this skeleton exactly. Target: under one screen for a PLANNING repo, ≤120 lines for the richest LIVE system. Stamp `template: v2` so stale forks are one grep away.

```markdown
# <CODE> — <Company Name>

@team/manager/manager-rules.md
@team/manager/managerSOP.md
<!-- template: v2 · if "OPERATOR HANDBOOK v2 LOADED" did not appear above,
     the /team junction is broken — stop and repair per boot-law BOOT-4 -->

You are the operator of **<Company (CODE)>** — <one line: what this company is and how it makes money>. Global law loads from `~/.claude/CLAUDE.md`; the handbook loads above; this file is only what is <CODE>-specific.

## Operator card
- **Company:** <CODE> · <name>
- **Status:** <PLANNING / KICKED-OFF / LIVE / FOR-SALE / RETIRED>
- **Operator seat:** Claude (operator) · Raze (owner)
- **Owned KR:** <org KR line, or "none">
- **Money path:** payer <who> · kill gate <date + metric> <PLANNING: "gate not written — no build">
- **Runtime:** <what runs where — process, host, deploy command — or "nothing runs">
- **Stacks:** <declared stacks whose dev-rule packs load when coding; every repo on this machine carries windows-host>
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

## T6 · `msot/` — master source of truth (repo root and every module)

```
msot/
├─ onboarding.md        what this level is, who works in it, where to start, which lane index to read
├─ sop-<act>.md         one per repeatable act (e.g. sop-new-website.md): steps · docs needed · template · agent · output path · done check
├─ qna.md               answered questions, one row each, with the doc that settles it
└─ templates/           one template per doc every project below must produce in its sot/
```
Every file here has a row in its level's lane index. A module whose projects all need the same doc gets its template here — never a copy per project. The level's Google Drive folder mirrors this path with the identical folder names (`<company>/<module folder>/msot`, MGR-45) and holds the non-Markdown files; the lane index's `Human view:` line links it.

## T7 · `sot/` — one project's finalized truth

```
sot/
├─ <code>-register.md   T3 table: every truth doc, link, agent, key and URL of this project; its `Human view:` line links the project's Drive folder
└─ <the docs its module's msot/templates/ require, filled in>
```
A doc not in `sot/` and not a register row is working material (MGR-45) — promoted, merged or deleted at close. The project's Google Drive folder mirrors this path with the identical folder names (`<company>/<module folder>/<item folder>/sot`, e.g. `<your-hq-repo>/HQ-M4-finance/HQ-M4.2-entry-bot/sot`) and holds the non-Markdown files; a `sot/` is not done until that folder exists and the register links it.
