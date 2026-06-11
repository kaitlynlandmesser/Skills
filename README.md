# Skills

A collection of [Claude Code](https://claude.com/claude-code) agent skills.

## Skills

| Skill | What it does |
| --- | --- |
| [`screenshot-to-prd`](screenshot-to-prd/SKILL.md) | Turn one or more UI screenshots — mockups, competitor screens, sketches, Figma/Stitch exports, or shots of an existing screen — into a feature PRD. Reads the image, interviews you one question at a time grounded in the codebase, then synthesizes a UI-driven spec. |
| [`screenshot-to-design-system`](screenshot-to-design-system/SKILL.md) | Infer the simplest design system that explains a set of UI images — colors, type scale, spacing, radii, elevation, recurring components — confirming low-confidence reads (especially fonts) one question at a time, then writes `DESIGN-SYSTEM.md` plus a self-contained HTML proof sheet. Runs once to establish the visual foundation. |
| [`build-port-graph`](build-port-graph/SKILL.md) | Map an app's cross-feature dependencies once — which features need which shared building blocks and the order to port them in — into a `PORT-GRAPH.md` (with a Mermaid view) that per-feature export specs read for their prerequisites. Derived from the codebase, confirmed one question at a time. Runs once before exporting any port specs. |
| [`mobile-port-spec`](mobile-port-spec/SKILL.md) | Generate cross-platform port specs (default iOS↔Android), one per feature, by reading `PORT-GRAPH.md` and — one feature at a time, in port order — reconciling its shipped code with its PRD, tagging every decision KEEP or ADAPT, and interviewing the user in context on what reconciliation surfaces. Confirms source→target direction and all-vs-one scope up front. An attended pass that keeps the interview coherent; each bundle is the handoff interface for the target-platform agent. Runs after `build-port-graph`. |
| [`voice-preserving-editor`](voice-preserving-editor/SKILL.md) | Turn a rough draft or pile of notes into a finished piece without losing the author's voice. Judges whether there's enough real human content (asks for more if not), proposes a structure for approval, captures voice from a `VOICE.md` or the draft itself, then interviews one question at a time — recommending answers for editorial decisions but never for open-ended substance prompts — before weaving a draft in the author's own words. Organizes and elicits human writing; never generates from nothing. |
| [`accessibility-audit`](accessibility-audit/SKILL.md) | Audit iOS (SwiftUI/UIKit) and Android (Compose/Views) screens for accessibility issues against Apple HIG and Google/Material guidance — reading the code file by file with platform-specific mechanism references — and produce `ACCESSIBILITY-AUDIT.md` plus an interactive, filterable HTML dashboard. Identifies and locates issues by `file:line`; leaves fixes to a separate pass and routes anything a static read can't confirm to a manual-verification section. |
| [`what-could-i-build`](what-could-i-build/SKILL.md) | Generate candidate ideas from the author's own skills and interests (the divergence front-end to `what-should-i-build`), built on Guillebeau's convergence concept. Builds or accepts a convergence lens, proposes ideas at lens *intersections* (not single interests), then runs an adoption gate where the author claims, edits, or rejects each one before it's scored. Everything leaves flagged unvalidated and hands off to the scorer; never lets a brainstorm pose as a shortlist. |
| [`what-should-i-build`](what-should-i-build/SKILL.md) | Decide which of several candidate ideas to build next, using Chris Guillebeau's *$100 Startup* convergence + market-testing framework. Interviews the author for a skills/interests convergence lens, scores every idea they bring in a basis-carrying table (skill fit = your read, interest = theirs, demand = a search), market-tests the top three one at a time, and fills a one-page business plan for the survivor. Scores the author's ideas; never invents new ones. |

## Installing

Each skill is a directory containing a `SKILL.md`. To use one, copy its directory into your skills folder:

```bash
# Personal (all projects)
cp -r screenshot-to-prd ~/.claude/skills/

# Or per-project
cp -r screenshot-to-prd /path/to/project/.claude/skills/
```

Claude Code discovers the skill from its `SKILL.md` frontmatter and surfaces it by name (e.g. `/screenshot-to-prd`).

## Structure

```
skill-name/
└── SKILL.md      # frontmatter (name + description) and instructions
```

## License

MIT
