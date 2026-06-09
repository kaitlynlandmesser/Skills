---
name: feature-to-port-spec
description: Generate cross-platform port specs — one per feature — by reading PORT-GRAPH.md and, for each feature in port order, reconciling its shipped code with its PRD, interviewing the user on what reconciliation surfaces, and writing a spec the target-platform agent will consume. Works one feature at a time in a single attended session so the interview stays in context. Use when porting an app to another platform (iOS↔Android) and the user wants port specs written for the target agent. Runs after build-port-graph.
---

<what-to-do>

You are producing the **handoff interface** between the source-platform agent and the target-platform agent: a folder of port specs, one per feature, that the target agent will read to rebuild the app idiomatically. Each spec reconciles two sources — the **shipped code** (the truth about what was built) and the **PRD** (the truth about what's allowed to change).

This is an attended, conversational pass. The deep product grilling already happened upstream, at PRD and graph time; here you reconcile code against intent and resolve what *reconciliation* newly surfaces — code that diverged from its PRD, KEEP-vs-ADAPT calls that are genuinely unclear. Those questions need the user holding one feature's context in their head, so work **one feature at a time, in port order**, and grill in context. Never batch a shuffled pile of questions across many features at once — that context-switching is exactly what makes the interview painful and the answers worse.

Work in this order:

1. **Read `PORT-GRAPH.md` first.** It gives you the feature list, each feature's prerequisites (`needs`), and the recommended port order. If it's missing, say so and offer to run `build-port-graph` — without it you'd be rederiving dependencies per feature, and you'd lack the order that keeps the interview coherent.

2. **Decide the batch and order it.** Default to every feature; honor a narrower request ("just the editor and gallery"). Then sort the work by the graph's waves: shared foundations first, then the features that depend on them. Order matters precisely *because* you interview — settle a shared block's decisions once, in its own pass, and the features that inherit it won't re-litigate them.

3. **Then, for each feature in turn:**
   - **Gather its inputs** — the graph node (prerequisites), the PRD (from `screenshot-to-prd`), the shipped source, the screenshots (via the screenshot-reference convention), and `DESIGN-SYSTEM.md`.
   - **Reconcile and draft** — read the shipped code as the truth about what the feature does; where it disagrees with the PRD, the code wins and you note the divergence. Draft the spec and tag each notable decision **KEEP** or **ADAPT**.
   - **Grill in context** — ask the user about what reconciliation surfaced, one question at a time: a recommended answer attached for decisions (a KEEP-vs-ADAPT call), none for open-ended ones ("why did autosave diverge from the PRD?"). Resolve to completion before moving on — don't park a question you could ask now.
   - **Finalize** the spec into the feature's bundle (`port-specs/<feature-slug>/spec.md`), then move to the next feature.

4. **Index.** Once every feature is done, write `port-specs/INDEX.md` listing each bundle in port order with its prerequisites, so the target agent has a reading order. Report what was written and anything that genuinely couldn't be settled.

</what-to-do>

<supporting-info>

## Principles

### Code is what shipped; intent is what's allowed to change
Read the source code as the truth about what the feature actually does — when it disagrees with the PRD, the code wins and the divergence gets noted. Read the PRD for *why*, which is the only thing that tells the target agent which behaviors are product intent (reproduce them) versus source-platform conventions (replace them with the idiomatic equivalent). A spec that carries only the code produces a faithful translation of the wrong thing.

### Tag every notable decision: KEEP or ADAPT
This is the spec's reason to exist. For each meaningful choice, mark whether it's product intent to **keep** or a source-platform habit to **adapt**. A back-swipe dismissal is usually ADAPT (target users expect the system back button); a specific empty-state message is usually KEEP. The code can't make this distinction; the spec must.

### Go in port order, because you interview
You resolve decisions live, so sequence matters. Walk features in the graph's wave order so a shared foundation is settled in its own pass before the features that depend on it. That's what keeps entangled questions from ricocheting — a decision about a shared block is made once and inherited, not re-asked from three different features' contexts.

### Ask in context; don't park
Because this pass is attended, anything reconciliation surfaces gets asked during that feature's turn, while its context is in front of the user. A spec's Open questions section is only for what you genuinely can't settle — a product direction the user hasn't decided, or screens not yet captured — and is usually empty. Don't hand back a spec full of questions you could have asked.

### The spec and its screens are one handoff bundle
Each feature gets a folder — `spec.md` plus a `screens/` directory and a `screens.manifest.md` — and that folder is the interface that crosses the repo boundary. Don't copy or capture screenshots; `screenshot-reference` deposits them into `screens/` and you reference them by relative path (`./screens/<name>.png`), so there's a single source of truth and the bundle stays portable. If `screens/` doesn't exist yet, don't block: list the screens this feature needs (from the PRD and code) as a capture checklist, which is exactly `screenshot-reference`'s work list — the two skills hand off in both directions.

## Spec template

````markdown
# Port spec — <feature> (<source> → <target>)

> Source of truth: shipped <source> code. Intent: PRD at <path>.
> Read the prerequisites below before starting this feature.

## Prerequisites (port these first)
From the port graph: <needs list>.

## Intended behavior (from the PRD)
Problem, solution, screens & states, interactions, navigation, data — the product intent.

## Shipped reality (from the code)
What actually got built, and where it diverged from the PRD (code wins; note the divergence).

## Decisions — keep vs adapt
The heart of the spec. For each notable decision:
- **KEEP** — product intent; reproduce the behavior on <target>.
- **ADAPT** — a <source> convention; do the idiomatic <target> equivalent.

## Platform translation notes
Known <source>→<target> mappings this feature touches: gestures, navigation, components,
system integrations, permissions.

## Screens & screenshots
Embed each screen from `./screens/` beside the behavior it shows, so the spec renders visually
and the target agent gets a stable path:

`![<screen> — <state>](./screens/<name>.png)` — then a line on what this screen/state should do on the target platform.

Map every screen to its state using `screens.manifest.md`. If `screens/` isn't populated yet, replace the embeds with a **capture checklist** — the list of screens/states this feature needs shot — and note in Open questions that screens are pending.

## Open questions
Only what genuinely couldn't be settled in the interview — undecided product direction, or screens not yet captured. Usually empty.

## Behavioral parity checklist
Scenarios the target port must reproduce — the handoff to the test-parity skill.
````

</supporting-info>
