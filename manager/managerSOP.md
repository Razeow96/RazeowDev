# Manager SOP — /team/manager

How every repo is operated: boot sequence, the fixed files and folders. The law lives in the companion `manager-rules.md`. Both are authored once in HQ, junction-distributed, read-only in every repo. §1–3 apply every session. Templates, spin-up and transfer moved to their class homes 2026-08-30 (structure-law) — see §4.

**Sections**
1. Boot — the read order every session starts with
2. The files — what each file is, and its exact contract
3. folders — the repo skeleton
4. Moved — where the templates and rare procedures now live

## 1 · Boot

- **MGR-1** Boot order: `CLAUDE.md` → `learnrules.md` → `session.md` → `M0-laneindex.md`. Nothing else at boot. A file that is empty or missing is noted, never silently skipped. (`vocabulary.md` is read on demand — when naming anything, writing schema, or drafting copy — not at boot.)
- **MGR-2** Conditional loads: coding → `team/devteam/` rules first · diagrams → `team/drawingteam/drawing-law.md` first (DRAW-1: purpose clear → launch its skill; unclear → ask what kind of design) · entering a sub-folder with its own `CLAUDE.md` → read it before touching that folder · an act with act-scoped law (Linear → `linear-law.md` · boot → `boot-law.md` · filing docs → `structure-law.md`) → that law loads at the act's start, by the skill or step performing it.
- **MGR-3** Trust order, total: **live surface > HQ registry > M0-laneindex > session.md > memory**. Any load-bearing claim (deployed, migrated, paid, "X is live") is verified one level up before work stands on it. A disagreeing surface is fixed on sight, in the same session.
- **MGR-4** MCP gate: the session's mandatory servers are verified at boot; never work on a partial toolset. A tool that dies mid-session stops that step — it is never silently worked around. (Gate mechanics: `boot-law.md`.)
- **MGR-5** A repo missing any of the five fixed files: the first session **creates them from `manager-templates.md`** before any other work (`vocabulary.md` starts as the template skeleton and grows at each natural touch — never a back-fill sprint).

## 2 · The files
*(five fixed files: `CLAUDE.md` · `session.md` · `learnrules.md` · `M0-laneindex.md` · `vocabulary.md`)*

- **MGR-6** `CLAUDE.md` — operator card + project invariants only (skeleton: manager-templates T4). No global law, no history, no restated handbook rules.
- **MGR-7** `session.md` — **replaced, never appended.** One `# Session — YYYY-MM-DD` H1, then exactly `## What exists now` / `## Blocked` / `## Next` / `## Progress`. Present tense, state not story; empty section = `none`. `## Progress` is the one summary exception: 1–3 lines of what moved on this project this session, written for Laura's daily read (the Brain pulls this file — MGR-8) — a human summary, never a Linear-state dump (Linear already records the moves).
- **MGR-8** `session.md` machine contract (HQ Brain pulls it daily): **under 6,000 characters** (silently truncated past that) and **no line starts with `STALE` or `IN REVIEW`** (those prefixes are scraped into the owner's daily briefing as flags).
- **MGR-9** `learnrules.md` — the rule **INBOX**, project rules only. Numbered rows; numbers are never reused (`next: #N` counter line). A rule is registered the moment Raze confirms it AND codified in its authority home in the same step — never deferred to /close. A rule that would prevent the same mistake in another repo is flagged **with a `suggest:` destination from the org roster**. The sweep verdicts each row, verifies the home holds the law, then **deletes the row** — a codified rule's row is duplication (owner ruling 2026-08-30); git history holds the past. Empty is the healthy state. Global rules are never authored locally. (Format: manager-templates T2.)
- **MGR-10** `M0-laneindex.md` — the module map, one line per lane with folder + Linear + board links. It wins locally; the HQ registry wins above it. Every shipped lane gets its line the day it ships. Over the size guard → spine + leaf split (structure-law).
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

## 4 · Moved (structure-law class homes)

- File templates (session · register · vocabulary · laneindex · repo CLAUDE.md skeleton + never-list) → **`manager-templates.md`** — read at creation.
- New-repo spin-up + internal move → **/repospinup** skill · transfer/sale → **/repotransfer** skill.
- Junction repair → **`boot-law.md`** BOOT-4.
- Status transitions → **manager-rules MGR-44** (law, not procedure).
