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
- **SEC-24** Secret leakage is blocked mechanically, not by discipline: gitignored env/private paths PLUS a pre-commit + CI secret-scan with a current pattern list, rehearsed once with a fake key to prove the hook actually blocks. An unrehearsed hook is not a hook. Reference: `security-perimeter.md` — read when building or auditing the scan's pattern list.
- **SEC-27** Credentials and PII: passwords are hashed with a slow, salted, purpose-built KDF (bcrypt/scrypt/argon2/PBKDF2 at the runtime's supported cost — SEC-7) — never encrypted, never reversible, never home-rolled, never logged or returned. Lookup identifiers (email, username) are stored normalized and uniquely constrained. Personal data is minimized — never collect what the product doesn't need — and never appears in logs, URLs, or error messages.
- **SEC-5** After setting or rotating a secret, verify the stored value actually took — Windows shells corrupt secrets silently (BOM injection on a pipe, `$`-expansion in a literal) and it surfaces as a misleading wrong-key error. The platform-specific command form is a stack-pack note, not a security rule.
- **SEC-6** Guard required config at the boundary: an unset binding must never reach an upstream API (it stringifies to `"undefined"`); loaders refuse to start on missing/malformed/placeholder config — fail fast with a CONFIG error, not a vendor error.

## 4 · Network & transport

Reference: `security-perimeter.md` (SEC-28 · SEC-14 · SEC-15 · SEC-26) — read when the review scope includes network, transport, rate limiting or auto-block.

## 5 · Third-party & supply chain

- **SEC-19** Inbound callers prove identity — signatures verified, tokens validated server-side; "it came from their IP" is NOT authentication.
- Reference: `security-perimeter.md` (SEC-19 in full · SEC-18 · SEC-20) — read when the review scope includes a third-party integration or a dependency change.

## 6 · Safe failure

- **SEC-10** A fail-closed catch still logs the caught error with its class: "wrong input" and "the operation crashed" are different facts — a security control that hides its own malfunction is unobservable by design.
- **SEC-7** Check the runtime's ceilings before picking security parameters; record the ceiling next to the constant. Valid, typecheck-clean code can still fail 100% at runtime.
- **SEC-9** Third-party content is hostile: sanitize free text before it reaches output (injected links); licence/host filters are code, not judgment; every factual claim traceable to source text or the draft fails validation.
- **SEC-29** The client is readable; the server is the boundary: assume anything shipped to a browser or device is readable and modifiable — no secrets, no keys, no business rules whose exposure matters, no client-side-only enforcement (SEC-13). Minification/obfuscation is a speed bump, never a control. Protection comes from the server holding the secrets and the logic, and from access to the server being restricted (SEC-16/17).

## 7 · Detection & response

- **SEC-12** A security review runs before any client-facing exposure, and accepted risks are RECORDED with owner sign-off. The review's content is the §8 test plan; this rule is its gate.
- Reference: `security-perimeter.md` (SEC-21 · SEC-22 · SEC-12 in full) — read when the review scope includes anomaly detection, alert thresholds or a leak response.

## 8 · Threat modelling & the security test plan

Reference: `security-threatmodel.md` (SEC-30 · SEC-31 · SEC-32) — read when running a critical-function review: naming the top 3, enumerating abuse cases, or executing the pre-production pass.

- **SEC-34** Credential topology is the architect's call, not a review finding — cite **ARCH-35** (doorman + gate) rather than restating it. The security seat's job here is to check it was applied and that the residual list is real: that no route returns a credential, that the gate judges the change and not the requester, and that a control filtering by origin (IP allow-list, firewall, SSH key, a signature made with a key the caller can read) is never counted as the control that stops an insider.
- **SEC-33** Critical functions get defence in depth as a CLASS, not case by case: every path to them is authenticated (SEC-3), authorized to the narrowest role (SEC-8), verified server-side against the provider or our own record (SEC-25), rate-limited (SEC-15), and writes an audit row naming who did what to which entity. Depth means a single failed control never equals a breach — one bypassed check should still meet another.

`SEC RULES v1 LOADED`
