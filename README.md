# Intent Review

Spend less time reconstructing an agent's work, and more time judging whether it's right.

These Markdown skills help preserve the purpose of a code change and connect it to the implementation and verification evidence. They are initial drafts for testing on real reviews; review-time savings and behavioral reliability have not yet been established.

## Skills

| Skill | When to use it | Output |
| --- | --- | --- |
| [capture-intent](skills/capture-intent/SKILL.md) | Before a non-trivial change | Source-labeled intent with observable acceptance criteria, constraints, and open questions |
| [review-against-intent](skills/review-against-intent/SKILL.md) | When reviewing a diff or PR | Evidence-backed findings, intent coverage, and verification gaps |
| [receipts](skills/receipts/SKILL.md) | As the coding agent completes a non-trivial change | Checks actually run, unchecked behavior, assumptions, and where to inspect first |

Each skill is usable independently. The product is one review of a diff. `review-against-intent` reconstructs intent when none was recorded, labeling every inference, and it audits a receipt when one exists. A verified line with no replayable command, or a result that disagrees with a re-run, is a finding. Capturing intent beforehand and attaching a receipt are optional. They give the review better inputs. They are not steps the human has to perform.

## Use with your coding agent

The skills are agent-neutral Markdown instructions. Use them with a coding agent that can read repository files and inspect changes. Native skill discovery and invocation syntax vary by agent; no particular model or provider is required. Compatibility has not yet been tested across agents.

Download or clone this repository somewhere outside your target codebase:

```bash
git clone https://github.com/theroofkashyap/intent-review.git
```

For a first trial, copy the desired folders from this repository's `skills/` directory into a location in your target codebase and explicitly tell your agent to read the relevant `SKILL.md`. Each current skill needs only that file; no dependencies or build step are required.

```text
Read [path/to/capture-intent/SKILL.md] and follow it for this task: [request].
```

If your agent supports installed skills, use its documented skill directory and invocation mechanism. For example, Codex supports this project layout:

```text
your-codebase/
└── .agents/skills/
    ├── capture-intent/SKILL.md
    ├── review-against-intent/SKILL.md
    └── receipts/SKILL.md
```

For that optional Codex setup, see the [official skill documentation](https://learn.chatgpt.com/docs/build-skills). The workflow below applies to any compatible agent; provide the skill file paths when native discovery is unavailable.

The review, which is the only required step:

```text
Use review-against-intent to review [explicit diff or PR] against this request:
[original request]. Reconstruct intent where none was recorded, keep inferences
labeled, audit any receipt by re-running claimed commands, and report unsupported
verification claims as findings.
```

Optional, before implementation, when you want the intent fixed in advance:

```text
Use capture-intent to record the intent for this change: [request].
Then implement it and use receipts to report the verification evidence.
```

The intent record stays in chat or the project's established location, and can be carried into a PR description. These skills do not require a new tracking file or an external service. Receipt lines are exact commands and observed results, so the review can replay them.

## Current scope

This is an early experiment. All three skill files pass the skill format validator; real-world evaluation is still pending. A valid skill file is not evidence of faster or more accurate reviews.

Try it on a real change and note whether it reduces time spent understanding the diff, surfaces useful findings, or adds unnecessary reading. Report unsupported intent claims and misleading verification statements as failures, even when the output looks convincing.

Historical brainstorming and the earlier receipts-only plan live in [docs/history](docs/history/). They describe the project's development, not additional installation steps.

## License

[MIT](LICENSE).
