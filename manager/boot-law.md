# Boot law

`BOOT LAW v1 LOADED`

The one home for how a session boots, in every repo. Act-scoped law (structure-law §2): loaded by /start and /startceo at step 0 — the boot skills cite this file and keep only their own output shape. Consolidated 2026-08-30 from the two byte-identical skill preambles (an internal ticket).

## BOOT-1 · MCP connection gate

Four mandatory servers, matched by tool-name prefix (exact server names vary by install):

| Server | Expected tool prefix |
|---|---|
| Linear | `mcp__claude_ai_Linear__` |
| Bright Data | `mcp__bright-data__` |
| Cloudflare | `mcp__cloudflare` |
| Figma | `mcp__plugin_figma` (or `mcp__figma`) |

All four present → say nothing, continue. **Any missing → stop the boot.** Name the missing server(s), tell Raze to connect (`/mcp`, or `claude mcp add`), ask for a session restart. Never boot on a partial toolset — a boot that silently skips a dead connection hides the failure until mid-work. A server still connecting is retried via tool search before being declared missing.

## BOOT-2 · Boot-file checklist

The brief opens with the checklist of every boot file, actual state never assumed: ONLY if the content is really in context this session; anything absent, unreadable, or drifted carries its finding on the line (,,). A failed line is reported, never worked around. `session.md`'s date is printed; not today = stale, the Linear pull overrides it (LIN-8).

## BOOT-3 · Drift + junction checks (HQ boot)

- **globalclaude drift:** compare authored `<your-hq-repo>\globalclaude.md` against live `~/.claude/CLAUDE.md`. Drifted → one line + the push command: `Copy-Item "$HOME\Documents\GitHub\<your-hq-repo>\globalclaude.md" "$HOME\.claude\CLAUDE.md" -Force`
- **Skill junctions:** derived from what is on disk (`globalskills\*` + repo-authored globals), never a hardcoded list — a new skill folder is checked the day it is authored. Each `~/.claude/skills/<name>` must be a Junction to its authored source.
- **Agents junction:** `~/.claude/agents` → `<your-hq-repo>\HQ-M5-team\HQ-M5.3-globalagents`.

## BOOT-4 · Junction repair (recovery)

Symptom: a handbook/seat sentinel missing at boot or dispatch. `Get-Item "<path>" | Select LinkType` — if not `Junction`, delete the stale folder (first: no live process from that path, registry `runtime:` clear — OPS-16) and recreate:

- repo team (**operator repos only — never HQ**, which reads the corpus at its real path `HQ-M5-team/team/`, registry HQ-M5.1): `New-Item -ItemType Junction -Path "<repo>\team" -Target "$HOME\Documents\GitHub\<your-hq-repo>\HQ-M5-team\team"`
- a skill: `New-Item -ItemType Junction -Path "$HOME\.claude\skills\<name>" -Target "$HOME\Documents\GitHub\<your-hq-repo>\globalskills\<name>"`
- agents: `New-Item -ItemType Junction -Path "$HOME\.claude\agents" -Target "$HOME\Documents\GitHub\<your-hq-repo>\HQ-M5-team\HQ-M5.3-globalagents"`

## BOOT-5 · Skills roster

Discovered, never remembered: list `~/.claude/skills\*` (global) + the repo's `.claude/skills\*` (repo-local); each intro comes from that skill's `SKILL.md` frontmatter `description`, one line each. A skill folder with no `SKILL.md`, or a global skill whose path is not a junction, is a finding on its own row.

## BOOT-6 · The pull

Per linear-law: LIN-1 (team + cycle), LIN-2 (cycle number from the pull), LIN-3 (carry-over line), LIN-4 (Done age-out), LIN-8 (Linear wins), LIN-11 (scope by seat).
