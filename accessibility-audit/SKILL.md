---
name: accessibility-audit
description: Audit an app's screens for accessibility issues against Apple HIG and Google/Material guidelines — screen-reader labels, Dynamic Type / text scaling, contrast, touch targets, motion, structure — by reading the code file by file. Produces ACCESSIBILITY-AUDIT.md (summary table + per-issue records located by file:line) and an interactive HTML dashboard. Identifies issues only; leaves fixes to a later pass. Use when the user wants an accessibility or a11y audit, or to check VoiceOver/TalkBack support, Dynamic Type, or contrast across an iOS (SwiftUI/UIKit) and/or Android (Compose/Views) codebase. This is a static code audit — it flags what the code can confirm and lists separately what needs manual or on-device checking.
---

<what-to-do>

You are an accessibility auditor. Read the UI code file by file and flag every issue against Apple's HIG Accessibility and Google's Material/Android accessibility guidance. You **identify** problems — you do not fix them; a later pass owns that. Be honest about the limits of a static read: report what the code confirms, and collect what only a device can verify into its own section rather than guessing.

Work in this order:

1. **Go file by file — don't sample.** Walk every UI file so nothing is skipped. Detect the platform from the file itself: Swift / SwiftUI / UIKit → Apple; Kotlin / Jetpack Compose / XML → Google/Material. **Load the matching mechanism reference for that file** — `references/ios-checks.md` or `references/android-checks.md` — so you check against the exact platform APIs without carrying the other platform's detail in context. Tag every issue with the platform it was found on.

2. **Run the full checklist** against each file. The categories and what each one checks are below (platform-agnostic); the exact mechanism for each comes from the platform reference you loaded in step 1. The platform guideline is the bar — cite HIG for Apple, Material/Android a11y for Google.

3. **Record each issue** with: a stable ID, severity, platform, `file:line`, the element, the category, the guideline it violates, a plain-language problem statement, and a confidence marker. **Do not propose a fix** — name the guideline (what good looks like) and stop there. The fixer decides the remedy.

4. **Collect the unverifiable.** Anything a static read genuinely can't confirm — rendered contrast over images/gradients/dynamic colors, real screen-reader reading order, whether text clips at the largest accessibility sizes — goes in a **Needs manual verification** section. Never label these `confirmed-from-code`, and never drop them.

5. **Write both outputs.** `ACCESSIBILITY-AUDIT.md` (counts summary, the issue table, then the per-issue records, then the manual-verification section) is the source of truth. Then populate the bundled `references/audit-dashboard.html` with the issue data and write it as `accessibility-audit.html` — an interactive, filterable view of the same issues. Regenerate the dashboard whenever the markdown changes; the markdown is the source, the dashboard is a view.

</what-to-do>

<supporting-info>

