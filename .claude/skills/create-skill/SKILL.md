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