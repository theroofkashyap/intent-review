---
name: capture-intent
description: Record the intended outcome, observable acceptance criteria, constraints, and non-goals before a non-trivial code change. Use when preparing agent-written work for human review or when asked to capture or clarify a change's intent. Skip purely mechanical edits unless requested; this does not perform a code review.
---

# Capture intent

Give the eventual reviewer a compact account of what the change is supposed to accomplish, grounded in the request rather than reverse-engineered from the implementation.

## Capture before implementation

Read the request and any supplied issue, specification, or existing intent record. Inspect enough relevant code to understand current behavior and material constraints. Do not turn intent capture into a full repository audit.

Separate three kinds of information:

- **Stated:** requirements explicitly supplied by the user or referenced specification. Cite the source, using a message description when no link exists.
- **Observed:** current behavior or constraints established by inspected code or executed checks. Cite the file and symbol or the command and result. Existing code establishes behavior, not necessarily desired behavior.
- **Proposed:** interpretations or assumptions introduced by the agent. Keep them visibly provisional; silence does not make them agreed requirements.

Write the record before changing behavior. If work has already started, say that this is a retrospective capture and identify its sources. Do not imply an earlier agreement or invent the author's reasons from the diff.

## Make intent reviewable

Describe the problem and desired outcome in behavioral terms. Include a concrete trigger and expected result. For a behavior-preserving refactor, describe the structural goal and the behavior that must stay the same. For an investigation, name the question and expected evidence without promising a particular fix.

Give acceptance criteria stable IDs such as I1 and I2 so a later reviewer can connect findings to them. Use only as many criteria as the change needs. Include failure behavior or compatibility constraints when material to the request, not as boilerplate.

Do not quietly introduce implementation choices as requirements. Record a mandated approach as stated; otherwise label a consequential approach as proposed and explain its reason briefly. Record non-goals only when stated or clearly label them as proposed boundaries.

When an ambiguity materially changes behavior, compatibility, or scope, ask a focused question before implementing the affected part. Continue independent work already authorized. Routine implementation choices do not need confirmation. If the user asked only for an intent record, return the record without starting implementation.

## Record format

Adapt the length to the change; aim for a brief a reviewer can read in about a minute. Omit empty optional sections rather than filling them with generic claims.

```markdown
## Intent

**Problem:** Who encounters what problem, and under what conditions?
**Outcome:** What should become possible or different?
**Basis:** Source references and whether this was captured before or after implementation began.

**Acceptance criteria:**
- I1 [stated | observed | proposed; source]: Trigger → expected result.

**Constraints / non-goals:** Source-labeled boundaries, if any.
**Open questions:** Unresolved decisions that affect the outcome, if any.
```

Choose the applicable source label for each entry; the bracketed alternatives are template guidance. An observed behavior becomes a preservation criterion only when the request or an explicit proposal establishes that it should be preserved.

## Keep one record through review

Use the project's established location if one exists. Otherwise emit the record in chat. When preparing a PR, carry the current record into the PR description or a local draft within the authorized workflow. Do not create a tracking file or publish a PR solely because this skill is active.

If the intent changes during implementation, preserve the original criterion ID and briefly record the old and new intent, the reason, and the source authorizing the change. An agent's suggested scope change stays proposed until resolved; do not rewrite the original goal to fit the code. Retire IDs rather than reusing them for unrelated criteria.

Update the existing record instead of scattering competing versions across chat and files. Keep material changes traceable within that record.

## Handoff

At completion, identify unresolved intent questions and point to the current record. Keep verification evidence separate: an acceptance criterion describes the desired behavior, not proof that it works.

If a verification receipt exists, attach or reference it without repeating its contents. This skill works independently of `receipts` and `review-against-intent`; neither is required to capture intent.
