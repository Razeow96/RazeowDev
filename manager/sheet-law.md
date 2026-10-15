# Sheet law — Google Sheets and the Drive tree

Type: Law · how every company and project uses its Google Sheet and its Google Drive folders; pulled through rulesindex whenever a task reads or writes a Sheet, creates a truth folder, or links a human view. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/governance-law-rationale.md` §sheet-law, same rule IDs, open when a rule's judgment is in play.

- **SHEET-1** One Sheet per company and one per project, a module is a tab in its project Sheet, and a Sheet may exist at any other level where humans need to sync with the machine.
- **SHEET-2** Every company and project Sheet carries the tabs Index · State · Plan · Changelog: Index = one row per tab and linked file · State = what is true now · Plan = task detail · Changelog = one row per change, one line.
- **SHEET-3** Raze and AI edit the same Sheet, and Index, State and Plan rows are edited in place.
- **SHEET-4** The Markdown in the repo is the truth for documents and rules and the Sheet is the human surface and the home of plan and state rows: a Sheet cell that contradicts a Markdown truth doc is corrected to the doc, and a plan or state row is edited in the Sheet, never copied into Markdown.
- **SHEET-5** The Drive tree mirrors the repo tree level by level with identical folder names — company (repo) → project → module → `msot` or `sot` — a Drive folder holds the non-Markdown side of that truth (PDFs, decks, exports), and a Drive file that contradicts the Markdown is corrected, never the other way round.
- **SHEET-6** A new `msot/` or `sot/` is not done until its Drive folder exists and its row links it; a truth folder older than this rule gets its Drive folder at its next touch, never as a back-fill sprint; a renamed or retired code renames or retires its Drive folder in the same pass.
- **SHEET-7** The project's repolaneindex row carries its Sheet link (Sheet column), the module lane index row links its tab, the Sheet's Index tab links the Drive folder, and there is no separate `Human view:` line.
- **SHEET-8** /start reads the Sheet's State and Plan, and /close writes the session's changes (SHEET-2) after the owner confirms his own edits.
