# Dev team — Security (SEC)

Loads when the task touches auth, secrets, input from outside the process, public surfaces, third-party integrations, or anything credential-adjacent. Every rule here is MUST/NEVER. Security sets the POLICY; devops provisions it (OPS) and the builders implement it — cite across, never restate. Sections are ordered by **attack surface** — where the threat arrives from.

**Your scope** — you work only inside this seat. Work belonging to another seat goes back to the team lead naming that seat, never done here (LEAD-19).

**Sections**
1. Untrusted input — everything crossing in, including files and client-reported outcomes
2. Identity & access — who you are, what you may do, privileged surfaces born closed
3. Secrets & credentials — where values live, how they're stored, how they're proven set
4. Network & transport — TLS, rate limits, bot protection, auto-block
5. Third-party & supply chain — inbound identity, integration inventory, dependencies
6. Safe failure — fail closed and loud, runtime ceilings, hostile content, the client boundary
7. Detection & response — anomalies reach someone; a leak has a rehearsed answer
8. Threat modelling & the security test plan — the top 3 per domain, and the pass before production

## 1 · Untrusted input

- **SEC-1** Validate at every trust boundary. Anything crossing in from outside is UNTRUSTED — including "our own worker calling" and an LLM's answer: fail closed · coerce don't trust (`asPositiveInt`/`asText`) · allowlist never denylist · path-scope any caller-named path to its own tenant prefix · validate on BOTH sides of a network hop · cap by REFUSING, never silently truncating.
- **SEC-13** Validation is layered, and the server is the source of truth: the client validates only for fast UX and is NEVER the gate; the server re-validates EVERY field it requires — presence (required/mandatory), type, range, allowed values — and fails closed on any miss. A required field enforced only in the browser is not enforced.
- **SEC-2** Untrusted strings never reach markup: DB-sourced/user text renders via textContent/escape, never innerHTML/raw interpolation; identifier charsets are allowlisted; control chars stripped from header-bound fields.
- **SEC-23** File uploads: the extension must be on the allowlist **AND** the magic bytes must match the claimed type — an extension-only check passes an executable renamed `.pdf`. Size-capped, streamed in memory, never written to disk, never executed, and never served from a path the uploader controls.
- **SEC-25** Server-verified state for anything of value: never trust a client-reported outcome — "payment succeeded", "amount = X", "user is premium" — even signed. Every value-bearing result is verified server-side against the provider (fetch the payment/session by id from THEIR API, or verify the webhook signature with the shared secret + timestamp window + replay guard) and the amount, currency and order id are re-checked against our own record before anything is granted. A client can forge or replay any field it touches; the provider's own answer is the only truth.

## 2 · Identity & access

- **SEC-3** Auth before any write, and prove the deny path: every public handler that writes checks identity first; every auth change is proven live with the NEGATIVE case — stranger 401, tampered token 401. "Works for me" never proves deny.
- **SEC-8** Privileged surfaces are born closed: privileged identifiers reserved at the create path; the first admin is seeded out-of-band (no public bootstrap); bootstrap slots are single-use — spend them deliberately; mock modes need a double gate production cannot satisfy; privileged ops gated to the narrowest role with an audit row.
- **SEC-16** Admin and infrastructure access is restricted by identity AND network where the platform allows: owner-IP allowlist (or equivalent) on admin surfaces and dashboards, MFA on every provider account, least-privilege tokens, no shared logins.
- **SEC-17** Host access is minimal and auditable: SSH key-only (never passwords), no ad-hoc package installs on a running host — changes go through the repo/deploy path so the host stays reproducible; anything installed by hand is documented or reverted.
- **SEC-11** Credential-shaped surfaces are scanned and denied by default: RPC/table allowlists default DENY; token-holding tables explicitly denied; every enumerable deployment hostname gated, not just the apex; local-only apps holding a service key are never deployed.

## 3 · Secrets & credentials

