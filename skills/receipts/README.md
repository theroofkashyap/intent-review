# receipts

**Your coding agent will tell you what it tested. This makes it tell you what it didn't.**

One markdown file. Once it is installed, every non-trivial change your agent makes ends with a receipt: what it actually ran, what it did not check, and what it assumed about code it never opened.

## What a receipt looks like

<!-- TODO before publishing: replace this block with a real receipt from a real change. Not a synthetic one. This is the thing people screenshot. -->

_Real receipt goes here._

## Install

Copy `SKILL.md` into a folder named `receipts` at the path for your harness. Each line shows the user-wide path first, then the per-repo path.

- **Claude Code:** `~/.claude/skills/receipts/SKILL.md` or `.claude/skills/receipts/SKILL.md`
- **Codex:** `~/.agents/skills/receipts/SKILL.md` or `.agents/skills/receipts/SKILL.md`
- **Cursor:** `~/.cursor/skills/receipts/SKILL.md` or `.cursor/skills/receipts/SKILL.md`
- **opencode:** `~/.config/opencode/skills/receipts/SKILL.md` or `.opencode/skills/receipts/SKILL.md`

Paths checked against each harness's documentation on 2026-09-12. Cursor and opencode also read `.agents/skills/`, so one copy there covers Codex, Cursor, and opencode.

## Scope

It skips trivial changes on purpose. A one-line fix, a typo, a rename, or a formatting pass gets no receipt. The skill treats that as a rule rather than a preference, because a receipt on a trivial change is noise, and noise teaches reviewers to skip the receipts that matter. Anything a person will review gets the full receipt.

## License

MIT
