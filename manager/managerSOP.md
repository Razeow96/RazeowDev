# Manager SOP — /team/manager

How every repo is operated: boot sequence, the fixed files and folders. The law lives in the companion `manager-rules.md`. Both are authored once in HQ, junction-distributed, read-only in every repo. §1–3 apply every session. Templates, spin-up and transfer moved to their class homes 2026-08-30 (structure-law) — see §4.

**Sections**
1. Boot — the read order every session starts with
2. The files — what each file is, and its exact contract
3. folders — the repo skeleton
4. Moved — where the templates and rare procedures now live

## 1 · Boot

- **MGR-1** Boot order: `CLAUDE.md` → `learnrules.md` → `session.md` → `repolaneindex.md` → `repovocabulary.md`. Nothing else at boot. A file that is empty or missing is noted, never silently skipped. A module folder's `CLAUDE.md` and `<code>-laneindex.md` load on entering that folder (MGR-2), never at boot.
- **MGR-2** Conditional loads: coding → `team/devteam/` rules first · diagrams → `team/drawingteam/drawing-law.md` first (DRAW-1: purpose clear → launch its skill; unclear → ask what kind of design) · entering a sub-folder with its own `CLAUDE.md` → read it before touching that folder · an act with act-scoped law (Linear → `linear-law.md` · boot → `boot-law.md` · filing docs → `structure-law.md`) → that law loads at the act's start, by the skill or step performing it.
- **MGR-3** Trust order, total: **live surface > HQ registry > repolaneindex (then the module lane index below it) > session.md > memory**. Any load-bearing claim (deployed, migrated, paid, "X is live") is verified one level up before work stands on it. A disagreeing surface is fixed on sight, in the same session.
- **MGR-4** MCP gate: the session's mandatory servers are verified at boot; never work on a partial toolset. A tool that dies mid-session stops that step — it is never silently worked around. (Gate mechanics: `boot-law.md`.)
- **MGR-5** A repo missing any of the five fixed files or its root `msot/`: the first session **creates them from `manager-templates.md`** before any other work (`repovocabulary.md` starts as the template skeleton and grows at each natural touch — never a back-fill sprint).

## 2 · The files
*(five fixed files: `CLAUDE.md` · `session.md` · `learnrules.md` · `repolaneindex.md` · `repovocabulary.md` — plus the root `msot/` folder, MGR-45)*

