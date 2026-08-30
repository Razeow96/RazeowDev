# Dev team — Security perimeter (SEC §4 · §5 · §7)

REFERENCE class (structure-law): read when the review scope is network/transport, third-party and supply chain, or detection and response; the law lives in `security.md`.

## §4 · Network & transport

- **SEC-28** Encrypted transport everywhere, no exceptions: every call — inbound, outbound, internal, webhook, and every documented example — is HTTPS/TLS; certificate validation is NEVER disabled (no `verify=False`, no `rejectUnauthorized:false`, not "temporarily in dev"); credentials and tokens never travel over plain HTTP; a raw-HTTP integration is upgraded or refused. Redirect-to-HTTPS is enforced at the edge.
- **SEC-14** Public surfaces are protected before they are public: bot/DDoS protection at the edge, and a challenge (CAPTCHA/Turnstile) on abuse-prone unauthenticated endpoints — signup, login, contact, and anything that costs money per call.
- **SEC-15** Rate limits are per endpoint CATEGORY, not global: auth/login strictest (per-IP AND per-account, with lockout/backoff), then write endpoints, then expensive/paid endpoints, then public reads. An unlimited login endpoint is a credential-stuffing invitation. Abuse limits (SEC) and spend gates (BE-2/3) are different controls — both are required.
- **SEC-26** Escalating auto-block on probing: repeated failures from one source are a signal, not noise — failed auth, malformed/unauthorized requests, or 404-scanning from one IP escalates throttle → temporary block → **auto-ban at a stated threshold**, with every ban logged and surfaced (SEC-21). A prober gets one cheap window, not unlimited retries. Legitimate-user lockout is handled by account-scoped backoff, never an IP ban.

## §5 · Third-party & supply chain

- **SEC-19** Inbound callers prove identity: webhook signatures verified (HMAC + timestamp window + replay guard), tokens validated server-side — "it came from their IP" or "it hit our secret URL" is NOT authentication. Never trust an inbound payload because it looks like the vendor's.
- **SEC-18** Every third-party integration is inventoried: what it is, what data crosses, which credential it uses, where that credential lives, and its blast radius if compromised. An integration nobody has written down is one nobody can rotate or revoke.
- **SEC-20** Dependencies are pinned, reviewed when added, and updated on a cadence for security patches; a new dependency is a decision (LEAD-6 reuse-check), never a reflex install.

## §7 · Detection & response

- **SEC-21** Anomalies are detected, not merely logged: failed-auth spikes, unfamiliar access (new IP/geo/device on an admin surface), unusual spend or volume — each with a stated threshold and a named destination that reaches the owner (OPS-23). Logs nobody queries are archaeology, not security.
- **SEC-22** A leak has a rehearsed response: rotate the credential FIRST, then assess blast radius, then close the hole — using the same rotation procedure kept current for drills (OPS-24). Any secret that has ever been exposed is rotated, never "probably fine".
- **SEC-12** A security review runs before any client-facing exposure, and accepted risks are RECORDED with owner sign-off — an unrecorded accepted risk is an unknown risk at handover. The review's content is the §8 test plan; this rule is its gate.

## Secret-scan pattern list (SEC-24)

The pre-commit + CI secret-scan carries a current pattern list: JWT `eyJ…` · `sk-…` · `ghp_`/`github_pat_` · `AKIA…` · bot-token shapes · generic `key/secret/password/token =`.

`SEC PERIMETER LOADED`
