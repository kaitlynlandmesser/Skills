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

4. **Never guess what you cannot see.** You can read a hex value reliably. You usually cannot read a typeface, an exact point size, the true base spacing unit, or whether two near-identical greys are one token at different opacities or two distinct ones. When your confidence is low, **ask — one question at a time, wait for my answer, and give your recommended answer with each.** Fonts especially: describe what you observe ("geometric sans-serif, ~17pt, medium weight") and ask me to confirm the family. Do not assert "this is Inter."

5. **Write `DESIGN-SYSTEM.md` lazily.** Only write it once values are confirmed — don't commit a guess to disk. Keep it minimal. Mark anything still unconfirmed in the open-questions section rather than inventing a value to fill the gap.

6. **Generate the visual proof sheet.** Always finish by writing `design-system-proof-sheet.html` next to the markdown, so I can *see* the system and check it against the screenshots — a system I can't see is one I can't trust. Render every token visually from the values you just confirmed (see the proof-sheet spec below). This is a view, not a source: regenerate it whenever the markdown changes, and never ask me to hand-edit it.

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
Things still to confirm — fonts, duplicate-vs-distinct colours, the true base unit.

## Source
Images: <list>. Reconcile against code tokens once they exist.
```

## Proof sheet

The proof sheet is how a human verifies a system that was guessed from pixels. Build it as a single self-contained `design-system-proof-sheet.html` — inline CSS, no external dependencies, opens straight from disk.

Two rules give it its value:

- **Render from the tokens, not around them.** Define the confirmed tokens as CSS variables and draw every sample from them, so a wrong value produces a visibly wrong page. Keep the page chrome deliberately neutral — the tokens are the only thing that should stand out, never the proof sheet's own styling.
- **Make the checking sections do real work.** Include a *components* section that composes the tokens into a few real UI pieces (a button, a card, a list row), because mismatches against the screenshot show up there, not in isolated swatches. Include an *open / low-confidence* section that lists exactly the calls you made — the font you guessed, the colours you merged, the one-offs you dropped — so I have a punch-list to confirm or overturn.

Sections to render: colours (as swatches), typography (as specimens with the role/size/weight noted), spacing (as bars at each step), radii, elevation, components, open/low-confidence, and a slot to place the source screenshots beside it for direct comparison.

</supporting-info>
