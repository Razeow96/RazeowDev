# Structure law — doc classes

`STRUCT LAW v1 LOADED`

The org's filing constitution: every doc belongs to exactly ONE of six classes, and the class fixes how it loads and how it grows. Read at the act — whenever a doc is created, filed, split, or audited. Approved by Raze 2026-08-30 (an internal ticket).

## 1 · The six classes

| Class | Test | Load behavior | Growth behavior |
|---|---|---|---|
| **LAW** | must the reader obey it while acting? | always-loaded **on the seat it binds** (org → everyone · seat → that seat's boot/dispatch · domain → folder entry). See §2 for standing vs act-scoped | dedup first (cite, never restate), then split **by seat or scope** — a new seat file or nested domain card. Law never gets an overflow file, and never gains a file from size alone |
| **SKILL** | executable steps for one discrete act? | on invocation — the skill is the loader for any act-scoped law it needs; prints Raze's steps where hands are required | over the guard = two acts in one skill → split by act; law inside a skill → replace with a citation |
| **TEMPLATE** | a fixed file shape? | read at creation moment only | templates don't grow; explanation crept in → move it to law or reference |
| **REFERENCE** | a fact you look up? | pull on demand, addressed by filename/section; the registry wins disagreements | **unbounded growth lands here by design.** One file over the guard → spine + leaf split: small index stays, sections become files addressed by name |
| **LEARNED** | a confirmed rule not yet verified in its home? | inbox — at registration + at sweep; **delete-on-codified** (a codified rule's row is duplication); numbers never reused (`next: #N` counter); empty is healthy | never grows; a full inbox = overdue sweep — an audit finding, never a split |
| **STATE** | what is true now? | `session.md` always (≤6,000 chars, replaced never appended); ledgers/Brain pulled | never grows — replaced. An append-log state file is a violation |

Exceeding a guard **never creates a new class** — it splits inside its class by that class's rule. A new class exists only when a genuinely new load behavior appears, and only on Raze's ruling.

## 2 · LAW load modes

- **Standing law** can bind on ANY turn (decision authority, hard constraints, communication, a domain's invariants while inside it) → carried always by the seat it binds.
- **Act-scoped law** binds only during one recognizable act (writing to Linear → `linear-law.md` · booting → `boot-law.md` · filing a doc → this file · authoring a rule → `devteam-rulelaw.md`) → loaded at the act's start **by the skill or step that performs the act** — never left to memory, never carried outside the act.
- **A law file's boundary follows its readers.** Same readers → same file. Different readers (e.g. Linear law: CEO skills + subsidiary skills + Laura) → separate file each cites. Never home a law where one of its readers must inherit someone else's law to reach it.

## 3 · The size guard

**≤ 4k tokens (≈ 16,000 chars) per loaded doc = safe · above = risk, split it by its class's growth rule** (owner-simplified 2026-08-30, one line, no tiers). `session.md` guard: 6,000 chars (MGR-8). `/loadstatus` measures; `/auditceo` P2 enforces.

## 4 · Dedup law

One rule, one home (MGR-11) — everything else cites the ID. **HQ is the master parent:** a rule duplicated between a repo and the HQ corpus resolves in HQ's favour — delete the repo copy, or promote it up first, after which the local copy still deletes. Cleanup deletes duplication and dead/expired text outright — never a pointer-copy, never a tombstone row. Cleanup scans run one subagent per doc so none is overloaded enough to miss one.

## 5 · Storage law

**Markdown is the law.** Rules live in junction-fresh `.md` files loaded in context — agents never fetch rules from SQL · the memory dir is never a home for law · Brain SQL holds lifecycle ledgers and state only.

## 6 · Folder → class map

| Where | Class |
|---|---|
| `~/.claude/CLAUDE.md` (globalclaude) · repo `CLAUDE.md` cards · nested domain `CLAUDE.md` · `team/manager/*-rules|*-law` · `team/devteam/<seat>.md` + `stacks/` · seat cards (`HQ-M5-team/<seat>/`) | LAW |
| `globalskills/` · `.claude/skills/` · agent wrappers (`globalagents/`) | SKILL |
| `team/manager/manager-templates.md` · `team/devteam/dispatch-brief.md` | TEMPLATE |
| registry (`HQ-M1-org-chart`) · lane indexes · `team/devteam/*-reference|*-topology|*-perimeter|*-threatmodel` · `docs/` · readmes · `vocabulary.md` | REFERENCE |
| `learnrules.md` / `learnedrules.md` per repo | LEARNED |
| `session.md` · fin ledger · Brain tables | STATE |

## 7 · Folder & item law (approved 2026-09-02)

How item codes land on disk — one rule set, every module, every repo:

1. **An item with 2+ files gets an item folder** `<code>-<slug>/`; a **single-file item stays flat** as `<code>-<slug>.ext` in the module folder. No folders for one file.
2. **Migrations are sub-item files inside the data-layer item's folder** (e.g. `HQ-M4.1-data-layer/HQ-M4.1.2-….sql`) — never module items.
3. **Shipped / junction-distributed assets keep their real names** — the code stays registry metadata (§6 code-don't-rename). The canonical case: the `team/` corpus (M5.1) — junction-mounted into operator repos as the wired address, published publicly — never carries a code prefix, outside or inside.
4. **Every module folder carries its root doc** `<module-code>-<slug>.md` with the item index and any old→new code map (§7.6).

## 8 · Enforcement

`/loadstatus` — measures every seat's real load against §3. `/auditceo` P2 — per doc, one mechanical check: *does this doc match its class's load and growth behavior?* A doc fitting no class, or two, is the split signal. The close sweep keeps LEARNED draining. Findings are reported; fixes execute on Raze's verdicts.
