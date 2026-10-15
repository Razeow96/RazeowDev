# Dev team — Security perimeter (SEC §4 · §5 · §7)

Type: Law (perimeter rules for `security.md`) · pulled through rulesindex when the review scope is network/transport, third-party and supply chain, or detection and response. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/security-perimeter-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## §4 · Network & transport

- **SEC-28** Every call — inbound, outbound, internal, webhook and every documented example — uses HTTPS/TLS with certificate validation never disabled (no `verify=False`, no `rejectUnauthorized:false`, not even temporarily in dev), credentials and tokens never travel over plain HTTP, a raw-HTTP integration is upgraded or refused, and redirect-to-HTTPS is enforced at the edge.
- **SEC-14** Before a surface goes public it has bot/DDoS protection at the edge and a challenge (CAPTCHA/Turnstile) on every abuse-prone unauthenticated endpoint: signup, login, contact, and anything that costs money per call.
- **SEC-15** Rate limits are set per endpoint category, never global — auth/login strictest (per-IP and per-account, with lockout/backoff), then write endpoints, then expensive or paid endpoints, then public reads — and abuse limits (SEC) and spend gates (BE-2/3) are both required.
- **SEC-26** Repeated failures from one source (failed auth, malformed or unauthorized requests, 404-scanning from one IP) escalate throttle → temporary block → auto-ban at a stated threshold, every ban logged and surfaced (SEC-21), and legitimate-user lockout uses account-scoped backoff, never an IP ban.

## §5 · Third-party & supply chain

- **SEC-19** Inbound callers prove identity — webhook signatures verified (HMAC + timestamp window + replay guard), tokens validated server-side — and a source IP, a secret URL or a payload that looks like the vendor's is never authentication.
- **SEC-18** Every third-party integration is inventoried with what it is, what data crosses, which credential it uses, where that credential lives (MGR-32) and its blast radius if compromised.
- **SEC-20** Dependencies are pinned, reviewed when added and updated on a cadence for security patches, and a new dependency passes the reuse-check (LEAD-6), never a reflex install.

## §7 · Detection & response

- **SEC-21** Failed-auth spikes, unfamiliar access (new IP/geo/device on an admin surface) and unusual spend or volume are each watched against a stated threshold on the record BE-19 provides, with the alert routed per OPS-23.

## Secret-scan pattern list (SEC-24)

Method: `rsot/knowledge/secret-scan-patterns.md`.
