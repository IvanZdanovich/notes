# Normalizing AI Code Generation

Silent LLM updates, downgrades, or changes in token-based pricing by providers can break code and workflow consistency.
Regardless of the reasons, the focus now should shift toward improving accuracy and consistency of generated output.
That means using tools more effectively, exploring new approaches, and experimenting with different models.
The core problem is simple: how do we keep output consistent (not deterministic, but stable), especially when models
change or are silently replaced?
The most practical tool here is instructions (skills). I'll refer to them as instructions, because that term is clearer
and less misleading.
Regardless of the terminology I use, Skills are supported by both GitHub Copilot and Claude Code, which makes them the
preferred choice — a single source of truth for both systems.

## What Instructions Actually Do

Instructions don't make models smarter.
They:

- structure output
- reduce randomness
- introduce repeatable patterns

This creates partial determinism — not perfect, but stable enough to rely on.

## Scope Is Everything

Instructions should not be global. They work best when applied to specific file types or patterns or parts of workflow -
linked to specific code structures.
Trying to generalize across everything leads to vague rules and weaker results.

In theory, skills describe workflows.
In practice, they become misleading when you define too much in one place.
To keep them effective:

- keep them scoped
- tie them to file patterns
- limit responsibility

In reality, skills behave the same way as instructions — the only difference is that instructions explicitly link to
file patterns, while skills are more loosely defined.
The key is to avoid overloading either with too much responsibility or too broad a scope.

## The Real Problem: Formatting

Most instructions are written like documentation for a junior developer:

- long
- conversational
- explanatory

This leads to:

- poor maintainability
- difficult debugging
- unclear cause → effect relationships
- inconsistent results

Instead of guiding the model, this approach overloads it.
The fix is simple: remove narrative and use structured signals. Treat the model as an experienced but lazy developer. It
doesn't need explanations. It needs clear, structured signals.

Use a flat KEY → VALUE structure.
Here is the example of a skill for creating new skills in a compact, rule-based format.

```markdown
---
name: create-skill
description: Use when creating or updating a skill file in `.claude/skills/`. Produces compact, tag-based instruction files that follow the project ruleset.
---

PURPOSE: guide production of token-efficient SKILL.md
SCOPE: `.claude/skills/<skill-name>/SKILL.md`
FRONTMATTER: `name` (kebab-case), `description` (one-line trigger sentence)
HEADING_LEVELS: H1 skill title, H2 major sections
TAG_FORMAT: `UPPER_SNAKE_CASE: value`
TAG_COUNT: one rule per line
TAG_STYLE: desired pattern as noun tag, positive framing, stable vocabulary
LIST_STYLE: comma-separated inline for compact sets
SCOPE_SPECIFICITY: one skill per responsibility, no overlapping scopes

TAG_CHECK: UPPER_SNAKE_CASE, one rule per line, noun-led
FRAMING_CHECK: positive guidance and desired patterns throughout
TOKEN_CHECK: no filler phrases, no narrative prose
SCOPE_CHECK: frontmatter `description` matches the skill's actual trigger
PATH_CHECK: declared paths exist in the workspace

```

This format is:

- token-efficient
- easy to scan
- easy to maintain
- easier to enforce
- better for traceability

No explanations, no prose, no ambiguity — only signals.
The model doesn't "read" this. It aligns to it.

## Final Takeaway

If your instructions are:

- scoped
- minimal
- consistent

You can achieve stable, predictable output — even when models change underneath you.
Not deterministic, but controlled enough to trust.