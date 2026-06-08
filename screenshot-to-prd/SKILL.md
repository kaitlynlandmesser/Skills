---
name: screenshot-to-prd
description: Turn one or more UI screenshots — mockups, competitor-app screens, hand sketches, Stitch/Figma exports, or shots of an existing screen — into a feature PRD by reading the image, interviewing the user one question at a time, then synthesizing a spec. Use this at the START of building any new feature, any time the user shares a screenshot, mockup, or design and wants a spec/PRD, or asks you to "ask me questions and write a spec" — even if they never say the word "PRD". Builds a grill-with-docs–style interview on top of a screenshot analysis, and ends with a to-prd–style synthesis.
---

# Screenshot → PRD

Turn a picture of a UI into a written feature spec. A screenshot shows the happy path and almost nothing else: it hides the empty state, the loading state, the error state, where the data comes from, and what every tap does. Your job is to read what the image *does* show, drag everything it *doesn't* show into the open through a relentless interview, and then write it all down as a PRD.

This skill composes three moves: **analyze the screenshot**, **interview like `grill-with-docs`** (one question at a time, grounded in the codebase and its docs), and **synthesize like `to-prd`** (no re-interviewing at the end — just write).

## When to use

Use at the very start of a new feature whenever the user hands over an image of a UI and wants it turned into a spec. The image may be aspirational (a mockup, a competitor's app, a sketch, a generated design) or real (a screen that already exists and is being formalized or ported). Do not wait for the user to say "PRD" — "here's a screenshot, ask me questions and write the spec" is the trigger.

## Process

### 1. Read the screenshot(s) out loud first

Before any questions, produce a short structured read-back so the user can correct your perception immediately. Separate what you **observe** from what you **infer**, because confusing the two is how specs go wrong.

- **Observed**: screens present, visible components, copy, layout, apparent hierarchy.
- **Inferred**: the feature's purpose, the data behind each element, the likely interactions.
- **Ambiguities**: anything you can't tell from the image. Each ambiguity is a future interview question — collect them now.

Keep this tight. The point is a fast "yes, that's right / no, you've misread X" from the user, not an essay.

### 2. Ground in the codebase and its docs

This is the `grill-with-docs` inheritance. Before interviewing, orient yourself:

- Explore the repo to understand current state. If a question can be answered by reading the code, read the code instead of asking.
- If `CONTEXT.md` exists, use its glossary vocabulary throughout. If `docs/adr/` exists, respect prior decisions in the area you're touching.
- If `DESIGN-SYSTEM.md` exists, load it — it's the **visual glossary**, the exact counterpart of `CONTEXT.md`. Hold its tokens (colours, type scale, spacing, radii, components) in mind so you can check the screenshot against them during the interview. If it doesn't exist and this looks like the start of a new app or visual direction, say so and offer to run `screenshot-to-design-system` first — the two skills pair, and a PRD grounded in a known design system beats one inventing visuals as it goes.
- Cross-reference the screenshot against reality: does this component already exist? Does the data it implies already have a source? Does anything in the image contradict current behavior? Surface conflicts the moment you spot them.
- These files may not exist yet — this skill often runs early. Don't require them. Create `CONTEXT.md` lazily, only when the first term gets resolved (see step 3).

### 3. Interview relentlessly, one question at a time

This is the heart of the skill, and it inherits `grill-me` / `grill-with-docs` discipline:

- Ask **one question at a time** and wait for the answer before the next.
- For every question, **provide your recommended answer** — make it easy to agree or push back, not a blank prompt.
- Walk the decision tree branch by branch, resolving dependencies between decisions in order.
- When the user uses a fuzzy or overloaded term, sharpen it against the glossary ("you said 'gallery' — is that the saved-pages list or the template picker? those are different"). When a term resolves, update `CONTEXT.md` inline; don't batch it.
- Challenge the screenshot against the design system the same way you challenge terms against the glossary. When a visual value drifts from `DESIGN-SYSTEM.md`, flag it and ask — one at a time, with a recommendation: "this button is a blue that isn't in your system — a new token, or should it be `primary`?" Treat a brand-new colour, font, or radius as drift to resolve now, not a detail to transcribe into the spec.

Seed your questions from two places: the **ambiguities** you collected in step 1, and the **UI states checklist** below — because the screenshot won't show most of them.

#### UI states checklist (the value-add)

Walk this explicitly; it's what makes the spec UI-driven rather than a generic feature doc:

- **States of every screen**: default/populated, empty (no data yet), loading, error/retry, offline, permission-denied, success/confirmation.
- **Components**: which are new vs reused; which should become shared.
- **Interactions & gestures**: every tap, swipe, long-press, pull-to-refresh; what each does.
- **Navigation**: how the user gets in, how they get out, what the back behavior is.
- **Data**: what backs each element, where it comes from, what happens before it loads.
- **Edge cases**: long text, very many or zero items, missing fields, localization, Dynamic Type / large fonts, dark mode, slow network.

#### ADRs, sparingly

Offer to record a decision as an ADR only when all three hold: it's **hard to reverse**, it's **surprising without context**, and it's the **result of a real trade-off**. Otherwise skip it.

### 4. Synthesize the PRD — do not re-interview

Once the decision tree is resolved, stop asking and write. Use the template below. Use the project's glossary terms. Do **not** include file paths or code snippets (they go stale); the exception is a small schema/state-machine/type shape that captures a decision more precisely than prose can.

Save the PRD as markdown (default: `docs/prds/<feature-slug>.md`). Confirm the destination with the user the first time. Only publish to an issue tracker if the user asks — the saved spec is the primary artifact, and it's also the seed the spec-export and porting skills will consume later.

## PRD template

ALWAYS use this structure:

```markdown
# <Feature name>

## Problem statement
The problem, from the user's perspective.

## Solution
The solution, from the user's perspective.

## Screens & UI behavior
For each screen in the design:
- **Purpose** — what this screen is for.
- **Components** — the elements on it (new vs reused), named by their design-system token/role where one applies.
- **States** — default, empty, loading, error, offline, permission-denied, success.
- **Interactions** — what every control does.
- **Navigation** — entry points, exits, back behavior.
- **Data** — what backs each element and where it comes from.
- **Visual** — reference `DESIGN-SYSTEM.md` tokens by role; list any new tokens this feature deliberately introduces (resolved during the interview, not invented here).

## User stories
A long, numbered list. Format: "As a <actor>, I want <capability>, so that <benefit>."
Cover every aspect of the feature, not just the happy path.

## Implementation decisions
Modules to build/modify, interfaces, architectural choices, schema changes, API contracts,
and technical clarifications from the interview. No file paths or code (except a small
decision-encoding snippet where prose is genuinely worse).

## Testing decisions
What makes a good test here (test external behavior, not implementation), which behaviors
to cover, and prior art for similar tests in this codebase.

## Out of scope
What this PRD deliberately does not cover.

## Open questions
Anything left unresolved, with the current best guess.
```

## Notes

- If the user provides several screenshots that form a flow, treat them as one feature and capture the transitions between them in **Navigation**.
- If the screenshot is a competitor's app or a generated mockup, say so in the PRD — "modeled on X" — so later readers know the source isn't ground truth.
- Degrade gracefully: with no `CONTEXT.md`, no `docs/adr/`, and no issue tracker, this skill still works end to end and just produces a markdown spec.
- The design system is optional too: with no `DESIGN-SYSTEM.md`, skip the conformance checks and proceed; with one, reference its tokens by role in the spec and record any new tokens the feature introduces.
