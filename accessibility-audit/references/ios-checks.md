# iOS mechanisms — SwiftUI / UIKit

The exact APIs and thresholds for each checklist category on Apple platforms. Bar: Apple HIG Accessibility.

## A. Screen-reader semantics (VoiceOver)
- Accessible name — `.accessibilityLabel("…")` (SwiftUI) / `accessibilityLabel` (UIKit)
- Hide decorative — `.accessibilityHidden(true)`; hide siblings behind a modal with `.accessibilityElement(children: .ignore)` or `accessibilityElementsHidden`
- Role/trait — `.accessibilityAddTraits(.isButton)`, `.isHeader`, `.isLink`; `.accessibilityRemoveTraits(...)` to correct wrong ones
- Hint — `.accessibilityHint("…")` (only when the action isn't obvious from the label)
- Value/state — `.accessibilityValue("…")`; for toggles/sliders the value should reflect state
- Grouping — `.accessibilityElement(children: .combine)` to read a cluster as one; `.ignore` to suppress children
- Reading/focus order — `.accessibilitySortPriority(_:)` (higher reads first)
- Live region — `UIAccessibility.post(notification: .announcement, argument:)`; `.accessibilityAddTraits(.updatesFrequently)` for live values
- Custom actions — `.accessibilityAction(named:)` so gesture-only interactions are reachable
- Modal containment — `.accessibilityViewIsModal(true)` (or `accessibilityViewIsModal` on the container)

## B. Dynamic Type
- Scalable text — use Dynamic Type text styles (`.font(.body)`, `.headline`) or `@ScaledMetric` for custom sizes; avoid fixed `.font(.system(size: 17))` with no scaling
- Suppressing scaling — flag `.minimumScaleFactor`, fixed `.lineLimit`, or `.fixedSize` around essential text
- Bold text — respect `@Environment(\.legibilityWeight)`
- Range — support up to the accessibility sizes; `.dynamicTypeSize(...DynamicTypeSize.accessibility5)` shouldn't be capped low

## C. Contrast & colour
- Use semantic `Color` assets; compare declared foreground vs declared background
- Higher-contrast variants — `@Environment(\.colorSchemeContrast)`
- Not colour-alone — `@Environment(\.accessibilityDifferentiateWithoutColor)` should change the UI (add shape/icon)
- Check both `.light` and `.dark` color schemes

## D. Touch targets
- Minimum **44×44 pt** (HIG). Check declared `.frame(...)`; expand small hit areas with `.contentShape(Rectangle())` + padding

## E. Motion, timing, gestures
- Reduce Motion — gate animations on `@Environment(\.accessibilityReduceMotion)` or `UIAccessibility.isReduceMotionEnabled`
- Custom/complex gestures need an `.accessibilityAction` alternative

## F. Forms & input
- `textContentType`, `keyboardType` set appropriately; labels associated with fields
- Announce validation errors (post an announcement) and reflect them in the field's label/value

## G. Structure & navigation
- Headings — `.accessibilityAddTraits(.isHeader)`
- Set `.navigationTitle(...)`

## H. System settings (read via `@Environment`)
`\.accessibilityReduceMotion`, `\.accessibilityReduceTransparency`, `\.accessibilityDifferentiateWithoutColor`, `\.accessibilityInvertColors`, `\.legibilityWeight`, `\.colorSchemeContrast` — and the `UIAccessibility.is*Enabled` equivalents in UIKit.
