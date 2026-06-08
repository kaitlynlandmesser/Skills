# Skills

A collection of [Claude Code](https://claude.com/claude-code) agent skills.

## Skills

| Skill | What it does |
| --- | --- |
| [`screenshot-to-prd`](screenshot-to-prd/SKILL.md) | Turn one or more UI screenshots — mockups, competitor screens, sketches, Figma/Stitch exports, or shots of an existing screen — into a feature PRD. Reads the image, interviews you one question at a time grounded in the codebase, then synthesizes a UI-driven spec. |

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
