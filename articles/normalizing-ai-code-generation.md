# Stop Prompting, Start Constraining: How to Keep AI Code Output Stable When the Model Changes Underneath You

You wrote instructions that worked. The generated code was clean, the patterns held, your team trusted the output. Then
one morning it all drifts. Same instructions, worse results. Nothing in your repo changed — but the model did.

Providers ship silent updates, quiet downgrades, and token-pricing changes that reshape behavior without a changelog.
The model you tuned against last month may already be gone. This is the uncomfortable part most AI-tooling advice skips:
you are building on a foundation that moves.

So chasing a "better prompt" for a specific model is a losing game. The reasons behind each change don't matter. What
matters is the output — its accuracy and its consistency — and whether you can keep it stable when the ground shifts.
The core question is narrow and practical: how do you keep output *consistent* (not deterministic, but stable) even when
models change or are silently replaced?

The cost of ignoring this is concrete. Every silent shift becomes hours of re-reviewing generated code you thought you
could trust, patterns that quietly diverge across a codebase, and a team that stops believing the tool. Instability
doesn't announce itself — it leaks in as rework.

The most practical lever is instructions. Both GitHub Copilot and Claude Code call them Skills, and both support them —
which makes a single instruction file a single source of truth for both systems. I'll call them *instructions*
throughout, because the word is clearer and less misleading than "skills." The terminology doesn't change how they work.

## What Instructions Actually Do (and What They Don't)

Here's the myth worth killing first: instructions do not make models smarter. They add no knowledge and no reasoning.

What they do is narrower and more useful. Instructions:

- structure the output
- reduce randomness
- introduce repeatable patterns

Think of it like guardrails on a bowling lane. The ball isn't more skilled, and no two throws are identical — but it
stops ending up in the gutter. That's the goal here: **partial determinism**. Not perfect, but stable enough to rely on
across model changes.

## Scope Is Everything: Why Global Rules Fail

Instructions should not be global. They work best tied to specific file types, patterns, or parts of a workflow —
anchored to concrete code structures.

The failure mode is trying to generalize across everything. Broad rules become vague rules, and vague rules produce
weaker, less predictable results. A rule that applies "always, everywhere" is a rule the model can satisfy in a hundred
conflicting ways.

In theory, instructions describe workflows. In practice, they turn misleading the moment you pile too much into one
place. To keep them effective:

- keep them scoped
- tie them to file patterns
- limit responsibility per file

Instructions and skills behave the same way in the end. The only real difference: instructions explicitly link to file
patterns, while skills are more loosely defined. Either way, the trap is identical — overloading one file with too much
responsibility or too broad a scope. A scalpel beats a Swiss Army knife when the job is precise.

## The Real Problem Is Formatting, Not Content

Most instructions are written like documentation for a junior developer:

- long
- conversational
- explanatory

That style feels helpful. It isn't. It produces:

- poor maintainability
- difficult debugging
- unclear cause-and-effect relationships
- inconsistent results

Narrative prose gives the model too many degrees of freedom. A paragraph explaining *why* to do something invites
interpretation; interpretation is exactly the randomness you're trying to remove. Instead of guiding the model,
explanation overloads it.

The fix is simple: strip the narrative and use structured signals. Treat the model as an experienced but lazy developer.
It doesn't need the reasoning. It needs clear, structured signals it can align to.

Use a flat KEY → VALUE structure. Below is a real example — an instruction for creating new instruction files, written
in exactly this compact, rule-based format:

```markdown
---
name: create-skill
description: Use when creating or updating a skill file in `.claude/skills/`. Produces compact, tag-based instruction files that follow the project ruleset.
---

# Principles

PURPOSE: a skill is a self-contained, reusable SKILL.md that shifts the model's output toward a desired pattern —
otherwise it is prose absorbed once, not an instruction the model can reapply
LIFETIME_COST: optimize tokens across every future load plus the downstream cost of misapplying a rule, not the file's
raw length — otherwise a rule that misfires downstream costs more than the tokens it saved
SIGNAL: a rule earns its load cost only by changing what the model produces — otherwise it restates the model's default,
shifting nothing at pure cost
REASONING: a divergent rule's why lets the model extend it to unnamed cases — otherwise the bare rule reverts to the
model's default at the first case it does not name
INVERSION: put each rule's failure mode in its why, not in a negative directive — otherwise a positive-only rule hides
the cost it guards against and the reader cannot separate the pattern from its rationale
SEGMENTATION: one skill per responsibility, one section per concept, one tag per rule — otherwise a rule mixing concerns
cannot be reused or removed without collateral damage

# Method

SCOPE_PATH: write to `.claude/skills/<skill-name>/SKILL.md`
FRONTMATTER: `name` (kebab-case), `description` (one-line trigger sentence)
SECTIONS: frontmatter, principles, method, validation
TAG_FORMAT: `UPPER_SNAKE_CASE: value`, one rule per line, noun-led
TAG_STYLE: name the desired pattern as a noun tag, positive framing, stable vocabulary
KEEP_TEST: delete each candidate rule and keep it only if the model's output would change
WHY_CLAUSE: write each divergent rule as a positive directive, then `— otherwise <the failure that omitting it causes>`;
leave arbitrary conventions bare
FRAMING: express limits as orderings and comparisons, not arithmetic thresholds the model self-counts; prefer
domain-loaded words over coined abstractions
FLOW_STYLE: express a sequence as an arrow chain or named phase tags, never a numbered list — the model tracks named
stages and transitions, not ordinals
DEDUP: fold any rule that restates another rule or a section heading
REVERSE_BRAINSTORM: add tags that block known failures — trivial signals, content restated across skills, narrative
prose, markdown tables, numbered steps, generic names and values, mixed tag responsibilities
CODE_EXAMPLE: fenced snippet only where it resolves ambiguity a tag cannot, never as decoration

# Validation

TAG_CHECK: UPPER_SNAKE_CASE, one rule per line, noun-led
SIGNAL_CHECK: every rule passes KEEP_TEST; each divergent rule carries its why, each convention stays bare
FRAMING_CHECK: positive directives throughout, each divergent rule's why introduced by `— otherwise` and naming the
failure it prevents
TOKEN_CHECK: no filler phrases, no narrative prose, no markdown tables, no numbered lists, no decorative code fences
SCOPE_CHECK: frontmatter `description` matches the skill's actual trigger
PATH_CHECK: declared paths exist in the workspace
SECTION_CHECK: mandatory sections present — Principles, Method, Validation
```

Notice what this format buys you. It is:

- token-efficient
- easy to scan
- easy to maintain
- easier to enforce
- better for traceability

No explanations, no prose, no ambiguity — only signals. The model doesn't "read" this the way it reads a paragraph. It
aligns to it.

## Final Takeaway

Remember the morning when working instructions quietly went bad after a silent model swap? You can't stop providers from
changing the model underneath you. You can stop that change from wrecking your output.

Keep every instruction **scoped**, **minimal**, and **consistent**, and you get stable, predictable results even as
models shift beneath you — not deterministic, but controlled enough to trust. Start today: open your longest instruction
file and cut it down to signals.