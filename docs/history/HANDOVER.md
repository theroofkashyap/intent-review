# Handover: `receipts`

> Historical plan for the standalone receipts skill. The current repository contains three skills under `skills/`; see the [root README](../../README.md) for the current layout and scope. Tasks and layout constraints below are preserved as historical notes.

## What this is

An agent skill that makes any coding agent end every non-trivial change with a receipt: what it actually ran, what it did not check, and what it assumed about code it never opened.

Single markdown file. No code, no dependencies, no CLI.

## Why it exists

Agent-written diffs sit unreviewed because the reviewer has to reconstruct intent backwards from the result. The agent holds information the reviewer does not and currently throws it away at the end of the session. The receipt is that information, written down.

The load-bearing section is `Not verified`. Everything else is context for it. If a change to this project makes that section less honest or less specific, the change is wrong.

## Current state

`SKILL.md` is written and is the whole product. It is good. Do not rewrite it. Two additions are needed (task 2 below) and nothing else.

## Tasks, in order

### 1. Scaffold

```
receipts/
├── SKILL.md
├── README.md
├── examples/
└── LICENSE          MIT
```

Four entries. Nothing else.

### 2. Two additions to SKILL.md

**a. Where the receipt goes.** Currently unspecified, which is the one real gap. Add a short section:

- Working session with a human in the loop: emit in chat
- Opening a PR: put it in the PR description
- Never in the commit message, it goes stale the moment anyone amends or rebases

**b. A line reinforcing scope.** The skill already says skip trivial changes. Make sure that reads as a firm rule rather than a suggestion, because the most likely failure mode is receipt noise on one-line fixes.

Keep SKILL.md under about 150 lines after these edits.

### 3. README

Structure, in this order:

1. Headline: "Your coding agent will tell you what it tested. This makes it tell you what it didn't."
2. A real receipt, above the fold, from an actual change. Not a synthetic one. This is the thing people screenshot and it is what sells the repo.
3. Install, per harness
4. One paragraph on scope: it skips trivial changes on purpose

### 4. Install instructions

Claude Code, Codex, Cursor, and opencode each load skills from different paths, and these change. Check each harness's current documentation and write the actual path. Do not guess, and do not copy paths from memory. Four lines total.

If a harness cannot load a skill in under thirty seconds, say so plainly rather than writing a workaround.

### 5. Dogfood before publishing

Run the skill on a real project for a week. Every receipt produced is either a README example or evidence the format is wrong.

Watch two things specifically:

- Does `Assumed` fill with anything real, or does it just restate what is already visible in the diff? If the latter, that section needs tightening or cutting.
- Do `Verified` entries ever cite commands that were never actually run? If yes, that is the central failure mode and it needs a note in the skill, not a script.

Collect the three best receipts into `examples/`.

### 6. Publish

1. Ship the repo quietly
2. Post the real receipts, not the skill. The artifact is the pitch
3. Then submit to skills directories and Trendshift

## Do not build

These are deliberate omissions, not oversights:

- No `scripts/` directory. The moment this has code it becomes a tool and gets compared to other tools. A single markdown file gets compared to nothing.
- No config file, no flags, no customization layer
- No CLI wrapper
- No CONTRIBUTING.md, no code of conduct, no issue templates, no logo
- No CI beyond whatever is already free
- No attempt to programmatically verify the receipt is truthful. That is a separate project and it is not this one.

## Definition of done for v0

- Repo has exactly four entries
- SKILL.md under 150 lines, with the receipt destination specified
- README opens with a real receipt from a real change
- Install path verified against current docs for at least two harnesses
- Three genuine examples in `examples/`

## Known risk

Agents are agreeable. There is a real chance this produces well-formatted confessions that are partly invented, which would be worse than no receipt at all. The fix is not a script inside this repo. If it shows up during dogfooding, note it and stop, and it becomes the case for a separate CI-side tool that reads session logs rather than asking the model to self-report.
