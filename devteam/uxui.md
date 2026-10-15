# Dev team — UX/UI Designer (UX)

Type: Law · loads with `frontend.md` when screens are designed, built or changed. This seat owns the design system and the user's experience of it; `frontend.md` owns the code that implements it, and per-repo kits define the actual values. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/uxui-rule-rationale.md`, same rule IDs, open when a rule's judgment is in play.

## 1 · Understand first

- **UX-10** Before screens are drawn, the real function (what the person is trying to accomplish, in what order, how often, what happens when it goes wrong) is mapped end to end and the product owner confirms the intent (method: `rsot/knowledge/uxui-dashboard-design.md`).
- **UX-18** Before drawing a screen, know which roles reach it and what each may do; every screen states the roles it serves, and a control a role cannot use is hidden or disabled with a stated reason, never present-but-failing (SEC-13).

## 2 · Map the journey

- **UX-21** The flow between screens is designed, not only the screens: the shortest correct path, entry points, dead ends, the return path after completion and progress on multi-step flows, with the user always knowing where they are, what comes next and how to go back or cancel, and a cancel never silently discards typed input (DRAW-1; method: `rsot/knowledge/uxui-design-method.md`).

## 3 · Choose what's on the screen

- **UX-12** A screen shows only what a person needs to decide or act, never internal ids, flags, timestamps, plumbing fields or a raw table or dataset dump, translated into human-readable knowledge (labels not enum codes, relative time not raw timestamps, status words not integers, a computed answer not its source columns).
- **UX-14** Copy is plain sentences with no em-dash-heavy phrasing, filler or marketing voice in an operational UI, labels use the words the user already uses (`rsot/knowledge/vocabulary.md`), and the same thing has the same name on every screen.

## 4 · Structure the screen

- **UX-20** Every screen has one primary action a user landing cold identifies within seconds: one primary button per view, secondary actions visually subordinate, destructive actions separated from the safe path, and hierarchy built from size, weight, colour and position, never by making everything prominent.
- **UX-19** An action of a given class keeps the same position and the same label on every screen and for every audience, never moved or renamed per audience.
- **UX-7** Layout is full width by default with no artificial max-width, except reading surfaces, which take one reading column.
- **UX-24** A heading, display line or lead never ends on a one-word last line (a red build): headings and display lines carry `text-wrap: balance` and leads and standfirsts `text-wrap: pretty` kit-wide in the base stylesheet (never per block), no heading or lead is capped narrower than its container (UX-7), Figma headings and leads are drawn at container width and broken by measurement, and before handover the page is read at 1440 and 390 with a stranded-word count of zero.

## 5 · Compose from the system

- **UX-1** Every size, colour and gap comes from a token, never a magic number; a new element takes the nearest existing token, and only if none fits is one reviewed token added to the scale for everyone.
- **UX-2** One canonical component per category (one table, one modal, one pager, one toast), never a hand-rolled variant at a different size, and a deliberately absent operation is stated on the screen.
- **UX-8** The design system, structure and navigation included (side menu, top bar, sidebar), is documented for the frontend developer in a handover doc carrying the inventory in `rsot/knowledge/uxui-reference.md`, read when writing that doc.
- **UX-9** Colour follows a conventional platform strategy, never invention: one primary action colour used consistently, neutral surfaces, semantic colours with their expected meaning (red destructive/error · green success · amber warning · blue informational), and never mixed colours across buttons of the same rank (UX-20).

## 6 · Design the interactions

- **UX-16** The input control is chosen by the shape of the choice, not by habit (control-picker table: `rsot/knowledge/uxui-reference.md`), and every selection control shows its current state plainly and has a way to clear it.
- **UX-15** Notifications, toasts, dialogs and error surfaces are shared (UX-2): one place a success confirms, one place an error appears, one confirmation dialog shape for destructive actions, and every error says what happened and what to do next (FE-11's humane message, rendered by the shared surface).
- **UX-17** Every screen designs its empty, loading and error states: empty says why and what to do, loading shows progress without layout shift, and error explains and offers the next step.
- **UX-6** Aggregate states that disagree show `partial`, never a silent lie; empty required text is a save refusal, never a rendered blank; and submit stays blocked until required fields are satisfied (FE-11).

## 7 · Make it work for everyone

- **UX-22** Accessibility is a build requirement: text contrast passes on the shipped theme (UX-5), body text is never below a readable size or fixed against zoom, touch targets (UX-3) have spacing between them, every interactive element has a visible focus state and is keyboard reachable and operable, colour is never the only signal, every input has a real label (never placeholder-as-label), and meaningful images and icons have text alternatives.
- **UX-5** All text/background pairs pass readable contrast, checked on the phone in the theme shipped.
- **UX-3** Mobile lands in the same pass: fluid layout, no fixed width that overflows a small viewport, tap targets at thumb size, tables and modals that reflow, and desktop-only is not Done (global card law; this is the how).