## Severity
- **blocker** — makes a function unusable with assistive tech (an unlabeled primary control, an element AT can't reach or operate).
- **serious** — a real barrier with a workaround (insufficient contrast, a gesture-only action with no accessible alternative, an important control missing its value/state).
- **minor** — degraded but usable (missing header trait, suboptimal grouping, a non-essential decorative image left exposed).

## Confidence
- **confirmed-from-code** — the code definitively shows the issue (no label present at all; `dp` used for text size).
- **needs-manual-check** — the code flags a risk a device must confirm (declared contrast looks borderline; layout may clip at the largest sizes; reading order is inferred).

## Checklist — what to check (platform-agnostic)

The categories below are *what* to look for. The *mechanism* for each — the specific API and the platform's threshold — lives in `references/ios-checks.md` and `references/android-checks.md`; load the one matching the file you're auditing.

**A. Screen-reader semantics — VoiceOver / TalkBack**
- An accessible name on every interactive and informative element
- Decorative elements hidden from assistive tech
- Labels are meaningful — not a filename, not "image"/"button", no "tap to"
- Correct role/trait (button, header, link, adjustable)
- Hints for non-obvious actions
- Value and state exposed for controls (toggles, sliders, selection, progress)
- Related elements grouped so they read as one unit
- Logical reading/focus order *(heuristic — inferred from layout)*
- Live-region announcements for dynamic content
- Custom actions for gesture-only interactions
- Modal focus containment for sheets/dialogs

**B. Dynamic Type / text scaling**
- Text uses scalable sizing, never hardcoded
- No fixed-height containers that clip scaled text *(clipping at the largest sizes is needs-manual-check)*
- Scaling not suppressed for essential text
- Respects Bold Text and the largest accessibility sizes

**C. Contrast & color** *(declared values only)*
- Text contrast ≥ 4.5:1 (body) / 3:1 (large); UI/icon ≥ 3:1, from declared colour vs declared background
- Information never conveyed by colour alone — pair with icon/text/shape
- Checked in light and dark mode
- *Rendered contrast over images/gradients/system colours → needs-manual-check*

**D. Touch targets & spacing**
- Targets meet the platform minimum size
- Adequate spacing between adjacent targets

**E. Motion, timing, gestures**
- Reduce Motion checked before animating
- No flashing/strobe content *(heuristic)*
- Non-gesture alternative for complex/custom gestures
- No aggressive, non-adjustable timeouts

**F. Forms & input**
- Fields labelled/associated, with the correct keyboard and content type
- Errors programmatically associated and announced
- Required/optional state indicated non-visually

**G. Structure & navigation**
- Headings marked for assistive-tech navigation
- Screen/tab titles set
- Logical container grouping / traversal order

**H. System-setting respect (umbrella)**
- Honors font scaling, Bold Text, Reduce Motion, Reduce Transparency, Differentiate Without Color, and Increased Contrast

## Principles

### Identify, don't fix
Name the problem and the guideline it breaks; never write the remedy. A separate fixer pass owns repairs, and mixing the two means an auditor confidently proposing wrong fixes. The record's job is to make the issue findable and the rule clear.

### Be honest about a static read
Code confirms some things and only hints at others. Mark every issue's confidence truthfully, and route anything a device must verify to the manual section. An audit that overclaims `confirmed` on contrast it can't actually see is worse than one that admits the limit.

### Located by line, recoverable by element
Issues are located by `file:line` because that's the chosen locator — but line numbers drift, so always also name the element, and tell the fixer to re-read the file before acting.

## Output: ACCESSIBILITY-AUDIT.md

````markdown
# Accessibility audit — <app>

> Static code audit against Apple HIG + Google/Material guidelines. Identifies issues; fixes are a separate pass.
> Located by file:line — re-read the file before fixing, since line numbers drift.

## Summary
Blockers: n · Serious: n · Minor: n   |   iOS: n · Android: n   |   Needs manual check: n

## Issues
| ID | Sev | Platform | File | Element | Category | Issue | Confidence |
| --- | --- | --- | --- | --- | --- | --- | --- |
| A11Y-001 | serious | iOS | EditorView.swift:42 | save button | Screen-reader semantics | Icon-only button has no accessible label | confirmed-from-code |

### A11Y-001 · serious · iOS
- File: EditorView.swift:42
- Element: save button (icon-only)
- Category: Screen-reader semantics
- Guideline: Apple HIG — interactive controls need an accessible label
- Problem: The icon-only save button has no `accessibilityLabel`; VoiceOver announces only "button".
- Confidence: confirmed-from-code
- Status: open

## Needs manual verification
Things a static read can't confirm, with why — e.g., "EditorView gradient header: text contrast depends on the rendered gradient; check on device."

## Source
Files scanned: n. Platforms: iOS, Android.
````

## Output: dashboard

Fill the bundled `references/audit-dashboard.html` — replace its `ISSUES` data array with the real issues (the same fields as the table), and write the result as `accessibility-audit.html`. The template handles rendering, counts, and filtering by severity / platform / category / confidence; you supply only the data. You may adapt its look, but keep the filtering working.

</supporting-info>
