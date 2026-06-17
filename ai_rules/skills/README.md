# Agent Skills

Version-controlled Cursor skills for this project. Each skill lives in its own folder with a `SKILL.md` file.

| Skill | Use when |
|-------|----------|
| [generate-simple-rules](generate-simple-rules/SKILL.md) | Creating or updating rules in `documentation/rules/` or structure docs in `documentation/docs/` |

To use a skill locally in Cursor, symlink it into `.cursor/skills/`:

```bash
mkdir -p .cursor/skills
ln -sf ../../documentation/skills/generate-simple-rules .cursor/skills/generate-simple-rules
```

Add new skills under `documentation/skills/<skill-name>/SKILL.md` and link them here.