- **MGR-6** `CLAUDE.md` — operator card + project invariants only (skeleton: manager-templates T4). No global law, no history, no restated handbook rules.
- **MGR-7** `session.md` — **replaced, never appended.** One `# Session — YYYY-MM-DD` H1, then exactly `## What exists now` / `## Blocked` / `## Next` / `## Progress`. Present tense, state not story; empty section = `none`. `## Progress` is the one summary exception: 1–3 lines of what moved on this project this session, written for Laura's daily read (the Brain pulls this file — MGR-8) — a human summary, never a Linear-state dump (Linear already records the moves).
- **MGR-8** `session.md` machine contract (HQ Brain pulls it daily): **under 6,000 characters** (silently truncated past that) and **no line starts with `STALE` or `IN REVIEW`** (those prefixes are scraped into the owner's daily briefing as flags).
- **MGR-9** `learnrules.md` — the rule **INBOX**, project rules only. Numbered rows; numbers are never reused (`next: #N` counter line). A rule is registered the moment Raze confirms it AND codified in its authority home in the same step — never deferred to /close. A rule that would prevent the same mistake in another repo is flagged **with a `suggest:` destination from the org roster**. The sweep verdicts each row, verifies the home holds the law, then **deletes the row** — a codified rule's row is duplication (owner ruling 2026-08-30); git history holds the past. Empty is the healthy state. Global rules are never authored locally. (Format: manager-templates T2.)
- **MGR-10** `repolaneindex.md` — the company map, in the register format (T3): one row per module and per company-level truth doc, with path, Linear, board and human-view links; each module row points to that module's `<code>-laneindex.md`, which holds the module's own rows (full code in the name, e.g. `RS2-M3-laneindex.md` — never a bare `M3`). It wins locally; the HQ registry wins above it. Every shipped lane gets its row the day it ships. Over the size guard → spine + leaf split (structure-law).
- **MGR-11** One rule, one home. Never restate a rule in a second loaded file — cite its ID instead. Routing: binding invariants → repo `CLAUDE.md` · lessons → `learnrules.md` · decisions with trade-offs → Linear. Never all three.
- **MGR-12** Deprecation is deletion, in the same pass — from code, docs, memory and tools. A fallback that must survive carries a stated trigger and an expiry date.
- **MGR-13** Filename casing is law: `CLAUDE.md` uppercase · `session.md` `learnrules.md` `repolaneindex.md` `repovocabulary.md` lowercase · module files `<CODE>-laneindex.md` / `<CODE>-vocabulary.md` with the code as registered. Case-sensitive systems (CI, cloud agents, the Brain pull) break silently on anything else.
- **MGR-41** Every living doc — anything maintained rather than one-shot — opens with a short **Sections** pointer list (number · name · one-line of what it covers) before any content. The reader gets the map before the territory.
- **MGR-43** `repovocabulary.md` — the repo's ubiquitous language (a module gets its own `<code>-vocabulary.md` only on its first term of its own; a term is written once, at the lowest level that uses it), one row per term: `term · meaning · owner (the ONE file/table/surface that defines it) · notes`. It is org-wide alignment, not a dev artifact: code, marketing copy, stakeholder docs, Linear titles and diagrams all mean the same thing by the same word. One word = one meaning per repo; a word already taken is never reused for a second concept. A new term is added in the SAME pass as whatever introduces it (schema per DB-14, campaign, doc, or feature). Populate at the next natural touch of an area — never a back-fill sprint.

- **MGR-45** Truth folders — `msot/` and `sot/`.
  - **Levels.** Repo root (the company): `CLAUDE.md` · `repolaneindex.md` · `repovocabulary.md` · `msot/`. Module folder (M1, M2… — the owner's word is "sub-domain"): `CLAUDE.md` (loads on entering) · `<code>-laneindex.md` (required) · `<code>-vocabulary.md` (on its first own term) · `msot/` (required). Project folder (an item under a module that holds project docs, e.g. `RS2-M3.4.1-coos/`): `sot/` (required).
  - **`msot/` = master source of truth** for its level: onboarding, SOPs, Q&A, and `templates/` — one template per doc every project below must produce. **`sot/` = one project's finalized truth**: its register (`<code>-register.md`) plus the docs its module's templates require.
  - **Truth = a doc in `msot/` or `sot/`, or a row in a lane index. Everything else is working material** — discussion notes, drafts, scratch, sub-agent output: never cited as truth, swept at every close (`/close` step 3b). Documents only; code, config and generated content pages follow the build and its gates.
  - **One topic, one home, at the lowest level that shares it:** used by one project → its `sot/`; by every project in a module → the module `msot/`; company-wide → the root `msot/`. Never two copies — a changed fact is edited in its home, never re-written as a new doc.
  - **Google Drive mirror**. Every `msot/` and every `sot/` has one Google Drive folder, and the Drive path mirrors the repo path level by level with the identical folder names (§6 one name per thing): company folder → module folder → item folder → `msot` or `sot`. Example: `<your-hq-repo>` › `HQ-M4-finance` › `HQ-M4.2-entry-bot` › `sot`. The Drive folder is the file side of that truth — PDFs, sheets, decks, exports, anything that is not Markdown — and it is what the `Human view:` line of that level's register links to. The Markdown in the repo stays the truth; a Drive file that contradicts it is corrected, never the other way round. A new `msot/` or `sot/` is not done until its Drive folder exists and the register links it; a renamed or retired code renames or retires the Drive folder in the same pass (§8 an update replaces).
  - **Human view.** Every lane index and `sot/` register carries a `Human view:` link line, pointing at that level's Drive folder. Sheet tool for the table itself not yet chosen (Google Sheets / Smartsheet / Notion); once connected: pull at `/start`, and at `/close` pull → owner confirms his edits → push. After every close the Markdown is the truth.
  - Shapes: manager-templates T3 (lane index and register table), T6 (`msot/`), T7 (`sot/`).

## 3 · folders

- **MGR-14** `/team` — junction from HQ, committed to git through the junction (never gitignored: a clone or a sold repo keeps its frozen handbook copy). Never edited here; edits happen in HQ only.
- **MGR-15** `/docs` — live SPECs only; superseded docs are deleted, not kept alongside. `docs/private/` is gitignored and holds secret values; tracked docs record what exists and where, never the value.
- **MGR-16** `.github/workflows/` — CI and project cron jobs, authored per-repo.
- **MGR-17** Two kinds of skill, nothing else. **Global** — authored in HQ `globalskills/`, junctioned to `~/.claude/skills`, usable from every repo. **Repo** — `<repo>/.claude/skills/<name>/`, used only inside the repo that owns it. Another repo that needs a repo skill uses a global one: the skill is promoted to `globalskills/` and the repo copy is deleted — never copied repo to repo.
- **MGR-18** `README.md` is the human/buyer landing (what this is, how to run it). `CLAUDE.md` is the operator card. Neither restates the other.

## 4 · Moved (structure-law class homes)

- File templates (session · register · vocabulary · laneindex · repo CLAUDE.md skeleton + never-list) → **`manager-templates.md`** — read at creation.
- New-repo spin-up + internal move → **/repospinup** skill · transfer/sale → **/repotransfer** skill.
- Junction repair → **`boot-law.md`** BOOT-4.
- Status transitions → **manager-rules MGR-44** (law, not procedure).
