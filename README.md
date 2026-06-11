# Skills

The skills I wrote for my solo app projects, built with [Claude Code](https://claude.com/claude-code).

A quick honesty note: these are opinionated, mostly untested, and I expect to keep adapting them as I go. You can get most of the way without any of this — I still believe that. But I wanted to do better, so here they are. Try them out, and tell me what works and what breaks.

## Skills

| Skill | What it does |
| --- | --- |
| [`screenshot-to-prd`](screenshot-to-prd/SKILL.md) | Hand it a UI screenshot — a mockup, a competitor's screen, a sketch, a Figma or Stitch export, a shot of an existing screen — and it turns it into a feature PRD. It reads the image, grills you one question at a time (grounded in your codebase), then writes a UI-driven spec. |
| [`screenshot-to-design-system`](screenshot-to-design-system/SKILL.md) | Figures out the simplest design system that explains a set of UI images — colors, type scale, spacing, radii, elevation, the components that keep showing up. It checks the reads it's unsure about (fonts, especially) one question at a time, then writes a `DESIGN-SYSTEM.md` plus a self-contained HTML proof sheet so you can see it in action. Run it once to set your visual foundation. |
| [`build-port-graph`](build-port-graph/SKILL.md) | Maps your app's cross-feature dependencies once — what needs which shared building blocks, and the order to port them in — into a `PORT-GRAPH.md` with a Mermaid view. The per-feature export specs read it for their prerequisites. Derived from your code, confirmed one question at a time. Run it once before you export any port specs. |
| [`mobile-port-spec`](mobile-port-spec/SKILL.md) | Generates cross-platform port specs (iOS↔Android by default), one per feature. It reads the `PORT-GRAPH.md` and works one feature at a time, in port order — reconciling the shipped code against its PRD, tagging every decision KEEP or ADAPT, and asking you about whatever that surfaces. It confirms direction (source→target) and scope (all features vs. one) up front. This is an *attended* pass — it stays in a single session so the interview keeps its context, and each spec is the handoff to the agent building the other platform. Run it after `build-port-graph`. |
| [`accessibility-audit`](accessibility-audit/SKILL.md) | Audits iOS (SwiftUI/UIKit) and Android (Compose/Views) screens against Apple HIG and Google/Material guidance, reading the code file by file. It writes an `ACCESSIBILITY-AUDIT.md` plus an interactive, filterable HTML dashboard, and points you to each issue by `file:line`. It finds and locates problems — fixing them is a separate pass — and anything a static read can't confirm gets routed to a manual-verification section. Run it as a first pass, then again later to catch what new code dragged in. |
| [`voice-preserving-editor`](voice-preserving-editor/SKILL.md) | My favorite skill. Turns a rough draft or a pile of notes into a finished piece without flattening your voice. It checks there's enough real human material to work with (and asks for more if there isn't), proposes a structure for you to approve, learns your voice from a `VOICE.md` or the draft itself, then interviews you one question at a time — recommending answers on editorial calls, but never on the open-ended substance — before weaving a draft in *your* words. It edits and draws things out of you. It doesn't make things up. |
| [`what-could-i-build`](what-could-i-build/SKILL.md) | Generate candidate ideas from the author's own skills and interests (the divergence front-end to `what-should-i-build`), built on Guillebeau's convergence concept. Builds or accepts a convergence lens, proposes ideas at lens *intersections* (not single interests), then runs an adoption gate where the author claims, edits, or rejects each one before it's scored. Everything leaves flagged unvalidated and hands off to the scorer; never lets a brainstorm pose as a shortlist. |
| [`what-should-i-build`](what-should-i-build/SKILL.md) | Decide which of several candidate ideas to build next, using Chris Guillebeau's *$100 Startup* convergence + market-testing framework. Interviews the author for a skills/interests convergence lens, scores every idea they bring in a basis-carrying table (skill fit = your read, interest = theirs, demand = a search), market-tests the top three one at a time, and fills a one-page business plan for the survivor. Scores the author's ideas; never invents new ones. |


## Installing

Each skill is just a directory with a `SKILL.md` inside. To use one, copy its directory into your skills folder:

```bash
# Personal (all projects)
cp -r screenshot-to-prd ~/.claude/skills/

# Or per-project
cp -r screenshot-to-prd /path/to/project/.claude/skills/
```

Claude Code picks it up from the `SKILL.md` frontmatter and surfaces it by name (e.g. `/screenshot-to-prd`).

## Structure

```
skill-name/
└── SKILL.md      # frontmatter (name + description) and instructions
```

## License

MIT
