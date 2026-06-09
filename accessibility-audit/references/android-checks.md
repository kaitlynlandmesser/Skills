# Android mechanisms — Jetpack Compose / Views

The exact APIs and thresholds for each checklist category on Android. Bar: Google/Material accessibility guidance.

## A. Screen-reader semantics (TalkBack)
- Accessible name — Compose `Modifier.semantics { contentDescription = "…" }` or the `contentDescription` parameter; Views `android:contentDescription`
- Hide decorative — Compose `contentDescription = null` (for images) or `Modifier.clearAndSetSemantics {}`; Views `importantForAccessibility="no"`
- Role — Compose `Modifier.semantics { role = Role.Button }`; mark headings with `Modifier.semantics { heading() }`
- State — Compose `Modifier.semantics { stateDescription = "…" }`; `toggleableState` for toggles
- Grouping — Compose `Modifier.semantics(mergeDescendants = true) {}` to read a cluster as one
- Reading/traversal order — Compose `Modifier.semantics { isTraversalGroup = true }` plus `traversalIndex`
- Live region — Compose `Modifier.semantics { liveRegion = LiveRegionMode.Polite }`; Views `accessibilityLiveRegion`
- Custom actions — Compose `Modifier.semantics { customActions = listOf(...) }` so gesture-only interactions are reachable
- Modal — manage focus so background content isn't reachable behind sheets/dialogs

## B. Text scaling
- Use **`sp`** units for text, never `dp` or hardcoded `textSize`; respect the system `fontScale`
- Suppressing scaling — flag `maxLines` + `ellipsize` on essential text, or fixed-height containers around text
- Support large font scales without clipping

## C. Contrast & colour
- Compare declared colour vs declared surface; Material guidance mirrors the 4.5:1 / 3:1 ratios
- Not colour-alone — pair status colour with icon/text/shape
- Check light and dark theme

## D. Touch targets
- Minimum **48×48 dp** (Material). Compose `Modifier.minimumInteractiveComponentSize()` or sized accordingly; Views ensure `minWidth`/`minHeight` or touch-delegate

## E. Motion, timing, gestures
- Reduce motion — check the system animator duration scale (animations off) before running non-essential animation
- Complex/custom gestures need an accessible alternative (custom action)

## F. Forms & input
- Error semantics — Compose `Modifier.semantics { error("…") }`; Views associate via `labelFor`/`hint`
- Correct `inputType` / keyboard; required state announced, not colour-only

## G. Structure & navigation
- Headings — Compose `Modifier.semantics { heading() }`
- Set screen/`Toolbar` titles

## H. System settings
Honor system font scale, bold/high-contrast text, "remove animations", and colour-correction/high-contrast settings; read the relevant `Settings`/`AccessibilityManager` state before adapting behaviour.
