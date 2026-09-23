---
name: capture-vision
description: Interview the builder and record the company vision, product visions, committed features, and builder intent as a single VISION.md that later changes can be scored against. Use when starting a project, when asked to write or update a vision, or when vision-alignment reports that no VISION.md exists. Elicits and records; does not author the vision or produce a roadmap.
---

# Capture vision

Give every later change a fixed point to be measured against. Features accumulate into products and products into a company; a change that is locally sensible can still pull the whole tree somewhere the builder never chose. This skill records where the builder is going so `vision-alignment` can say how far a change strays.

## Elicit, do not author

The vision belongs to the human. Ask, reflect back, and record. Do not fill gaps with plausible goals: a vision the agent wrote is not a standard the agent can be held to.

Work top-down, one level per exchange:

1. **Company.** What does this exist to do, and for whom? What will it never do?
2. **Products.** The distinct things the company ships or has committed to ship. For each: the outcome it delivers, what it deliberately leaves out, and the constraints it operates under.
3. **Features.** Under each product, the capabilities that make the product goal true. Only ones the builder has actually committed to.
4. **Builder intent.** Why the builder is doing this at all, and the tradeoffs that follow: maintainability over speed, learning over shipping, no outside money, one person must be able to run it. These are the tiebreakers when a change is on-vision but wrong in flavor.

Label each entry. **Stated** means the builder said it. **Proposed** means the agent inferred it from the codebase, a README, or a conversation and it awaits confirmation. Silence is not agreement; a proposed entry stays proposed until the builder confirms it.

If code already exists, inspect enough to draft a proposed tree, then interview to confirm or correct it. Say that the capture is retrospective. Do not turn this into a repository audit.

## Non-goals are the point

Goals attract agreement; non-goals do the work. A change can serve a goal and still be drift because it does something the builder decided not to do. Push for non-goals at every level, and for constraints concrete enough to violate: "no runtime dependencies," "single binary," "no user accounts." "Keep it simple" cannot be violated and should not be recorded.

## Keep it a vision

Aim for something a reviewer reads in about two minutes. Longer than a page, it has become a roadmap. Do not add priorities, dates, status, owners, or task lists; those change weekly and belong elsewhere. VISION.md changes when the builder changes their mind, not when work progresses.

## Record format

Write `VISION.md` at the repository root, or wherever the project already keeps its top-level documents. IDs are stable: products are P1, P2; features are F1.1, F1.2 under P1. When something is dropped, delete it and never reuse its ID.

```markdown
# Vision

Last reviewed: YYYY-MM-DD

## Company

**Goal:** What this exists to do, and for whom.
**Non-goals:** What it will never do.
**Constraints:** Rules every product operates under.

## Products

### P1: Name

**Goal:** The outcome this product delivers.
**Non-goals:** What this product deliberately leaves out.
**Constraints:** Rules specific to this product.

#### F1.1: Feature name [stated | proposed]

One or two sentences: the capability, and what it makes true for the user.

## Builder intent

Why this is being built at all, and the tradeoffs that follow from that.
```

Omit empty optional sections. Keep source labels on any entry the agent proposed; remove the label once the builder confirms it, so a settled vision carries no labels.

## Updating

When `vision-alignment` finds a change that serves a product but no feature names it, it proposes an addition. Apply it here, with the builder's confirmation, rather than letting the vision fall behind the product. Update `Last reviewed` whenever the builder confirms the tree still holds.

The agent proposes edits to VISION.md; the builder commits them. Do not modify VISION.md because a change would score better if the vision said something different.
