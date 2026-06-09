---
name: screenshot-to-design-system
description: Infer the simplest design system that explains one or more UI images — colors, type scale, spacing, radii, elevation, and recurring components — then confirm every low-confidence read (especially fonts) through a one-question-at-a-time interview before writing anything down. Use at the START of a new app or visual direction, whenever the user shares mockups, a competitor's app, Dribbble/Figma exports, or hand-drawn screens and wants a design system, design tokens, "the colors and fonts," or a visual foundation — even if they never say "design system." This runs ONCE to establish the system, not once per feature.
---

<what-to-do>

Look at every image I give you before you propose anything. A design system is a glossary of the visual decisions that *recur* — not a catalogue of every pixel on the screen. Your job is to find the **smallest** set of tokens that reproduces these screens, confirm what you can't read with confidence, and throw out the accidents.

Work in this order:

1. **Inventory the raw facts across all images at once.** Never infer a system from a single screen. Pull out colors (with hex), text styles (size, weight, role), spacing gaps, corner radii, shadows/elevation, and repeated component shapes. As you go, **count how often each value appears** — frequency is your main signal for what is a real token versus an accident.

2. **Propose the simplest system that explains the screens.** Collapse near-duplicates into one token. Prefer a tiny type scale (≈4–6 roles, not twelve) and a single base spacing unit expressed as multiples. It is always easier to add a token later than to remove one in use — so start small and let me expand it, never the reverse.

3. **Cut the one-offs.** Any value that appears only once across the whole set is a candidate for removal, not a token. Surface it and recommend dropping or folding it into an existing token. *Example: ten screenshots and a typeface that shows up on exactly one of them — propose dropping it ("this serif appears once, on the onboarding screen — drop it, or is it a deliberate accent?"). Default to dropping.*

4. **Never guess what you cannot see, and never park what you could ask.** You can read a hex value reliably. You usually cannot read a typeface, an exact point size, the true base spacing unit, or whether two near-identical greys are one token at different opacities or two distinct ones. When your confidence is low, **ask — one question at a time, wait for my answer, and give your recommended answer with each.** Fonts especially: describe what you observe ("geometric sans-serif, ~17pt, medium weight") and ask me to confirm the family; do not assert "this is Inter." Anything you'd otherwise note as low-confidence or list under open questions is, by definition, something to ask about — ask it now, don't write it down as a guess. Keep asking until nothing askable is left.

5. **Write `DESIGN-SYSTEM.md` lazily.** Only write it once values are confirmed — don't commit a guess to disk. Keep it minimal. By the time you write, open / low-confidence should be empty or nearly so — you've already asked. Reserve it strictly for what I genuinely can't decide yet, never for a value you could have confirmed by asking.

6. **Generate the visual proof sheet.** Always finish by writing `design-system-proof-sheet.html` next to the markdown, so I can *see* the system and check it against the screenshots — a system I can't see is one I can't trust. Use `references/proof-sheet-template.html` as a **starting guide** — it's a known-good layout that encodes my preferences (large colour tiles, components grouped by type, neutral chrome) — but you have free reign to adapt its structure and styling to fit the system you actually found. Set the tokens to the confirmed values, render every token, and omit sections the system doesn't have. Just preserve the two properties that make it a verification tool (below). This is a view, not a source: regenerate it whenever the markdown changes, and never ask me to hand-edit it.

</what-to-do>

<supporting-info>

## Principles

### The simplest system possible
The best system is the one that explains every screen with the fewest tokens. Resist the urge to be thorough. A token earns its place by recurring; a value that appears once has not earned anything yet. When in doubt, leave it out and note it.

### Frequency decides tokenhood
A colour used on six screens is a token. A colour used once is probably an accident — a leftover from a different mockup, a stock-photo tint, a mistake. Treat repetition as the evidence that something is intentional, and treat singletons with suspicion.

### Collapse near-duplicates
Two values a hair apart are usually one value with drift, not two decisions. Surface them together and let me decide: "I see `#6B7280` and `#6C7280` — one grey, or deliberately two?" Recommend merging unless I tell you the difference is intentional.

### Name by role, not by value
Name tokens for what they *do* — `primary`, `surface`, `danger`, `body`, `caption` — not for what they look like — `blue1`, `bigText`. Roles survive a colour change; values don't.

### Reconcile with code when it exists
This skill is for the start of an app, when the pixels are the only source of truth. If the repo already has design tokens (`Assets.xcassets`, a SwiftUI `Color`/`Font` extension, a Compose theme), read those instead and let them win — the code is the real system, the screenshot is just a view of it. Flag any place the image contradicts the code.

### Ask before you park
Anything you'd note as low-confidence or list under open questions is something to ask about first — fonts, merged colours, dropped one-offs, the base unit are all askable. Resolve them in the interview, one at a time. Open / low-confidence is only for what I genuinely can't decide yet, and it's usually empty. Don't hand me a system with questions in it that I could have answered.

## Output format

Write `DESIGN-SYSTEM.md` (default at the repo root; confirm with me on first run). Keep it this lean:

```markdown
# Design system — <app or direction name>

> Inferred from screenshots. A hypothesis until reconciled against code.

## Colors
Semantic tokens only — `role — #hex — where it appears`
- primary — #______ — ...
- surface — #______ — ...

## Typography
A small scale by role — `role — family — size / weight`
- title — <family> — ...
- body — <family> — ...

## Spacing
Base unit: __pt. Scale: multiples (__, 2×, 3× …).

## Radii
- <name> — __pt

## Elevation
- <name> — <shadow / description>

## Components
Recurring patterns only — `component — what it's composed of`
- <component> — ...

## Open / low-confidence
Only what I genuinely can't decide yet. Everything askable — fonts, duplicate-vs-distinct colours, the base unit — should already be resolved in the interview, not parked here. Often empty.

## Source
Images: <list>. Reconcile against code tokens once they exist.
```

## Proof sheet

The proof sheet is how a human verifies a system that was guessed from pixels. `references/proof-sheet-template.html` is a **guide, not a cage** — a known-good starting layout (large colour tiles, typed component groups, neutral chrome) that you're free to adapt to the system in front of you. Whatever you change, keep the two properties that make it a verification tool rather than decoration:

- **Render from the tokens, not around them.** Draw every sample from the token values, so a wrong value produces a visibly wrong page. Keep the chrome neutral so the tokens are the only thing that stands out.
- **Let the checking sections carry the load.** Compose tokens into real UI grouped by type (buttons, cards, list rows), because mismatches against the screenshot surface there, not in isolated swatches. Fill the open / low-confidence section with whatever the interview genuinely couldn't resolve — usually little or nothing, since you asked.

Adapt the layout as the system needs — more colours, a denser grid, an extra component type — but don't let restyling bury those two properties.

</supporting-info>
