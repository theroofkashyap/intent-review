# Vision

Last reviewed: 2026-09-23

<!-- Retrospective capture from README.md, docs/history/, and the 2026-09-23 design conversation. Entries marked [proposed] were inferred by the agent and await the builder's confirmation. -->

## Company

**Goal:** Make agent-written code changes faster and safer for a human to judge, by preserving what the agent knew and what the builder intended so the reviewer does not reconstruct either from the diff.
**Non-goals:** Programmatically verifying that an agent's self-report is truthful. Replacing code review, CI, or a product manager. Tying to any one model, provider, or harness.
**Constraints:** Each skill is a single Markdown file with no scripts, config, dependencies, or build step. No CLI and no service. No new tracking files unless the builder asks for one. Skills stay agent-neutral. A skill earns its lines; keep each under about 150.

## Products

### P1: Intent review

**Goal:** A reviewer can judge a change from the diff alone. One review reconstructs purpose, audits verification claims, and reports concrete misses. A recorded intent or a receipt makes that review faster. It is not required.
**Non-goals:** Enforcing a workflow; each skill is usable alone. Producing paperwork on trivial changes. A program that proves a receipt is true.
**Constraints:** Intent and evidence are source-labeled as stated, observed, or proposed. Nothing an agent inferred is presented as author-confirmed. A verification claim with no replayable command is a finding, not evidence. The review re-runs claimed checks when that is safe and local, and says when it cannot.

#### F1.1: Capture intent

Optionally record the problem, acceptance criteria, constraints, and non-goals of a change before implementation, with each entry's source labeled. The review proceeds without this record.

#### F1.2: Receipts

The coding agent ends every non-trivial change with replayable commands and observed results, what it did not check, and what it assumed about code it never opened. The receipt is written to be audited.

#### F1.3: Review against intent

Compare a diff to its purpose, reconstructed and labeled when no intent was recorded. Audit any receipt: unsupported or contradicted verification claims are findings. Report mismatches, unexplained scope, independent bugs, and the single check that would most change confidence.

### P2: Vision alignment [proposed]

**Goal:** A builder can see whether a change moves the project toward the vision they wrote, and how far it strays, before it lands.
**Non-goals:** Producing a roadmap, priorities, or status tracking. Making the score the product; the trace is the product and the score summarizes it.
**Constraints:** The vision is human-owned; agents propose edits and never apply them. Scoring is done from the diff and VISION.md, not from the session that produced the change. Trivial changes are not scored.

#### F2.1: Capture vision [proposed]

Interview the builder and record company, product, and feature goals plus builder intent as VISION.md.

#### F2.2: Alignment scoring [proposed]

Trace each changed behavior to the vision node it serves, check non-goals and constraints, score 0-10 on an anchored ladder, flag under 7 as drift or vision gap, and propose the VISION.md edit when the vision is behind.

## Builder intent

Built for the world, not for one project. The audience is solo builders and small teams who use coding agents heavily and have no product manager; a large org with a roadmap will see this as paperwork and should not be sold to. The proof is real examples from real changes, not the skill text: a skill is only shown to work when it caught something a human would have missed. Prefer one honest, specific admission over a well-formatted claim. [proposed, from docs/history and the 2026-09-23 conversation]
