# cypress-start v2: Fewer Artifacts, Better Automation

The framework I open-sourced last year just got a major version bump.

I've seen automation projects collect so many layers that maintaining the automation becomes harder than
maintaining the product itself.

A requirement lives in one tool.

A manual test case repeats it in another.

A traceability matrix maps the two.

An automated test implements a third version.

Then a report tries to prove they are still connected.

That is not quality engineering. That is bookkeeping with extra steps.

cypress-start v2 takes a different path.

It deliberately avoids things many teams treat as defaults:

- **Test management integrations** — automated tests should not justify their existence by mirroring manual test cases
  and reporting coverage numbers. Manual test cases are for humans. Automation should verify requirements directly.
- **Requirements management tool integrations** — most teams either do not have a dedicated requirements tool or
  struggle to keep it synchronized. Every integration adds another dependency, another failure point, and another place where the truth can drift.
- **Tag-driven execution strategies** — tags are useful, but they often become a workaround for bloated suites. If you
  constantly exclude tests to keep execution time manageable, the underlying model probably needs attention.
- **BDD frameworks and glue code** — Given/When/Then is valuable. Maintaining another abstraction layer around it often
  is not. In cypress-start v2, named executable test titles provide readability without the ceremony.
- **Page Object Model by default** — rebuilding the application's UI structure in code often duplicates the product
  instead of describing the behavior. cypress-start favors requirement-focused automation.
- **Custom logging frameworks** — atomic, well-named checks already tell the story. Cypress gives the low-level
  execution details when you need them.

Removing things was not the goal.

cypress-start v2 is built around one idea:

The requirement should be the source of truth.

Not the requirement document.

Not the test case.

Not the traceability matrix.

Not the automation script.

The requirement.

To support that, cypress-start v2 introduces a traceability model with three layers:

- **constraints** — boundary values and rules;
- **examples** — named data instances that describe behavior and capture edge cases;
- **specs** — executable requirements written as ordered, atomic Given/When/Then statements.

Instead of maintaining five artifacts that describe the same behavior, keep the knowledge in one place and derive the
rest from it.

The tooling reinforces that model:

- a linter validates structure and naming conventions;
- Claude Code skills help generate, refactor, and maintain specs, examples, and constraints;
- coverage analysis identifies missing requirement paths and can enforce thresholds in CI;
- flaky test tracking keeps a historical ledger and classifies tests as consistent, flaky, or rare failures over time.

The biggest change in cypress-start v2 is not technical.

It is philosophical.

For decades, we have accepted a chain of requirements, manual test cases, traceability matrices, management tools, and
automation scripts, all describing the same behavior in different formats.

The framework shows there is a more efficient alternative.

Describe the requirement once.

Keep it executable.

Let everything else be derived from it.

The spec is the requirement.

GitHub: https://github.com/IvanZdanovich/cypress-start

#SpecificationByExample #GojkoAdzic #Cypress #Cypress.io #TestAutomation #SDET #QualityEngineering #SoftwareTesting #AutomationFramework
#ShiftLeft 