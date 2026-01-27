# Authoring React Router Skills

Guidelines for creating new skills in this repository.

## Structure

Each skill lives in its own directory at the root:

```
skill-name/
├── SKILL.md          # Required
├── scripts/          # Optional - executable helpers
├── references/       # Optional - detailed docs
└── assets/           # Optional - templates, schemas
```

## SKILL.md Requirements

Follow the [Agent Skills specification](https://agentskills.io/specification):

```yaml
---
name: skill-name
description: What this skill does and when to use it.
license: MIT
---
```

- `name`: lowercase, hyphens only, must match directory name
- `description`: be specific about when agents should activate this skill
- Keep the main SKILL.md under 500 lines
- Move detailed reference material to `references/`

## Naming

Use lowercase with hyphens. Examples:
- `data-loading`
- `route-config`
- `form-handling`

## Adding a New Skill

1. Create a directory matching your skill name
2. Add a `SKILL.md` with required frontmatter
3. Update the root README.md to list the skill
