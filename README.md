# Skills

A collection of [Claude Code](https://claude.com/claude-code) agent skills.

## Skills

| Skill | What it does |
| --- | --- |
| [`screenshot-to-prd`](screenshot-to-prd/SKILL.md) | Turn one or more UI screenshots — mockups, competitor screens, sketches, Figma/Stitch exports, or shots of an existing screen — into a feature PRD. Reads the image, interviews you one question at a time grounded in the codebase, then synthesizes a UI-driven spec. |
| [`screenshot-to-design-system`](screenshot-to-design-system/SKILL.md) | Infer the simplest design system that explains a set of UI images — colors, type scale, spacing, radii, elevation, recurring components — confirming low-confidence reads (especially fonts) one question at a time, then writes `DESIGN-SYSTEM.md` plus a self-contained HTML proof sheet. Runs once to establish the visual foundation. |
| [`build-port-graph`](build-port-graph/SKILL.md) | Map an app's cross-feature dependencies once — which features need which shared building blocks and the order to port them in — into a `PORT-GRAPH.md` (with a Mermaid view) that per-feature export specs read for their prerequisites. Derived from the codebase, confirmed one question at a time. Runs once before exporting any port specs. |
| [`feature-to-port-spec`](feature-to-port-spec/SKILL.md) | Generate cross-platform port specs (iOS↔Android), one per feature, by reading `PORT-GRAPH.md` and reconciling each feature's shipped code with its PRD — tagging every decision KEEP or ADAPT — then fanning the work out to one subagent per feature so specs are written in parallel into a shared `port-specs/` folder. Each bundle is the handoff interface for the target-platform agent. Runs after `build-port-graph`. |
| [`voice-preserving-editor`](voice-preserving-editor/SKILL.md) | Turn a rough draft or pile of notes into a finished piece without losing the author's voice. Judges whether there's enough real human content (asks for more if not), proposes a structure for approval, captures voice from a `VOICE.md` or the draft itself, then interviews one question at a time — recommending answers for editorial decisions but never for open-ended substance prompts — before weaving a draft in the author's own words. Organizes and elicits human writing; never generates from nothing. |

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
