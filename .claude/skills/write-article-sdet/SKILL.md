---
name: write-article-sdet
description: Use when writing, converting, or refining a technical article for an SDET/IT audience. Structured, reference-grade, problem-to-solution format. Sharp, sarcastic tone. Exposes misleading trends and hidden mistakes using absurd real-life analogies.
---

# Write article

PURPOSE: review, refinement, finalization of a draft or outline into a polished, comprehensive article
AUDIENCE: software engineers, SDETs, tech practitioners
TASK: review, refine coverage and argumentation, transform draft_content into final article

## Structure

HEADINGS: H2 for major sections, H3 for subsections; scannable
INTRO: state the problem and why it matters; call out the mainstream claim being challenged if applicable
CONCLUSION: one actionable takeaway or summary; no restatement

## Style

DENSITY: max signal per token, no filler
LENGTH: long-form; cover the topic completely
REPETITION: each idea stated once only
MARKETING: none; factual and direct throughout

## Content

TERMS: concrete, specific
ANALOGIES: use obvious, even absurd real-life comparisons to ground abstract concepts, prefer everyday situations
ANGLE: prefer myth-busting — identify the mainstream trend or popular belief, then dismantle it with evidence and
examples; expose what the trend gets wrong and what it silently breaks
HIDDEN_MISTAKES: actively name the mistakes practitioners make when following the trend blindly; make the cost visible
CODE: include when it adds insight; clean, runnable if possible
CODE_COMMENTS: only when non-obvious
LISTS: use numbered lists for steps/ordered items, bullets for unordered sets
LINKS: include cross-references to related articles when relevant
THEORY: replaced by examples wherever possible

## Syntax

ENGLISH_LEVEL: upper-intermediate; clear, idiomatic; prefer active voice
JARGON: allowed when precise; never decorative

## Tone

STANCE: sharp, direct, sarcastic when warranted — treat cargo-cult practices and buzzword dogma with visible skepticism
FOCUS: efficiency, actionability, simplicity
OPINION: grounded in evidence or experience; state it plainly, don't hedge
SARCASM: use to expose absurdity of mainstream mistakes; never punch at the reader, always punch at the bad idea

## Output

FORMAT: final article only
REASONING: explicit thoughts or real-life example for every problem and solution
FRONT_MATTER: H1 title required; optional subtitle

## Quality checks

ACTIONABLE: every section teaches or enables something concrete
USABLE: reader can apply content immediately
SIGNAL_CHECK: remove any sentence that restates the previous one
STRUCTURE_CHECK: sections follow a logical problem → solution arc
MYTH_CHECK: if article challenges a trend, the flaw must be demonstrated with a concrete example, not just stated
ANALOGY_CHECK: every abstract concept has at least one grounded example or analogy
