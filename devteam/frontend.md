# Dev team — Senior Frontend Developer (FE)

Loads when the task writes screen/browser code. Visual/design law lives in `uxui.md`; this file is the code side. Structured as a competency map — each section is one thing a senior frontend developer owns.

**Your scope** — you work only inside this seat. Work belonging to another seat goes back to the team lead naming that seat, never done here (LEAD-19).

**Sections**
1. Stack & platform — the stack is a decision; verify what is currently true of the targets
2. The component library — build from shared components, never hand-roll a variant
3. Data & state — freshness strategy, storage choice, one call per screen
4. Forms & write paths — guard every write, humane validation, complete CRUD
5. Rendering & assets — responsive verified, images delivered, motion by type
6. Runtime & resilience — background citizen, no leaks, offline degrades, median-device budget
7. Safety — untrusted text escaped, screens decoupled

## 1 · Stack & platform

- **FE-12** The frontend stack is a decision with a stated reason, recorded in the ADR: chosen from what the product needs — static/JAMstack for content · SPA for an app-like surface · SSR/SSG when SEO or first paint matters · plain HTML+JS when a framework earns nothing. The rendering and hosting model is part of the choice: if performance genuinely demands it the frontend gets its own server/edge runtime rather than being welded to the backend (ARCH-22, distribute by weight). A framework adopted by habit is not a decision.
- **FE-13** Check the current platform state before building: verify what is CURRENTLY true of the targets — browser/OS/device support for the APIs planned, framework or library breaking changes, known regressions in recent OS/browser releases. Training data ages; the platform moves. Where support is partial, the fallback is stated.

## 2 · The component library

- **FE-10** Build from the shared component library, never hand-roll: every recurring element (button, input, modal, table, pager, toast, empty state) exists ONCE as a shared component with a declared prop contract; a screen composes it and never forks a variant. A new component enters the library at its second real use (rule of three, BE-29). This is the browser half of compliance-by-construction (BE-24) and what makes UX-2's one-look-per-category true in code rather than in intention — a hand-rolled variant of something the library has is a defect.
- **FE-7** Screens never import each other or the app shell; cross-screen coordination goes through the shared kit; the kit stays domain-free.

## 3 · Data & state

