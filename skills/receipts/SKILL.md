---
name: receipts
description: Attach a verification receipt to every code change, recording replayable commands and observed results, what was not checked, and what was assumed about code that was never opened. A later review will try to re-run those commands. Use this whenever writing, editing, refactoring, debugging, or fixing code that another person will review or merge - which is almost always. Apply it to every non-trivial change without being asked, including bug fixes, new features, refactors, migrations, and dependency updates.
---

# Receipts

## Why this exists

The person reading your diff cannot see what you did. They see the result and have to reconstruct your reasoning backwards from it. That reconstruction is the slowest part of their day, and it is the reason agent-written changes sit in the queue longer than human ones.

You hold information they do not: what you ran, what came back, what you chose not to check, and what you took on faith about code you never opened. Writing that down costs you thirty seconds. Not writing it down costs them forty minutes.

The valuable part is not the summary. It is the admission. A reviewer who knows which five percent you did not verify can go straight there instead of reading all of it with equal suspicion.

## What to do

### Before writing code

Say the intent in one sentence before touching anything. If you cannot state what the change is supposed to do without describing how you will do it, you do not understand the task yet. Ask.

### While writing code

Track two lists as you go, in the moment. You will not remember them later.

- **Commands you actually executed**, and what they actually returned. Not what you expect them to return.
- **Things you relied on without checking.** Every time you call a function you did not open, assume a field exists on a response, or trust that a caller handles an error you are now returning, that is an assumption.

### After writing code

Emit a receipt in the format below.

## Receipt format

ALWAYS use this template, in this order:

```
## Receipt

**Intent:** one sentence, what this change is for

**Touched:** file - what changed there (one line each)

**Verified:**
- `exact command` - what it actually returned

**Not verified:**
- specific thing, and why it was not checked

**Assumed:**
- something relied on without opening it

**Start here:** file:line - the part most likely to be wrong
```

## Rules that make the receipt worth reading

**Verified means executed, and replayable.** A command belongs under Verified only if you ran it in this session and saw the output. Each line is one exact command plus what you observed: a count, an exit code, or a short quoted line. A later review will try to run that command. If you cannot name it, the item is Not verified. "Tests should pass" is a hope. A claim with no command, or a result that disagrees with a re-run, is a finding against the change.

**Do not pad Verified.** A typecheck passing does not verify behavior. A linter passing does not verify correctness. Say what each check actually proves, not that a green tick appeared.

**Never leave Not verified empty to look good.** An empty Not verified is a strong claim: that every behavior this change can affect has an executed check behind it. That is rarely true. On the rare occasion it is, say why in one line. Each item names the behavior and the command that would settle it.

**Be specific about what is untested.** "Did not test edge cases" is useless. "Did not test the empty-array path in `parseRows`, which is now reachable because the early return moved" is something a reviewer can act on in ten seconds.

**Assumptions are about code you did not read.** If you wired into an existing function based on its name and signature rather than its body, that is an assumption and it belongs in the receipt. This is where almost-right changes usually go wrong, and it is invisible in the diff.

**Start here points at exactly one place.** Pick the single riskiest hunk, not a list. If everything is equally risky, the change is too big and should be split.

## Scale it to the change

- One-line fix, typo, rename, formatting, comment change: **no receipt.** This is a rule, not a preference. A receipt on a trivial change is noise, and noise teaches reviewers to skip the receipts that matter.
- Anything a human will review: full receipt.
- More than about five files: full receipt, and group the Touched list by concern so the reviewer can skip whole sections at once.

## Where the receipt goes

- Working session with a person in the loop: in the chat, as the last thing you say about the change.
- Opening a pull request: in the PR description. That is where the reviewer is.
- Never in the commit message. It goes stale the moment anyone amends or rebases, and a stale receipt is worse than none.

## Examples

**Bad, because it is padded and quietly dishonest:**

```
**Verified:**
- Ran the full test suite, all passing
- Code is type-safe and follows project conventions

**Not verified:**
- Minor edge cases
```

Nothing here helps. "All passing" does not say whether a single test covers the new path. "Minor edge cases" hides the actual risk behind a phrase that sounds responsible.

**Good:**

```
**Verified:**
- `pytest tests/test_parser.py` - 34 passed. Three of those cover the new strict=True branch.
- `python -c "from app.parser import parse; print(parse(''))"` - returns [], no exception.

**Not verified:**
- The strict=False path. No existing test covers it and I did not add one, so the old behavior is unconfirmed.
- Anything downstream of parse(). I did not run the integration suite, it needs a live DB.

**Assumed:**
- Row.validate() raises on bad input rather than returning False. I matched the existing call sites but never opened it.

**Start here:** app/parser.py:88 - the early return moved above the null check, which changes behavior for empty input.
```

## If you verified nothing

Say so plainly at the top of the receipt rather than working around it. "I wrote this and ran nothing" is a completely acceptable receipt, and far more useful to the reviewer than an invented one.
