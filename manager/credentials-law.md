# Credentials law

Type: Law · where credential values live and how every other surface refers to them, in every repo and seat; pulled through rulesindex whenever a credential is created, stored, rotated, reported or leaked, and on dispatch of the security, devops and backend seats. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/governance-law-rationale.md` §credentials-law, same rule IDs, open when a rule's judgment is in play.

- **MGR-32** Credential values exist only in gitignored `.env` files under the repo's `/private` folder (`*.env` listed in that repo's `.gitignore`), in Cloudflare secrets for deployed Workers, in the HQ Brain's Supabase Vault (written only through `vault_set`) for its runtime tokens, and in mode-600 files outside any repo on a server host (VPS); no other store, and no recovery layer (Bitwarden) until after go-to-market.
- **CRED-1** A credential value never appears in a tracked file, a doc, a ticket, a commit, a log or chat; a tracked file records only what exists, where it lives, its purpose, its owner and the project that uses it.
- **CRED-2** Every repo keeps a committed `.env.example` with one line per key and an empty value.
- **CRED-3** A repo card lists its credential surfaces as paths only (the secret-bearing files and folders, each gitignore-covered).
- **CRED-4** A secret is reported as location + type + first 6 characters, masked, and a value is never echoed back, not even to the owner.
- **SEC-4** Secrets live in the secret store/env ONLY (CRED-1) — never the browser, never SQL (the Brain's `vault_set` is the one exception), never CLI arguments (rotate any key that has ever been one), never shared across environments — and config duplicated across env and worker secret is enumerated at its definition site and rotated in lockstep.
- **MGR-42** `bypassPermissions` is never the `defaultMode` in `.claude/settings.json` of any repo carrying credentials.
- **SEC-22** A leak is answered in this order — rotate the credential FIRST, then assess blast radius, then close the hole — using the rotation procedure kept current for drills (OPS-24), and any secret that has ever been exposed is rotated, never judged "probably fine".
