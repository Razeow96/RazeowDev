# Dev team — Security (SEC)

Type: Law · loads when the task touches auth, secrets, input from outside the process, public surfaces, third-party integrations, or anything credential-adjacent; every rule here is MUST/NEVER. Security sets the policy and, since 2026-10-19, builds the security tests (§9); devops provisions it (OPS) and the code builders implement it (FILE-12). Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/security-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 1 · Untrusted input

- **SEC-1** Everything crossing a trust boundary, our own worker and an LLM's answer included, is untrusted and validated there: fail closed, coerce never trust, allowlist never denylist, path-scope any caller-named path to its own tenant prefix, validate on both sides of a network hop, and cap outside input by refusing, never silently truncating — the cap applies to outside input only, never to internal batch sizes or loops (owner ruling 2026-10-20).
- **SEC-13** The server re-validates every field it requires (presence, type, range, allowed values) and fails closed on any miss; client validation is for UX only and is never the gate.
- **SEC-2** Untrusted strings never reach markup: DB-sourced or user text renders via textContent or escape, never innerHTML or raw interpolation, identifier charsets are allowlisted and control chars are stripped from header-bound fields.
- **SEC-23** An upload passes only when its extension is on the allowlist and its magic bytes match the claimed type, and it is size-capped, streamed in memory, never written to disk, never executed and never served from a path the uploader controls.
- **SEC-25** Never trust a client-reported value-bearing outcome, even signed: verify it server-side against the provider (fetch by id from their API, or the webhook signature, SEC-19) and re-check amount, currency and order id against our own record before anything is granted.

## 2 · Identity & access

- **SEC-3** Every public handler that writes checks identity first, and every auth change is proven live with the negative case (stranger 401, tampered token 401).
- **SEC-8** Privileged surfaces are born closed: privileged identifiers reserved at the create path, the first admin seeded out-of-band, bootstrap slots single-use and spent deliberately, mock modes behind a double gate production cannot satisfy, and privileged ops gated to the narrowest role with an audit row.
- **SEC-16** Admin and infrastructure access is restricted by identity and network where the platform allows: owner-IP allowlist or equivalent on admin surfaces and dashboards, MFA on every provider account, least-privilege tokens and no shared logins (devops provisions it, OPS §1/§3).
- **SEC-17** Host access is SSH key-only, never passwords, and no package is installed ad hoc on a running host; changes go through the repo and deploy path, and anything installed by hand is documented or reverted.
- **SEC-11** Credential-shaped surfaces are denied by default: RPC and table allowlists default deny, token-holding tables explicitly denied, every enumerable deployment hostname gated, not just the apex, and a local-only app holding a service key is never deployed.
- **SEC-35** Each ledger has exactly one write-holding agent seat with its own scoped credential; every other seat reads only.
- **SEC-38** A bot identity writing to a repository is never a code owner or ruleset bypass actor; its token lives one hour.

## 3 · Secrets & credentials

