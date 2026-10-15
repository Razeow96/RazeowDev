# Dev team — Stack pack: Windows host (WIN)

Type: Law · stack pack (LEAD-4) that every repo on this machine declares, because the Windows / PowerShell environment is always relevant when coding here; platform hazards only, the general coding law is in the seats, and this pack is the destination for globalclaude §12's dev-execution rules. Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/stacks-rule-rationale.md` (Windows host section), same rule IDs, open when a rule's judgment is in play.

## 1 · Shells & commands

- **WIN-1** Method: `rsot/knowledge/windows-host-shells.md`.
- **WIN-2** Commands handed to Raze are native pwsh (`&&`, `||`, ternary and null-coalescing allowed) with no bash-isms or unix-only commands (use cmdlets or their aliases) and full paths for non-PATH executables, or a `!`-prefixed line for the Claude prompt (global card §4).
- **WIN-3** Any sweep that must be complete runs a second `rg --no-ignore` pass over gitignored configs (`.env`, `docs/private/`).

## 2 · File & text encoding

- **WIN-4** Never bulk-replace file text with PowerShell string ops (`Get-Content` + `-replace` + `WriteAllText`): edit files one at a time with the Edit tool, and where PowerShell is unavoidable pass explicit UTF-8 encoding on **both** read and write.
- **WIN-5** A spawned subprocess emits ASCII-safe (or explicitly UTF-8) JSON on stdout, at every worker↔subprocess text boundary.
- **WIN-6** Non-ASCII HTTP payloads go through Bash + a UTF-8 file + `curl --data-binary @file`, and Python scripts set `PYTHONIOENCODING=utf-8`.
- **WIN-9** Code written through the file-writing tool writes control and special characters as `\xNN` (`/[\x00-\x1F\x7F]/g`, `/[\t\xA0]/g`) or `String.fromCharCode`, never `\uXXXX`, and before a build's first commit the new files are byte-scanned: no byte below 0x20 except tab/LF/CR, no 0x7F, no raw 0xC2 0xA0, no BOM.

## 3 · Secrets on Windows

- **WIN-7** A secret set via CLI is **pasted at the interactive prompt**, never piped (SEC-4), as SEC-5's command form.
- **WIN-8** SQL carrying a `$`-bearing literal (a hash) goes through `--file=x.sql` written as a single-quoted here-string (`@'...'@`), never `--command "...$..."` (SEC-5).
