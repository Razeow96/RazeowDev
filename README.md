# RazeowDev — an AI dev-team + operator rulebook

Rule corpus for running a **solo founder + AI dev team**: one human product owner, a main-loop team lead, and dispatched AI seat agents (architect · database · backend · frontend · ux/ui · devops · security · qa · tester), each governed by its own rulebook.

Battle-tested in a multi-project solo-AI organization; published as-is. This repo is a **read-only mirror** — it is generated from a private upstream on every rule change. Issues and PRs are welcome as proposals; accepted changes are ported upstream and flow back out here.

## Layout

```
devteam/    one rulebook per seat + teamlead.md (the entry point) + stacks/ (platform hazard packs)
manager/    operator law (manager-rules.md) + operating SOP (managerSOP.md)
templates/  sync-team.yml — copy into your repo for automatic update PRs
```

Start by reading `devteam/teamlead.md` — it is the entry point: leading principles first, then roster, addressing, routing, build loop, and how the rule system itself works.

## Use it

**One-time copy:**

```bash
git clone https://github.com/Razeow96/RazeowDev
mkdir -p your-repo/team
cp -r RazeowDev/devteam RazeowDev/manager your-repo/team/
```

**Stay updated (recommended):** copy `templates/sync-team.yml` into your repo's `.github/workflows/`. Weekly (or on manual trigger) it opens a **pull request** in your repo when the rules here have changed — you read the rule diff and merge deliberately. Rules are law, not a dependency; nothing should change your team's law silently.

**Pin a version:** every publish is tagged (`vYYYY.MM.DD-<sha>`); check `CHANGELOG.md` for what changed.

## Adapting

- The rulebooks assume the `/team` folder layout above; keep it and the cross-references just work.
- `manager/managerSOP.md` references a private HQ repo in a few procedures — substitute your own org's equivalents.
- Rules use MUST/NEVER/SHOULD grammar with per-rule IDs (`LEAD-1`, `DB-3`, …). Deviating from a MUST requires an inline `<ID>-ok: <reason>` marker at the site — the grammar is defined in `devteam/teamlead.md` §6.
- Using Claude Code? Wire each seat rulebook to a subagent via a thin wrapper in `~/.claude/agents/` whose only read instruction is its own seat file — the dispatch brief carries all repo context.

## License

MIT — see `LICENSE`.