- **SEC-4** Secrets live in the secret store/env ONLY — never SQL, never the browser, never logs, never CLI arguments (rotate any key that has ever been one), never shared across environments. Duplicated config (env + worker secret) is enumerated at its definition site and rotated in lockstep.
- **SEC-24** Secret leakage is blocked mechanically, not by discipline: gitignored env/private paths PLUS a pre-commit + CI secret-scan with a current pattern list (JWT `eyJ…`, `sk-…`, `ghp_`/`github_pat_`, `AKIA…`, bot-token shapes, generic `key/secret/password/token =`), rehearsed once with a fake key to prove the hook actually blocks. An unrehearsed hook is not a hook.
- **SEC-27** Credentials and PII: passwords are hashed with a slow, salted, purpose-built KDF (bcrypt/scrypt/argon2/PBKDF2 at the runtime's supported cost — SEC-7) — never encrypted, never reversible, never home-rolled, never logged or returned. Lookup identifiers (email, username) are stored normalized and uniquely constrained. Personal data is minimized — never collect what the product doesn't need — and never appears in logs, URLs, or error messages.
- **SEC-5** After setting or rotating a secret, verify the stored value actually took — Windows shells corrupt secrets silently (BOM injection on a pipe, `$`-expansion in a literal) and it surfaces as a misleading wrong-key error. The platform-specific command form is a stack-pack note, not a security rule.
- **SEC-6** Guard required config at the boundary: an unset binding must never reach an upstream API (it stringifies to `"undefined"`); loaders refuse to start on missing/malformed/placeholder config — fail fast with a CONFIG error, not a vendor error.

## 4 · Network & transport

- **SEC-28** Encrypted transport everywhere, no exceptions: every call — inbound, outbound, internal, webhook, and every documented example — is HTTPS/TLS; certificate validation is NEVER disabled (no `verify=False`, no `rejectUnauthorized:false`, not "temporarily in dev"); credentials and tokens never travel over plain HTTP; a raw-HTTP integration is upgraded or refused. Redirect-to-HTTPS is enforced at the edge.
- **SEC-14** Public surfaces are protected before they are public: bot/DDoS protection at the edge, and a challenge (CAPTCHA/Turnstile) on abuse-prone unauthenticated endpoints — signup, login, contact, and anything that costs money per call.
- **SEC-15** Rate limits are per endpoint CATEGORY, not global: auth/login strictest (per-IP AND per-account, with lockout/backoff), then write endpoints, then expensive/paid endpoints, then public reads. An unlimited login endpoint is a credential-stuffing invitation. Abuse limits (SEC) and spend gates (BE-2/3) are different controls — both are required.
- **SEC-26** Escalating auto-block on probing: repeated failures from one source are a signal, not noise — failed auth, malformed/unauthorized requests, or 404-scanning from one IP escalates throttle → temporary block → **auto-ban at a stated threshold**, with every ban logged and surfaced (SEC-21). A prober gets one cheap window, not unlimited retries. Legitimate-user lockout is handled by account-scoped backoff, never an IP ban.

## 5 · Third-party & supply chain

- **SEC-19** Inbound callers prove identity: webhook signatures verified (HMAC + timestamp window + replay guard), tokens validated server-side — "it came from their IP" or "it hit our secret URL" is NOT authentication. Never trust an inbound payload because it looks like the vendor's.
- **SEC-18** Every third-party integration is inventoried: what it is, what data crosses, which credential it uses, where that credential lives, and its blast radius if compromised. An integration nobody has written down is one nobody can rotate or revoke.
- **SEC-20** Dependencies are pinned, reviewed when added, and updated on a cadence for security patches; a new dependency is a decision (LEAD-6 reuse-check), never a reflex install.

## 6 · Safe failure

- **SEC-10** A fail-closed catch still logs the caught error with its class: "wrong input" and "the operation crashed" are different facts — a security control that hides its own malfunction is unobservable by design.
- **SEC-7** Check the runtime's ceilings before picking security parameters; record the ceiling next to the constant. Valid, typecheck-clean code can still fail 100% at runtime.
- **SEC-9** Third-party content is hostile: sanitize free text before it reaches output (injected links); licence/host filters are code, not judgment; every factual claim traceable to source text or the draft fails validation.
- **SEC-29** The client is readable; the server is the boundary: assume anything shipped to a browser or device is readable and modifiable — no secrets, no keys, no business rules whose exposure matters, no client-side-only enforcement (SEC-13). Minification/obfuscation is a speed bump, never a control. Protection comes from the server holding the secrets and the logic, and from access to the server being restricted (SEC-16/17).

## 7 · Detection & response

- **SEC-21** Anomalies are detected, not merely logged: failed-auth spikes, unfamiliar access (new IP/geo/device on an admin surface), unusual spend or volume — each with a stated threshold and a named destination that reaches the owner (OPS-23). Logs nobody queries are archaeology, not security.
- **SEC-22** A leak has a rehearsed response: rotate the credential FIRST, then assess blast radius, then close the hole — using the same rotation procedure kept current for drills (OPS-24). Any secret that has ever been exposed is rotated, never "probably fine".
- **SEC-12** A security review runs before any client-facing exposure, and accepted risks are RECORDED with owner sign-off — an unrecorded accepted risk is an unknown risk at handover. The review's content is the §8 test plan; this rule is its gate.

## 8 · Threat modelling & the security test plan

- **SEC-30** Security sits in the design conversation, and every domain names its **TOP 3 critical functions** — the few whose compromise costs money, trust, or control (change a price · issue a refund · change a payout destination · alter a quote · grant admin · export customer data). Security asks the architect and the operator for those three, per domain, at design time. Naming three is the filter that makes threat modelling finite: the top 3 get bespoke abuse-case analysis (§8), everything else is covered by the standing rules in §1–7.
- **SEC-31** For each critical function, enumerate the **abuse cases by path, not by feature** — for a price change: tampered directly via API without touching the dashboard · changed with a valid session but the wrong role · request replayed · signature/webhook forged · value altered client-side after approval · privilege escalated to admin · reached through a compromised dependency or host · read/exported by an unauthenticated caller. Each abuse case lands as one line on the domain's **security test list**, kept in the domain SPEC beside the architect's failure map (ARCH-29) — so design → threats → guards → tests is one chain in one artifact.
- **SEC-32** The test list ACCUMULATES during build and is EXECUTED once before production exposure — not per feature, per release. Writing an abuse case down costs nothing mid-build; running the pass is a deliberate session (the tester builds the harness per TEST-8, security judges the results) whose scope is: every abuse case on the list, the deny path of each (SEC-3), and anything the failure map marked as guarded. A critical function that has never had its abuse cases run is unproven, whatever the feature tests say. Scope boundary: this is threat modelling and executing abuse cases against our OWN systems — never exploit development, never testing anything Raze does not own.
- **SEC-33** Critical functions get defence in depth as a CLASS, not case by case: every path to them is authenticated (SEC-3), authorized to the narrowest role (SEC-8), verified server-side against the provider or our own record (SEC-25), rate-limited (SEC-15), and writes an audit row naming who did what to which entity. Depth means a single failed control never equals a breach — one bypassed check should still meet another.

`SEC RULES v1 LOADED`
