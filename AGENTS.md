# Authoring Agent Skills

Guidelines for creating skills in this repository.

## Directory Structure

Skills live in the `skills/` directory:

```
skills/
└── skill-name/
    ├── SKILL.md          # Required - main entry point
    └── references/       # Optional - detailed topic docs
```

## Naming

Use lowercase with hyphens. The directory name must match the `name` field in SKILL.md frontmatter:

- `react-router-framework-mode`
- `react-router-data-mode`
- `remix`

## SKILL.md Format

Follow the [Agent Skills specification](https://agentskills.io/specification).

### Required Frontmatter

```yaml
---
name: skill-name
description: What this skill does. Be specific about when agents should activate it.
license: MIT
---
```

### Recommended Sections

1. **Title & Intro** - Brief overview of the skill
2. **When to Apply** - Bullet list of specific trigger conditions
3. **References** - Table mapping topics to reference files
4. **Version Compatibility** - Feature/version requirements (if applicable)
5. **Critical Patterns** - Most important patterns with code examples
6. **Further Documentation** - Links to official docs

Example structure:

```markdown
# Skill Name

Brief description of what the skill covers.

## When to Apply

- Specific condition that triggers this skill
- Another trigger condition
- Yet another trigger

## References

| Reference             | Use When       |
| --------------------- | -------------- |
| `references/topic.md` | Working with X |
| `references/other.md` | Implementing Y |

## Critical Patterns

Show the most important patterns here with code examples.
```

### Size Guidelines

- Keep SKILL.md under 500 lines
- Move detailed reference material to `references/`
- Include only the most critical patterns inline

## Reference Files

Reference files provide detailed guidance on specific topics.

### Frontmatter

```yaml
---
title: Topic Name
description: Brief description of what this reference covers
tags: [relevant, searchable, tags]
---
```

### Recommended Sections

1. **Title** - Clear topic heading
2. **Overview** - Brief intro (1-2 sentences)
3. **Quick Reference Table** - Decision matrix or API summary
4. **Detailed Examples** - Code with explanations
5. **Anti-Patterns** - Common mistakes to avoid
6. **See Also** - Related references and external docs

### Code Examples

Always show correct and incorrect approaches:

```tsx
// ❌ DON'T: Description of the wrong approach
function badExample() {
  // problematic code
}

// ✅ DO: Description of the correct approach
function goodExample() {
  // correct code
}
```

Use comments to explain why one approach is preferred.

### Cross-Referencing

Link to related references using relative paths:

```markdown
See [other-topic.md](./other-topic.md) for details.
```

Use "See Also" sections at the end of files:

```markdown
## See Also

- [related-topic.md](./related-topic.md) - Brief description
- [Official Documentation](https://example.com/docs)
```

### Tables for Quick Reference

Use tables for decision matrices and API summaries:

```markdown
| Use Case         | Pattern      | Why                |
| ---------------- | ------------ | ------------------ |
| Search forms     | `Form GET`   | Updates URL params |
| Inline mutations | `useFetcher` | No page navigation |
```

## Adding a New Skill

1. Create `skills/skill-name/` directory
2. Add `SKILL.md` with required frontmatter and sections
3. Add `references/` directory if needed for detailed topics
4. Update root README.md to list the new skill

## Checklist

Before committing a new skill:

- [ ] Directory name matches `name` in frontmatter
- [ ] SKILL.md has "When to Apply" section
- [ ] SKILL.md under 500 lines
- [ ] Reference files have proper frontmatter
- [ ] Code examples show both correct and incorrect patterns
- [ ] Cross-references use relative paths
- [ ] README.md updated with skill listing
