---
name: vision-alignment
description: Score a diff, branch, or pull request against VISION.md by tracing each changed behavior to the vision node it serves and checking it against stated non-goals and constraints. Reports 0-10, flags scores under 7 as drift or as a vision gap, and proposes the VISION.md edit when the vision is behind. Use on branches and PRs; skips trivial changes. Does not edit code or apply vision edits.
---

# Vision alignment

Say where a change sits in the builder's tree, and how far it strays. The trace is the product; the score summarizes it.

## Preconditions

Find `VISION.md`. If none exists, say so and stop, or offer `capture-vision`. Do not reconstruct a vision from the codebase and score against it: a vision inferred from the code will always agree with the code.

Skip trivial changes: formatting, typos, comment edits, dependency bumps with no behavioral change, one-line fixes inside an existing feature. Say "trivial, not scored" and stop. Scoring these produces noise and trains people to ignore the output.

## Score from the diff, not from the session

If you are the agent that wrote the change, say so in the output, and score from the diff and VISION.md alone. Reasoning from the session that produced the change is not evidence of what the change does; the diff has to stand on its own. Where practical, run this in a fresh context that has not seen that session.

## Establish the boundary

Identify the requested comparison: working changes, a branch against its base, a commit, or a PR. Use the supplied base and head when given, and report the scope actually examined. Do not omit staged, untracked, or relevant changed files from a working-change review. Read the diff and enough surrounding code to understand what each change does; read VISION.md in full.

## Trace, then check for conflict

Work by changed behavior, not by file. One behavior may span several files; one file may contain several behaviors.

**Trace.** For each changed behavior, name the deepest node in VISION.md it serves: a feature (F1.2), a product (P1), or the company. Cite the diff location. Tests, documentation, and refactors that directly support a traced behavior inherit its trace. Maintenance that serves every product equally, such as build, CI, tooling, and security patches, traces to the company and scores as a feature trace. Anything that cannot be connected to a node is **unexplained**; list it separately and do not invent a connection.

**Conflict.** Walk the path from the traced node up to the company, and check every non-goal and constraint on it, plus builder intent. Cite the vision text and the diff location for each conflict. Distinguish:

- **Grazes:** approaches a constraint without clearly violating it.
- **Violates:** clearly breaks a constraint.
- **Contradicts:** does something a non-goal says will not be done, or works against the company goal or builder intent.

When the node is uncertain, say so. Do not pick the one that scores best.

## Score

The score is the first rung that applies, reading from the top. Every number has one meaning.

| Score | Meaning |
| --- | --- |
| 0 | Works against the company goal or builder intent |
| 1 | Contradicts a stated non-goal |
| 2 | Violates a constraint, and no node explains the change |
| 3 | Violates a constraint |
| 4 | No node explains the change |
| 5 | Traces only to the company goal: a product no one committed to |
| 6 | Traces to a product, no feature names it, and grazes a constraint or carries material unexplained scope |
| 7 | Traces to a product; no feature names it |
| 8 | Traces to a feature; grazes a constraint or carries material unexplained scope |
| 9 | Traces to a feature; minor supporting extras only |
| 10 | Traces to a feature; nothing unexplained, nothing grazed |

When behaviors in one change land on different rungs, the change takes the lowest. Say which behavior set it.

Material unexplained scope is a second behavior riding along that a reviewer would want explained, not a helper or a test fixture.

## Flag and remedy

Scores under 7 are flagged. Every flag names a remedy class:

- **Drift.** The change should be reconsidered: it contradicts, violates, or cannot be explained. Say what to remove or change.
- **Vision gap.** The change is coherent with the product goal and builder intent, and the only problem is that no node names it. The vision is behind the product. Propose the exact VISION.md addition.

When both readings are plausible, present both and let the builder decide.

A 7 passes but still gets a **vision note**: no feature names this behavior, and here is the proposed feature entry. This keeps the vision current without alarms.

Propose VISION.md edits as a diff block. Do not apply them, and do not modify VISION.md to improve a score.

## Output

```markdown
## Vision alignment

**Score:** N/10 — PASS | VISION NOTE | FLAG (drift | vision gap)
**Rung:** Which rung, and which behavior set it.
**Scope:** Comparison examined and material limits.
**Author:** Same agent as the change | different agent | unknown.

### Trace
| Changed behavior | Serves | Evidence |
| --- | --- | --- |
| Short description | F1.2 / P1 / Company / unexplained | file:line or hunk |

### Conflicts
- Grazes | Violates | Contradicts — quoted vision text. Evidence: file:line.

### Remedy
Drift: what to change. / Vision gap: proposed edit:
```diff
+ #### F1.3: Feature name
+ One sentence.
```

**Start here:** One location most useful for the builder to inspect, with a reason; omit if none stands out.
```

Omit Conflicts and Remedy when empty. A small change may need only the score, rung, and a two-row trace.

## Stay in the lane

Do not edit the implementation, apply VISION.md edits, approve or merge a PR, or post external comments merely because this skill is active. This skill judges direction, not correctness: use `review-against-intent` for whether the change does what its request asked, and `receipts` for what was actually verified. When their artifacts exist, use them as context, not as authority.
