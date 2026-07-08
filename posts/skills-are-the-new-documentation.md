# Why Skill Files Just Made Your Documentation Obsolete

Your documentation is out of date. I don't need to look at it to know that. Documentation is a *copy* of how your
project works — and every copy starts drifting the second the original moves.

We've made peace with this for decades. The wiki lags the code. The onboarding doc mentions a setup step that got
deleted two sprints ago. We shrug and read the source instead.

Then AI showed up and we made it worse. Now we write docs for humans *and* a separate pile of instructions for the
model. Two copies of the same intent. Nobody keeps them in sync.

Here's what should stop you: a skill file is just markdown. A human reads it fine. The model reads it fine. Same file.
So why keep two?

The moment two documents describe one rule, you don't have two sources of truth. You have zero. It's the phone calendar
and the paper one on the fridge — the instant they disagree, you trust neither.

So keep one. The skill file becomes the single source of truth, and everything else points at it.

Now the part that changes the game: most documentation isn't a document you need — it's an answer you need. Overview,
onboarding, "why does this work this way," research into a corner of the codebase. Those were never really files. They
were questions we froze into files because no one was around to answer them on demand.

Now someone is. Point AI at the source of truth and it resolves those questions live — an overview for the new hire, an
onboarding path for one person's role, an explanation aimed at the exact thing they're confused about. Generated when
asked, from the real thing, then thrown away. Not a stale copy you maintain forever.

And when the source changes, the answers change with it. There's no separate doc to update because there's no separate
doc. Maintenance stops being a chore and becomes a side effect.

The cost of skipping this is concrete. It's the new hire following a wiki that describes a pattern you abandoned. It's
the model generating code against a rule the humans overruled in a meeting. Every frozen copy is a future contradiction
with a delay timer on it.

Documentation was never the goal. A team that agrees on how things work was the goal — docs were just our lossy attempt
to store that agreement outside our heads. Keep the agreement in one file, and let AI hand it to everyone in whatever
shape they need.

So stop writing docs *about* your project. Write the one file it actually runs on, and let the answers write themselves.