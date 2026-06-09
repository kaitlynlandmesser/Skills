---
name: feature-to-port-spec
description: Generate cross-platform port specs — one per feature — by reading PORT-GRAPH.md and reconciling each feature's shipped code with its PRD, then fanning the work out to one subagent per feature so the specs are written in parallel into a shared folder. Each spec is the handoff interface the target-platform agent will consume. Use when porting an app to another platform (iOS↔Android) and the user wants to export specs for many features at once, "generate the port specs," or "write the specs for the Android agent." Runs after build-port-graph. Falls back to sequential generation where subagents aren't available.
---

<what-to-do>

You are producing the **handoff interface** between the source-platform agent and the target-platform agent: a folder of port specs, one per feature, that the target agent will read to rebuild the app idiomatically. Each spec reconciles two sources — the **shipped code** (the truth about what was built) and the **PRD** (the truth about what's allowed to change). The human grilling already happened when the PRD was written; this skill synthesizes, it does not re-interview.

Work in this order:

1. **Read `PORT-GRAPH.md` first.** It gives you the feature list, each feature's prerequisites (`needs`), and the recommended port order. If it's missing, say so and offer to run `build-port-graph` — without it you'd be rederiving dependencies per feature, which is the thing the graph exists to prevent.

2. **Decide the batch.** Default to every feature in the graph; honor a narrower request ("just the editor and gallery"). For each feature, gather its inputs: its graph node (prerequisites), its PRD (from `screenshot-to-prd`), the shipped source paths, the screenshots (via the screenshot-reference convention), and `DESIGN-SYSTEM.md`.

3. **Fan out — one subagent per feature.** Spec generation is independent across features, so spawn workers in parallel via the Task tool (`subagent_type: general-purpose`), up to ~10 at a time; queue the rest. Because each subagent starts with a fresh, isolated context, give it a **self-contained brief** (template below) — it inherits nothing from this conversation. Where subagents aren't available, generate the specs sequentially in this session instead; the output is identical.

4. **Each worker writes one spec** into its feature's bundle folder (default `port-specs/<feature-slug>/spec.md`) using the spec template below. The bundle is the unit of handoff: `spec.md` lives beside a `screens/` folder that `screenshot-reference` populates, so the target agent gets one self-contained folder per feature. A worker does not interview and does not spawn further subagents — if it hits a real ambiguity, it records it under **Open questions** and keeps going.

5. **Collect and index.** Once the workers return, write `port-specs/INDEX.md` listing each feature's bundle in recommended port order (from the graph) with its prerequisites, so the target agent has a reading order. Report which specs were written and surface any open questions the workers deferred.

</what-to-do>

<supporting-info>

## Principles

### Code is what shipped; intent is what's allowed to change
Read the source code as the truth about what the feature actually does — when it disagrees with the PRD, the code wins and the divergence gets noted. Read the PRD for *why*, which is the only thing that tells the target agent which behaviors are product intent (reproduce them) versus source-platform conventions (replace them with the idiomatic equivalent). A spec that carries only the code produces a faithful translation of the wrong thing.

### Tag every notable decision: KEEP or ADAPT
This is the spec's reason to exist. For each meaningful choice, mark whether it's product intent to **keep** or a source-platform habit to **adapt**. A back-swipe dismissal is usually ADAPT (target users expect the system back button); a specific empty-state message is usually KEEP. The code can't make this distinction; the spec must.

### Specs are independent; the graph orders porting, not writing
You can generate all specs at once no matter what depends on what — the dependencies live *inside* each spec as its prerequisites, not as a constraint on generation order. Only the eventual porting follows the graph's waves.

### Workers defer, they don't block
A subagent can't reach the human mid-run. Anything it can't resolve goes into the spec's Open questions for the human or the target agent to settle. Never let a worker stall waiting for input it can't get.

### The spec and its screens are one handoff bundle
Each feature gets a folder — `spec.md` plus a `screens/` directory and a `screens.manifest.md` — and that folder is the interface that crosses the repo boundary. Don't copy or capture screenshots; `screenshot-reference` deposits them into `screens/` and you reference them by relative path (`./screens/<name>.png`), so there's a single source of truth and the bundle stays portable. If `screens/` doesn't exist yet, don't block: list the screens this feature needs (from the PRD and code) as a capture checklist, which is exactly `screenshot-reference`'s work list — the two skills hand off in both directions.

## Subagent brief (hand this to each worker, fully self-contained)

```
Generate one cross-platform port spec.

- Feature: <name>
- Direction: <source> → <target>
- Prerequisites (from PORT-GRAPH.md): <needs list>
- PRD: <path or "none — derive intent from code">
- Shipped source: <paths / module>
- Screenshots: port-specs/<feature-slug>/screens/ + screens.manifest.md (populated by screenshot-reference; if absent, list the screens this feature needs as a capture checklist)
- Design system: <DESIGN-SYSTEM.md path or "none">
- Write the spec to: port-specs/<feature-slug>/spec.md
- Reference screens by relative path (./screens/<name>.png); do not copy or capture them.
- Use the spec template provided.
- Reconcile code (what shipped) with PRD (intent); code wins on conflicts, note divergence.
- Tag each notable decision KEEP or ADAPT.
- Do NOT interview anyone. Do NOT spawn subagents. Defer unresolved ambiguities to Open questions.
```

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
Embed each screen from `./screens/` beside the behavior it shows, so the spec renders visually and the target agent gets a stable path:

`![<screen> — <state>](./screens/<name>.png)` — then a line on what this screen/state should do on the target platform.

Map every screen to its state using `screens.manifest.md`. If `screens/` isn't populated yet, replace the embeds with a **capture checklist** — the list of screens/states this feature needs shot — and add an Open question noting screens are pending.

## Open questions (deferred)
Ambiguities the autonomous run couldn't resolve — for the human or the target agent. Not blocking.

## Behavioral parity checklist
Scenarios the target port must reproduce — the handoff to the test-parity skill.
````

</supporting-info>
