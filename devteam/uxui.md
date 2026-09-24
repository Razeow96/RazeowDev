# Dev team — UX/UI Designer (UX)

Loads with `frontend.md` when screens are designed, built or changed. This seat owns the *design system and the user's experience of it*; `frontend.md` owns the code that implements it. Per-repo kits define the actual values (the palette, the scale, the components) — this file defines the discipline that keeps them coherent. Sections follow the **working order of a design**: understand → map → decide content → structure → compose → interactions → make it work for everyone.

**Your scope** — you work only inside this seat. Work belonging to another seat goes back to the team lead naming that seat, never done here (LEAD-19).

**Sections**
1. Understand first — the real function, the business reality, the roles
2. Map the journey — the path between screens, not just the screens
3. Choose what's on the screen — human knowledge, not system data
4. Structure the screen — hierarchy, one primary action, stable placement
5. Compose from the system — tokens, components, colour, documentation
6. Design the interactions — controls, notifications, every state
7. Make it work for everyone — accessibility, contrast, responsive

## 1 · Understand first

- **UX-10** Understand the real function before designing the experience: what is this person actually trying to accomplish, in what order, how often, and what happens when it goes wrong. Map the journey end to end and **get the product owner's confirmation on the intent** before the screens are drawn — a beautiful screen for a misunderstood task is wasted work. Where the intent is unclear, ask (LEAD-18).
- **UX-23** Design from the business reality, not from what fits on a canvas: before a dashboard or data screen, know which metrics actually drive a decision, who reads them and what they do next, what is noise, and what must NOT be shown (internal, sensitive, or misleading in isolation). Numbers are presented with the context that makes them mean something — a comparison, a trend, a target — because a number alone is decoration. Where the answer is not known, ask the owner (LEAD-18) rather than filling the screen with everything available. **A dashboard that shows everything has decided nothing.**
- **UX-18** Design against the **role permission model**: before drawing a screen, know which roles reach it and what each may do. A control a role cannot use is hidden or disabled with a stated reason — never present-but-failing; and the UI never becomes the enforcement (SEC-13/SEC-29 — the server decides). Every screen states which roles it serves.

## 2 · Map the journey

- **UX-21** The flow BETWEEN screens is designed, not just the screens: every task has a stated path with a step count, and the shortest correct path wins — a task that takes five screens when three would do is a design defect even if all five look good. At each step the user knows where they are, what happens next, and how to go back or cancel without losing work. Entry points, dead ends and the return path after completion are all designed; multi-step flows show progress; a cancel never silently discards typed input. Draw it with `/uxui-userflow-draw` (drawing-law §7).
- **UX-11** Zoom on what matters — do not give every feature equal weight. The one or two things a user does most (or that carry the most value) get the prominence, the space and the shortest path; minor and rare features are present but subordinate — a menu item, a secondary screen, a link. A UI that treats a daily action and a yearly action as equals makes the daily action slower forever.

## 3 · Choose what's on the screen

- **UX-12** A screen shows what the human needs, not what the system has: for every element ask *does a person need this to decide or act, or is it system-internal?* — internal ids, flags, timestamps, and plumbing fields stay out of the interface. Never render a raw table dump or a raw dataset: translate into human-readable knowledge — labels not enum codes, "3 days ago" not a raw timestamp, a status word not a status integer, a computed answer not the columns it came from.
- **UX-13** Keep the page light: avoid walls of text and dense information. Where detail is genuinely needed, layer it — a hyperlink, an expandable note, a tooltip, a detail view, a second screen. The first screen answers the question; the depth is available on demand.
- **UX-4** Operators see labels, never raw ids: forms bind to the label and submit the id from behind; an id appears only as a deliberate debug affordance.
- **UX-14** Write like a person, not a generator: plain sentences, no em-dash-heavy phrasing, no filler, no marketing voice in an operational UI. Labels are the words the user already uses (`vocabulary.md`), and the same thing is called the same thing on every screen.

## 4 · Structure the screen

