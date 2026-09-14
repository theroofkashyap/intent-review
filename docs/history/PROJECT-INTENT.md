# Making intent visible in code review

## Original motivation

This project began with the observation that a code diff does not reliably reveal the author's intent. A reviewer can see what changed, but may not know what problem the author meant to solve, why they chose this approach, or which behavior they intended to preserve.

The project direction is to explore a few coding-agent skills that capture and communicate that intent so reviewers can assess a change against its purpose.

`receipts` is the first existing skill. It captures verification evidence, unchecked behavior, and assumptions. That supports review, but covers only part of the original problem.

## Initial skills

The first workflow now has three skill files. They are ready for local trials; their effect on real review effort has not yet been evaluated.

1. **[Capture intent before implementation](../../skills/capture-intent/SKILL.md).** Record the problem, observable acceptance criteria, constraints, and non-goals. Distinguish user-stated requirements from agent inferences and preserve material intent changes.
2. **[Review against intent](../../skills/review-against-intent/SKILL.md).** Compare the intended outcome with the actual diff and verification evidence. Surface missing behavior, unintended scope, independent correctness issues, and unresolved questions.
3. **[Receipts](../../skills/receipts/SKILL.md).** Report what the coding agent actually checked and what remains uncertain.

Separate decision-recording, drift-checking, and review-brief skills remain candidates. The first two new skills cover the essentials without requiring more installed skills or separate documents.

## Questions to resolve

- Is the primary pain understanding the desired outcome, understanding the implementation choices, or detecting a mismatch between the two?
- Where should intent live so it stays available through implementation and review without becoming stale?
- How much structure helps on an ordinary PR before it becomes paperwork?
- When reviewing an existing change with no recorded intent, how should the skill distinguish evidence from guesses?

## Relationship to the existing handover

`HANDOVER.md` describes the earlier plan to ship receipts as a standalone skill. Preserve that narrow scope for `receipts/`; this document records the broader project motivation and the ongoing exploration of additional skills.