- **SEC-24** Secret leakage is blocked mechanically by gitignored env and private paths plus a pre-commit and CI secret-scan with a current pattern list (`rsot/knowledge/secret-scan-patterns.md`), rehearsed once with a fake key to prove the hook blocks before it counts.
- **SEC-27** Passwords are hashed with a slow, salted, purpose-built KDF (bcrypt, scrypt, argon2 or PBKDF2 at the runtime's supported cost, SEC-7), never encrypted, reversible or home-rolled (CRED-1); lookup identifiers are stored normalized and uniquely constrained; personal data is minimized and never appears in logs, URLs or error messages.
- **SEC-5** After setting or rotating a secret, verify the stored value actually took (WIN-7 · WIN-8).
- **SEC-6** An unset binding never reaches an upstream API; loaders refuse to start on missing, malformed or placeholder config and fail fast with a config error, not a vendor error.

## 4 · Network & transport

Reference: `security-perimeter.md` (SEC-28 · SEC-14 · SEC-15 · SEC-26) — read when the review scope includes network, transport, rate limiting or auto-block.

## 5 · Third-party & supply chain

- **SEC-36** A refused vendored skill is never copied in; absence is the enforcement, not a disabled flag.
- **SEC-37** Vendored executable scripts pass a security review before their first run; each external API they call needs its own allow-list entry.

- Reference: `security-perimeter.md` (SEC-19 in full · SEC-18 · SEC-20) — read when the review scope includes a third-party integration or a dependency change.

## 6 · Safe failure

- **SEC-10** A fail-closed catch still logs the caught error with its class, so wrong input and a crashed operation stay distinct.
- **SEC-7** Check the runtime's ceilings before picking security parameters and record the ceiling next to the constant (TEST-11).
- **SEC-9** Third-party content is hostile: free text is sanitized before it reaches output, licence and host filters are code, not judgment, and a draft with any factual claim not traceable to source text fails validation.
- **SEC-29** Anything shipped to a browser or device is treated as readable and modifiable, so it carries no secrets, no keys and no business rules whose exposure matters (SEC-13); minification or obfuscation is never a control, and protection lives on the server with access to it restricted (SEC-16 · SEC-17).

## 7 · Detection & response

- **SEC-12** The security test suite (§9) runs green before any client-facing exposure (accepted risks: LEAD-29); its content is the §8 test plan, QA compares its results with the built code (QA-23), and this rule is its gate.
- Reference: `security-perimeter.md` (SEC-21) — read when the review scope includes anomaly detection or alert thresholds; leak response: SEC-22 in `credentials-law.md`.

## 8 · Threat modelling & the security test plan

Reference: `security-threatmodel.md` (SEC-30 · SEC-31 · SEC-32) — read when running a critical-function review: naming the top 3, enumerating abuse cases, or executing the pre-production pass.

- **SEC-33** Every path to a critical function is authenticated (SEC-3), authorized to the narrowest role (SEC-8), verified server-side against the provider or our own record (SEC-25), rate-limited (SEC-15) and writes an audit row naming who did what to which entity.

## 9 · The security test builder

- **SEC-39** Every surface (route, workflow, button, file, table, message) carries a sensitivity class set by the architect in the design and stated in the brief: **S0 public** (published pages, public assets) · **S1 internal** (settings, run logs, reports, health) · **S2 business and personal data** (leads, contacts, email drafts, replies, owner decisions) · **S3 credentials, money and acting on the owner's behalf** (keys, spend, sends, approvals, pauses, deploys); the highest class a surface touches is its class (owner ruling 2026-10-19).
- **SEC-40** This seat is a builder: it writes every security test script from the lead's brief (LEAD-34), black-box, red on the old code first, and never writes product code; a surface with no class, or a security result not stated as a technical metric (status code and exact message, writes = 0, provider calls = 0, no secret or out-of-class field in output, limit N per window), is returned as `BRIEF INCOMPLETE` (owner ruling 2026-10-19).
- **SEC-41** Depth follows the class, basic to medium: **basic** (S0–S1) — no secret, token, key or stack trace in any output or log line; every outside input validated with a named 400; authentication deny on every non-public surface; **medium** (S2–S3) — basic plus authorization to the narrowest role or chat; injection (SQL, formula, HTML/markup, prompt) through every outside field; replay and double-submit producing one effect; tampered ids, oversized and malformed payloads; rate limit; no out-of-class field in any message; every SEC-31 abuse case of a critical function; spend caps hold.
- **SEC-42** A failing security test goes back through the lead to the builder as `FAIL <test> · SEC-n · expected <x> · got <y> · repro <command>`; this seat never weakens, skips or deletes a failing test, and an accepted risk is recorded by the lead with the owner's sign-off (LEAD-29), never by editing the test.
- **SEC-43** Security tests run against local fakes and test instances only; a check against a live production surface or a third party runs only with the lead's dated approval, small, and never attacks a service we do not own.