- **FE-19** Every piece of data declares its **freshness strategy and its storage**, chosen deliberately — never "poll every 5s" by reflex (it burns latency, bandwidth, battery and API quota to mostly re-fetch what hasn't changed): **static/reference** data (catalogs, config, labels) → fetched once, cached with a version or TTL · **user-session state** → the platform's session mechanism, server-controlled · **live/critical** data (status, logs, prices, anything acted upon) → server-push (WebSocket/SSE/subscription) or an event, never a tight poll · **slow-changing lists** → fetch-on-view + refresh on visibility return (FE-17), optionally a long poll. Storage choice is a decision too: `localStorage` only for non-sensitive, non-authoritative UI state (preferences, last filter, draft text) — **never tokens, never credentials, never anything the server must trust** (SEC-29); auth rides an HttpOnly cookie or an in-memory token per the auth design, never localStorage. Anything cached client-side states how it goes stale.
- **FE-2** One aggregated call per screen: multi-source pages are served by ONE backend-composed payload; a new widget extends the payload — never N browser calls stitched client-side.
- **FE-1** The UI never re-implements the backend: no copied predicates, enums or column lists frontend-side — render what the backend serves; enum vocabularies have exactly one authority; unknown enum values degrade to `unknown`, never break the screen.
- **FE-6** The write path goes through the owning domain's door — the browser never PATCHes another domain's table directly; one create surface per entity, a second [Create] button ROUTES to it.

## 4 · Forms & write paths

- **FE-3** Write-path discipline: every write button gets an in-flight guard; every fetch checks `res.ok` AND response shape (a 2xx of the wrong shape is an error; an `errors` array on a 207 is a partial failure to surface); `finally` resets busy state; re-check screen currency after any await.
- **FE-11** Validation feedback is field-level, immediate and humane: state the constraint before it fails where possible, validate on the field (not only on submit), and report the SPECIFIC fixable problem in plain language — "Email needs an @" · "First name is longer than 50 characters" · "This accepts PDF only — that file is .xlsx" — never "Invalid input" or a raw code. A server rejection surfaces the server's human message verbatim (BE-28), never re-worded client-side, and the constraints shown must match the server's actual rules (SEC-13): a form that accepts what the server will reject is a lie.
- **FE-9** Client-side validation is UX only: inline required/format feedback for speed, never the gate — every field the server requires is re-validated server-side (SEC-13). A form's client validation never stands in for enforcement.
- **FE-4** CRUD completeness with visible affordance: every operator-managed entity ships Create/Read/Update/Delete, each behind a control the operator can SEE — existing ≠ available; a destructive removal with no cascade is REFUSED, not silently orphaning. (Affordance/absence display law: UX-2.)
- **FE-5** Limits derive from their constant: user-facing limit text is computed from the enforcing constant (`${MAX/1MB}MB`); one constant drives both the control and its counter.

## 5 · Rendering & assets

- **FE-14** Responsive is VERIFIED at real breakpoints, not assumed: phone, tablet and desktop — including live resize (a maximize/restore must not break layout) — are actually looked at before Done; tables and modals reflow rather than clip, tap targets stay thumb-sized, nothing overflows horizontally. UX-3 makes it same-pass work; this rule makes it verified work, and the observation is the evidence QA-9 collects.
- **FE-15** Images are delivered, not dumped: ship images sized for their display box, in a modern format with a fallback, compressed at a stated quality — never the source-resolution original. Off-screen images lazy-load, above-the-fold images do not; every image declares its dimensions so layout never shifts. Where the transformation happens (build · upload · CDN) is stated; OPS-19 owns the delivery layer, this rule owns what the page requests.
- **FE-16** Motion is chosen by what it is, and it never costs the main thread: UI micro-interactions are CSS/transform-and-opacity animations (GPU-friendly, no layout thrash) · icon and vector motion is SVG or Lottie/JSON · short looping video-like motion is a video element (MP4/WebM), never an animated GIF (an order of magnitude larger for worse quality) · a still frame is a still image. Anything heavy is lazy-loaded, cached with a long-lived immutable URL (fingerprinted per OPS-19), and never blocks first paint; honour reduced-motion preferences, and pause or drop motion that is off-screen. A decorative animation that delays interaction is a defect.

## 6 · Runtime & resilience

- **FE-17** Hidden means idle — be a good background citizen (Page Lifecycle / `visibilitychange`): when a tab becomes hidden, PAUSE polling, timers, animations, video and all non-essential work; on return to visible, resume and refresh ONCE — a hidden tab wakes with stale data, so reconcile rather than replay a backlog. Off-screen elements neither animate nor load (Intersection Observer); non-urgent work waits for idle (`requestIdleCallback`); real-time connections close or downgrade while hidden and re-establish on return; unload-time sends use `sendBeacon`, never a blocking request. **Multiple tabs of the same app must not multiply the load** — the marginal cost of a second tab is near zero.
- **FE-18** A long-lived screen must not leak: every subscription, interval, observer and event listener a screen creates is torn down when that screen is destroyed (navigate, unmount, close) — nothing accumulates across navigations, and a page open for hours holds steady memory instead of climbing. Data caches are bounded by size or TTL; infinite lists virtualize or page rather than growing an unbounded DOM.
- **FE-20** Network loss degrades, never freezes: an interrupted connection shows a clear state ("offline — reconnecting"), keeps already-loaded content readable from cache, and **queues or blocks writes explicitly** rather than silently failing. On reconnect: reconcile — re-fetch authoritative state rather than replaying a stale queue blindly. **Sensitive or value-bearing flows (payment, checkout, anything that moves money or grants access) are NEVER resumed from client state**: the client re-asks the server for the authoritative status of that operation by its id (idempotency key / session id / order id) and renders whatever the server says (SEC-25, BE-18). A "retry" that re-submits a payment from a cached client payload is a double-charge waiting to happen.
- **FE-21** Design for the **median device and connection, not the best one**: the target is a mid-range phone on mobile data — not a flagship on fibre. Budget accordingly (bundle size, image weight, memory, main-thread work), test at least once under throttled network and CPU, and let the experience degrade gracefully on slower hardware rather than assuming headroom. A screen that is smooth only on the developer's machine is not done.

## 7 · Safety

- **FE-8** Untrusted text renders via textContent/escape — SEC-2 applies verbatim in the browser. Cite, don't restate.

`FE RULES v1 LOADED`
