---
name: generate-simple-rules
description: >-
  Creates concise project rules and structure docs in the swissPass style.
  Use when the user asks to add or update rules, guidelines, conventions,
  documentation/rules, documentation/docs, or .mdc rule files.
---

# Generate Simple Rules

Write rules the way this project does: short, imperative, no filler.

## Before writing

1. Confirm scope: one topic per file.
2. Ask if unclear whether the output is a **rule** (enforce behavior) or a **structure doc** (record why/when/what).
3. Do not duplicate content already in another rule or doc.

## Where files go

| Type | Path | Purpose |
|------|------|---------|
| Behavioral / cross-cutting rules | `documentation/rules/general_guidelines.mdc` or new `.mdc` there | Always-on agent behavior |
| Domain rules (Flutter, API, etc.) | `documentation/rules/<topic>_guidelines.mdc` | Topic-specific constraints |
| Structure / migration notes | `documentation/docs/<topic>.md` | Why, when, what only |

## Rule file format (`.mdc`)

```markdown
---
description: One line — what it enforces and when to use it
alwaysApply: true
---

- Imperative bullet. No intro paragraph.
- Another rule. Prefer "Do X" / "Do not Y".
```

Use `globs: **/*.dart` instead of `alwaysApply: true` when the rule applies only to certain files.

## Style constraints

Match `documentation/rules/general_guidelines.mdc` and `documentation/rules/flutter_guidelines.mdc`:

- **Length:** Domain rules ~10–15 lines. Behavioral rules may use numbered sections with a bold one-liner each.
- **Bullets over prose.** No tutorials, no history essays, no "Notes" sections.
- **No code examples** unless one line is required to disambiguate (e.g. a helper name).
- **Imperative voice.** State what to do, not why Flutter works.
- **No speculative rules.** Only what the user asked for or what the codebase already does.
- **No duplicate "how to"** in structure docs when a rule file already covers workflow.

## Structure doc format (`documentation/docs/`)

Only when architecture or folder layout changed:

```markdown
# Topic

- **Why:** One sentence.
- **When:** Date or milestone.
- **What:** Paths, commands, and entry points — bullet list, no steps tutorial.
```

## Workflow

1. Read existing rules in `documentation/rules/` to avoid overlap.
2. Draft the smallest file that fully covers the request.
3. If adding a structure change, update or create the matching doc under `documentation/docs/`.
4. Show the user the file path and line count; ask before expanding.

## Anti-patterns

- Long "How it works" sections with numbered steps
- Repeating the same rule in both `.mdc` and `.md`
- Generic best-practice essays the agent already knows
- Examples blocks, migration narratives, or "Notes" appendices
