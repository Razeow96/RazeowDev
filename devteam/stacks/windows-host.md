# Dev team — Stack pack: Windows host (WIN)

Loaded when a repo's operator card declares `Stacks: windows-host`. **Every repo on this machine declares it** — the Windows / PowerShell 5.1 environment is always relevant when coding here. Platform hazards only; the general coding law is in the seats. This pack is the destination for globalclaude §12's dev-execution rules.

**Sections**
1. Shells & commands — three shells, and what to hand Raze
2. File & text encoding — the UTF-8 traps
3. Secrets on Windows — how to set one without mangling it

## 1 · Shells & commands

- **WIN-1** Three shells, not interchangeable: the owner's terminal = **Windows PowerShell 5.1**; Claude's Bash tool = **Git Bash (POSIX)**; a `!`-prefixed line runs in Claude's session shell. Know which one a command targets before writing it.
- **WIN-2** Commands handed to Raze are PS 5.1-safe: no `&&`/`||` (use `;` or `if ($?) { }`), no bash-isms, full paths for non-PATH executables — or give a `!`-prefixed line for the Claude prompt. Never a raw bash line for his terminal.
- **WIN-3** `rg`/ripgrep skips gitignored files by default — any sweep that must be complete runs a second `rg --no-ignore` pass over gitignored configs (`.env`, `docs/private/`).

## 2 · File & text encoding

- **WIN-4** Never bulk-replace file text with PowerShell string ops (`Get-Content` + `-replace` + `WriteAllText`): PS 5.1 reads BOM-less UTF-8 as ANSI and corrupts every non-ASCII character. Edit files one at a time with the Edit tool; if PowerShell is unavoidable, pass explicit UTF-8 encoding on **both** read and write.
- **WIN-5** A spawned subprocess emits ASCII-safe (or explicitly UTF-8) JSON on stdout — Windows stdout uses the console codepage, so `ensure_ascii=False` ships bytes the parent re-reads as U+FFFD. Applies to every worker↔subprocess text boundary.
- **WIN-6** Non-ASCII HTTP payloads (e.g. zh-TW) go through Bash + a UTF-8 file + `curl --data-binary @file`; Python scripts set `PYTHONIOENCODING=utf-8`.

## 3 · Secrets on Windows

- **WIN-7** Setting a secret via CLI: **paste at the interactive prompt** — never pipe (PowerShell injects a BOM that corrupts it, surfacing as a misleading wrong-key error), never pass as an argument (it lands in shell history — rotate any key that ever was one). This is SEC-5's command form.
- **WIN-8** SQL carrying a `$`-bearing literal (a hash) goes through `--file=x.sql` written as a single-quoted here-string (`@'...'@`) — never `--command "...$..."` (PowerShell eats `$`, mangling the value into a total auth outage that reads like "wrong password"). Verify the stored value after.

`WIN RULES v1 LOADED`