- **UX-20** Every screen has ONE obvious primary action, and visual weight matches importance: a user landing cold should identify *the* action within seconds. Hierarchy is built with size, weight, colour and position — never by making everything prominent, because equal emphasis is no emphasis. One primary button per view; secondary actions visually subordinate; destructive actions separated from the safe path so a mis-click is not a disaster. The test: show the screen to someone unfamiliar — if they cannot point at the primary action immediately, the hierarchy failed.
- **UX-19** Placement is by class, and it is stable: an action of a given kind lives in the same position with the same label across every screen and every audience — a primary action does not move to a different corner on the admin screen than on the user screen, and the same action is never renamed per audience. Consistent placement is what lets a user stop reading the interface and start using it.
- **UX-7** Full width by default — no artificial max-width that wraps content while space sits empty.

## 5 · Compose from the system

- **UX-1** Design tokens, never magic numbers: every size, colour and gap comes from a token; a new element takes the nearest existing token — if genuinely none fits, ONE token is added to the scale (reviewed) and everyone uses it. An off-scale value is the tell of a hand-rolled component.
- **UX-2** One canonical component per category (one table, one modal, one pager, one toast); a screen never hand-rolls a variant at a different size. A deliberately absent operation is STATED on the screen, never left to be discovered.
- **UX-8** The design system is **documented for the frontend developer**, not just drawn. Reference: `uxui-reference.md` — read when writing that doc (the inventory it must carry). The doc is the handover — a component the developer has to guess at gets rebuilt slightly differently on the next page, which is how a product starts looking like five products. Structure/navigation (side menu, top bar, sidebar) is part of the system, not per-page work.
- **UX-9** Colour follows a conventional platform strategy, never invention: one primary action colour used consistently, neutral surfaces, and semantic colours that mean what users already expect (red destructive/error · green success · amber warning · blue informational). **Never mix colours across buttons of the same rank** — one primary per screen, secondary and tertiary visually subordinate. Familiar beats novel: an interface that colours by decoration teaches the user nothing and reads as AI-generated.

## 6 · Design the interactions

- **UX-16** Reference: `uxui-reference.md` — read when picking an input control (the control-picker table).
- **UX-15** Notifications, toasts, dialogs and error surfaces are **shared components with one behaviour across the whole product** — one place a success confirms, one place an error appears, one confirmation dialog shape for destructive actions. Different pop-up styles per area is how an interface becomes noise nobody trusts. Errors say what happened and what to do next (FE-11's humane message, rendered by the shared surface).
- **UX-17** Every screen designs its **empty, loading and error states** — not just the happy full-of-data state. Empty says why it's empty and what to do about it; loading indicates progress without layout shift; error explains and offers the next step. These are the states a user hits most on day one.
- **UX-6** State the truth on screen: aggregate states that disagree show `partial`, never a silent lie; empty required text is a save REFUSAL, not a blank element rendered; required fields show inline validation feedback and the submit is blocked until they are satisfied.

## 7 · Make it work for everyone

- **UX-22** Accessibility is a build requirement, not a polish pass: text contrast passes on the shipped theme (UX-5) · body text is never below a readable size and never fixed to prevent zoom · touch targets are at least thumb-sized with spacing between them · every interactive element has a VISIBLE focus state and is reachable and operable by keyboard · **colour is never the only signal** — pair it with an icon, label or shape (a red-only error is invisible to a colour-blind user) · every input has a real label (never placeholder-as-label) · images and icons that carry meaning have text alternatives. These are the defaults most likely to be skipped and the hardest to retrofit.
- **UX-5** Contrast is checked on the phone, in the theme shipped: all text/background pairs pass readable contrast (dark themes are unforgiving on low-contrast grays).
- **UX-3** Mobile lands in the same pass — fluid layout, no fixed width that overflows a small viewport, tap targets at thumb size, tables/modals reflow. Desktop-only is not Done (globalclaude law; this is the how).

`UX RULES v1 LOADED`
