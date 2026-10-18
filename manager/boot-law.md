# Boot law

Type: Law · how a session boots, in every repo; pulled through rulesindex by /start and /startceo at step 0 — the boot skills keep only their own output shape. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/governance-law-rationale.md` §boot-law, same rule IDs, open when a rule's judgment is in play.

## MGR-1 · Boot order

The main-loop seat boots global card → repo card → rulesindex · knowledgeindex · repolaneindex → session.md (read by the boot skill) → act, pulling the rule file the act names; nothing else loads at boot (the boot files are BOOT-2's list), and a file that is empty or missing is noted, never silently skipped.

## BOOT-1 · MCP connection gate

All three servers below must be present, matched by tool-name prefix, or the boot stops: name the missing server(s), tell Raze to connect (`/mcp`, or `claude mcp add`) and ask for a session restart; all present → continue silently; a server still connecting is retried via tool search before being declared missing; Bright Data is never a boot server, and a dead Bright Data connection stops only the marketing or research task that needs it.

| Server | Expected tool prefix |
|---|---|
| Linear | `mcp__claude_ai_Linear__` |
| Cloudflare | `mcp__cloudflare` |
| Figma | `mcp__plugin_figma` (or `mcp__figma`) |

## BOOT-2 · Boot-file checklist

Every boot skill prints and verifies this one list, never a copy of its own (/loadstatus reads it too, monthly inside /auditceo, never at boot), opening the brief with a line per boot file marked ONLY if its content is really in context this session, else its finding (,,), a failed line reported and never worked around, and `session.md`'s date printed (not today = stale; the Linear pull overrides it, LIN-8).

- **Main loop, in load order:** `~/.claude/CLAUDE.md` (global card) · `CLAUDE.md` (repo card) · `rulesindex.md` · `knowledgeindex.md` · `repolaneindex.md` · `session.md` · `learnrulesinbox.md`.
- **Loaded by Claude Code itself, not by the skill:** skill descriptions · agent descriptions · auto-memory `MEMORY.md` · any `.claude/rules/*.md` without a `paths:` header.
- **Dispatched seat** (`omitClaudeMd: true` on every seat card): the brief · seat card · seat rule file (+ stack pack) · the knowledge files its rules cite, on request. No global card, no repo card, no indexes: the brief carries the repo facts the task needs.
- **Never at boot:** rule files, knowledge files, module lane indexes, module code indexes, path rules — each loads at the act, folder or module that binds it.

## BOOT-3 · Drift + link checks

The boot runs every check below, repairs what the line names itself and reports one line per finding.

- **globalclaude drift (HQ boot only):** authored `<your-hq-repo>\globalclaude.md` vs live `~/.claude/CLAUDE.md`; drifted → the boot copies authored over live itself, compares again and reports the result in one line.
- **Skill junctions:** derived from what is on disk (`globalskills\*` + repo-authored globals), never a hardcoded list; each `~/.claude/skills/<name>` must be a Junction to its authored source (MGR-17).
- **Agents junction:** `~/.claude/agents` → `<your-hq-repo>\HQ-M5-team\HQ-M5.3-globalagents`.
- **orgrules link (company repos):** `<repo>\orgrules` is a Junction to HQ `rsot\rules`; missing or not a Junction → repair per BOOT-4.
- **Org drift (HQ boot):** the repolaneindex company rows reconciled against the repos on disk and the Linear projects, one warning line per mismatch; container-only skill folders are never part of the roster check (FILE-17).
- **Non-Claude mirror:** any `AGENTS.md` that differs from its `CLAUDE.md` source (or `~/.agents/AGENTS.md` from `globalclaude.md`) is drift → rerun `HQ-M2-tooling\HQ-M2.6-agents-mirror.ps1`.

## BOOT-4 · Junction repair (recovery)

An `orgrules/`, skill folder or agents folder that is missing or not a Junction at boot or dispatch is deleted (first: OPS-16) and recreated as a Junction per `rsot/knowledge/junction-repair.md`.

## BOOT-5 · Skills roster

The skills roster is checked per `rsot/knowledge/boot-skills-roster.md`.

## BOOT-6 · The pull

The pull runs per linear-law: LIN-1 (team + cycle), LIN-2 (cycle number from the pull), LIN-3 (carry-over line), LIN-4 (Done age-out), LIN-8 (task state is Linear's, session.md holds exists · blocked · next), LIN-11 (scope by seat).
