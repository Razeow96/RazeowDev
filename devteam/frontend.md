# Dev team — Senior Frontend Developer (FE)

Type: Law · loads when the task writes screen or browser code. Visual and design law lives in `uxui.md`; this file is the code side. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/frontend-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 1 · Stack & platform

- **FE-12** The frontend stack and its rendering and hosting model are chosen from what the product needs (static for content, SPA for an app-like surface, SSR/SSG when SEO or first paint matters, plain HTML+JS when a framework earns nothing) with the reason recorded in the ADR, and the frontend gets its own server or edge runtime when performance demands it (ARCH-22).
- **FE-13** Before building, verify what is currently true of the targets (browser, OS and device support for the planned APIs, framework or library breaking changes, recent regressions) and state the fallback wherever support is partial (same class as BE-23).

## 2 · The component library

- **FE-10** Screens are built from the shared component library, never hand-rolled (UX-2): each shared component carries a declared prop contract, a new component enters at the BE-29 threshold, and a hand-rolled variant of something the library has is a defect (BE-24).
- **FE-7** Screens never import each other or the app shell; cross-screen coordination goes through the shared kit (ARCH-20).

## 3 · Data & state

- **FE-19** Every piece of data declares its freshness strategy and its storage, never polling by reflex (strategy matrix: `rsot/knowledge/frontend-reference.md`); `localStorage` holds only non-sensitive, non-authoritative UI state, never tokens, credentials or anything the server must trust (SEC-29), auth rides an HttpOnly cookie or an in-memory token per the auth design, and anything cached client-side states how it goes stale.
- **FE-2** Each screen consumes the one payload BE-27 composes for it.
- **FE-1** The UI never re-implements the backend: no copied predicates, enums or column lists, it renders what the backend serves (DB-11), and an unknown enum value degrades to `unknown`, never breaks the screen.
- **FE-6** Writes go through the owning domain's door, never a direct PATCH to another domain's table, with one create surface per entity and any second [Create] button routing to it.

## 4 · Forms & write paths

- **FE-3** Every write button gets an in-flight guard, every fetch checks `res.ok` and response shape (a 2xx of the wrong shape is an error, an `errors` array on a 207 is a partial failure to surface), `finally` resets busy state, and screen currency is re-checked after any await.
- **FE-11** Validation is field-level and immediate, required fields included, states the constraint before it fails where possible, reports the specific fixable problem in plain language, never "Invalid input" or a raw code, and matches the server's actual rules (SEC-13).
- **FE-4** Every operator-managed entity ships Create, Read, Update and Delete, each behind a control the operator can see (UX-2), and a destructive removal with no cascade is refused, never silently orphaning.
- **FE-5** User-facing limit text is computed from the enforcing constant, and one constant drives both the control and its counter.

## 5 · Rendering & assets

- **FE-14** Responsive is verified before Done at phone, tablet and desktop breakpoints, live resize included, with nothing overflowing horizontally, and that observation is the evidence QA-9 collects (UX-3).
- **FE-15** Images ship sized for their display box, in a modern format with a fallback, compressed at a stated quality, never the source-resolution original; off-screen images lazy-load and above-the-fold ones do not, every image declares its dimensions, and where the transformation happens (build, upload or CDN) is stated (OPS-19 owns delivery).
- **FE-16** Motion format is chosen by what it is (mapping: `rsot/knowledge/frontend-reference.md`), never an animated GIF; heavy motion is lazy-loaded, cached on a fingerprinted immutable URL (OPS-19) and never blocks first paint, reduced-motion is honoured, off-screen motion pauses or drops, and a decorative animation that delays interaction is a defect.

## 6 · Runtime & resilience

- **FE-17** A hidden tab pauses polling, timers, animations, video and all non-essential work and closes or downgrades real-time connections, then on return resumes and refreshes once, reconciling rather than replaying a backlog; off-screen elements neither animate nor load, non-urgent work waits for idle, unload-time sends use `sendBeacon`, never a blocking request, and a second tab of the same app adds near-zero load.
- **FE-18** Every subscription, interval, observer and event listener a screen creates is torn down when the screen is destroyed, data caches are bounded by size or TTL, and infinite lists virtualize or page rather than grow the DOM.
- **FE-20** Network loss shows a clear offline state, keeps loaded content readable from cache, queues or blocks writes explicitly, and re-fetches authoritative state on reconnect rather than replaying a stale queue; a value-bearing flow (payment, checkout, anything that moves money or grants access) is never resumed from client state, the client asks the server for that operation's status by its id and renders the answer (SEC-25, BE-18).
