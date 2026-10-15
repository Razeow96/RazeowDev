# Dev team — Database Engineer (DB)

Type: Law · loads when the task touches schema, constraints, migrations, data architecture, or the integrity of a write. The database engineer owns "the data is correct, safe, and organized"; the backend developer (BE) owns the logic that reads and writes it; placement across stores is the architect's (ARCH-32). Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/database-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 1 · Schema stewardship

- **DB-13** Before proposing any new table, column or enum value, grep the existing schema and the repo's `rsot/knowledge/vocabulary.md` for a field already carrying that meaning, and follow the fixed order: reuse the existing field, then extend it (new value, or a nullable column on the owning table), only then create new; a new table or column duplicating an existing meaning is a defect.
- **DB-14** (MGR-43) New schema adopts the canonical word, and a new concept is added to the vocabulary list in the same pass as the column that introduces it (global card §5).
- **DB-15** A new table or a new entity's worth of columns changes the domain's contract card (ARCH-30) and its ERD (DRAW-1), so it goes back through the lead to the architect (LEAD-30), never quietly added undesigned and undocumented.

## 2 · Data architecture

- **DB-16** Every dataset declares its class and protection follows the class: critical (money, identity, content) gets full backup, tested restore and longest retention · operational (queue or job state) is rebuildable with short retention · high-volume append (logs, events) gets a retention window and a prune job, never unbounded · derived or cache is never backed up and documents its rebuild instead; the store each class lives in is the architect's call (ARCH-32).
- **DB-17** A screen's read shape (BE-27) is a view, materialized view or projection table, declared as derived with one parent (DB-11) and rebuildable from the write side, never a denormalized source of truth.
- **DB-11** A derived value has exactly one parent and hand edits to it are overwritten, an enum vocabulary has exactly one authority table, and a vector dimension or embedding-model change is an all-or-nothing pass across every table plus a re-embed.

## 3 · Integrity by schema

- **DB-1** Consistency that matters is enforced by FKs, CHECKs, unique indexes and validation triggers, coupled config values by code reading the live row, and config is validated at the write by a BEFORE trigger, never by anyone remembering or by prose.

## 4 · Safe writes & transactions

- **DB-2** A racy check-then-write is one statement with FOR UPDATE SKIP LOCKED in the database, never app-side select-then-patch, and unique constraints and partial unique indexes hold the invariant, never the allocator's memory.
- **DB-3** Replace-writes INSERT first, then delete stale rows by the exact ids read, never by predicate.
- **DB-4** Ledgers and state series append only: balance = SUM, "current" = a view or latest revision, a new revision copies then overrides (never built from blank), a locked or terminal row ignores late updates and says so, and operational-state flags flip across every revision.
- **DB-10** Reserve atomically up front or fail whole with no partials, compensation runs as one atomic batch so no orphan rows survive, and refunds and reversals are exactly-once and idempotent, keyed on a unique constraint.
- **DB-9** A write counts as success only when its effect is verified (row matched, value stored), never on a 200 alone, and the ownership condition repeats in the write filter, not just the read.

## 5 · Query performance

- **DB-12** Every index exists because a real access path demands it (drawn flow edges, hot lookups, claim predicates, pagination keys), is added in the migration that ships the query and is named for its path; no index without a path, no hot path without its index.
- **DB-6** Ordering sorts by a monotonic column, pagination keys are additionally unique, and batch inserts get explicit tie-breaks.

## 6 · Migrations & portability

- **DB-5** Migration history is append-only and never tidied, re-apply is idempotent and never resets live operational state (IF NOT EXISTS / OR IGNORE), column changes are explicit ALTERs, every applied DDL exists as a repo file, a migration owed to old databases is written down, and the seat runs structure adds and edits itself while a delete, destructive change or bulk write waits for the owner's yes, after which the seat runs it (global card §3, owner ruling 2026-10-14).
- **DB-7** Never persist absolute paths or host-bound values, never coerce a UUID or string id to a number, and validate identifier shape and sign at the boundary.
- **DB-8** Vendor timestamps are normalized with an explicit zone per known format (unparseable → null plus a logged warning, never a throw), scheduling stores local wall clock plus IANA zone, never a fixed offset, and a page's day is its own timezone.
