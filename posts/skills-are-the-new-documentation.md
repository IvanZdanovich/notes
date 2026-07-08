# Why Skill Files Just Made Your Documentation Obsolete

Your documentation is out of date. I don't need to look at it to know that. Documentation is a *copy* of how your
project actually works — and every copy starts drifting the second the original moves.

We've made peace with this for decades. The wiki lags the code. The onboarding doc describes a setup step that got
deleted two sprints ago. We shrug and read the source instead.

Then AI showed up and we made the problem worse.

Now we write docs for humans *and* a separate pile of instructions for the model. Two artifacts describing the same
intent. Two things to keep in sync. Nobody keeps them in sync. So the human docs rot in one direction and the AI
instructions rot in another, and now you're maintaining two lies instead of one.

Here's the thing that should stop you: a skill file is just markdown. A human reads it fine. The model reads it fine.
It's the *same file*. So why on earth are you keeping two?

The moment you have two documents describing one rule, you don't have two sources of truth. You have zero. It's the
phone calendar and the paper one stuck to the fridge — the instant they disagree, you stop trusting both and start
asking people directly. Redundancy didn't buy you safety. It bought you doubt.

So flip it. The skill file becomes the single source of truth. Not "the AI's copy of the docs" — *the* docs. The rule
lives in exactly one place, and everything else points at it.

That one move changes what documentation *is*. It stops being a separate document you write, forget, and slowly betray.
It becomes a projection of the file your team and your model already obey. Human wants the overview? They read the
skill. Model needs the constraint? It reads the same skill. When the rule changes, you edit one file, and both audiences
are correct at the same instant. There is no second copy left to drift.

The cost of skipping this isn't abstract. It's the new hire who follows the wiki and ships against a pattern you
abandoned. It's the model that generates code matching an instruction the humans quietly overruled in a meeting. Every
duplicate is a future contradiction with a delay timer on it.

And the skill format punishes duplication on purpose. It's compact, scoped, one rule per line — so when the same
instruction shows up in two files, you *see* it, and you can delete one. Prose hides its own redundancy. Signals expose
it.

Documentation was never really the goal. A team that agrees on how things work was the goal — and documentation was just
our lossy, drifting attempt to store that agreement outside our heads. Skill files are the first format that stores it
once and hands it to everyone, carbon and silicon alike.

So stop writing docs *about* your project. Write the one file your project — and everyone building it — actually reads.
