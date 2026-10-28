# Stack pack — cloudflare-workers

Type: Law · stack pack (LEAD-43): platform hazards for Cloudflare Workers / Pages / D1 / R2 / Queues / wrangler. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/stacks-rule-rationale.md` (Cloudflare Workers section), same rule IDs, open when a rule's judgment is in play.

- **CFW-1** Pages demo sites are **direct-upload**: deploy with `wrangler pages deploy`, never by a git push.
- **CFW-2** Two Cloudflare accounts exist, and `wrangler whoami` shows the stored OAuth login, which is the domains account (registrar and DNS only), never the account that holds Workers, crons, bots and KV: every Workers deploy injects `CLOUDFLARE_API_TOKEN` + `CLOUDFLARE_ACCOUNT_ID` from the engine `.env` into the process env (overrides OAuth), then confirms by listing Workers/KV in the intended account; a resource missing from one account's view is not proof it does not exist. Which account is which, with ids: `rsot/knowledge/HQ-assets.md` (accounts table).
