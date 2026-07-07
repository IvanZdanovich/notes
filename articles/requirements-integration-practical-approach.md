# Embed Requirements in Test Descriptions: A Practical Cypress Guide to Traceability Without Extra Tools

## Preface

My [earlier article, Integrating Requirements into the Codebase](integrating-requirements-into-codebase.md), proposed
a heavy approach: a parallel hierarchy of requirement files shadowing the test suite. In practice, that second tree
rots. Every time a test moves, someone has to remember to move its requirement file too, and no one ever does.

So I am proposing something simpler. Embed each requirement directly in the description of the test block that verifies
it. The requirement and its test live in the same string, in the same file, and travel together forever.

Here is the problem this solves. On a growing project, tests multiply and scenarios tangle. Requirements arrive
half-written, live in a wiki or a ticket no one reopens, and drift out of sync with the code. Ask "which test proves
requirement X?" and you get a shrug. Ask "what does this test actually guarantee?" and you get the test name, which is
usually a vague hint like `it('works')`. That gap between what the product must do and what the suite proves is where
regressions hide.

Think of a library where the catalog lives in one building and the books in another, and no one reconciles them. The
catalog says you own a book; the shelf says otherwise. Embedding requirements in test descriptions is gluing the
catalog card to the spine of the book. The two can never disagree again.

## Table of Contents

- [Purpose](#purpose)
- [Pros and Cons](#pros-and-cons)
- [Implementation](#implementation)
- [Index Convention](#index-convention)
- [Example](#example)
- [Integrating Requirements with Cypress](#integrating-requirements-with-cypress)
- [References](#references)

## Purpose

Embedding requirements inside test descriptions buys you five things:

- **Simpler management**: No separate requirements document to maintain, review, or forget.
- **Real traceability**: A direct, unbreakable link between each requirement and the test that proves it.
- **Honest coverage**: Gaps become visible because an untested requirement has no test block to hide behind.
- **Consistency**: One set of requirements, phrased one way, in one place.
- **Collaboration**: A single source of truth that developers, QA, and product all read from the same file.

## Pros and Cons

### Pros

- **No third-party dependencies**: No spend of time or money on external requirement- and test-management tools.
- **Improved traceability**: Direct links make impact analysis fast — change a requirement, and the failing test names
  it.
- **Centralized management**: Requirements live in the test descriptions, not a detached document.
- **Streamlined test suite**: Redundant or dead requirements are easy to spot and delete.
- **Facilitated collaboration**: The whole flow, from requirement to verification, sits in one artifact.
- **Efficient onboarding**: Reading the tests teaches newcomers what the product must actually do.
- **Improved requirement quality**: Requirements are written at a low level, so they come out clearer and more precise.
- **Improved test quality**: Tests describe an exact obligation, so they are more accurate.
- **Accurate metrics**: Any metric built on these tests inherits maximum precision, because each test maps to one
  requirement.
- **Cheap maintenance**: You can locate and manage any requirement and its related tests in seconds.

### Cons

- **Gradual benefits**: The payoff shows up during maintenance and updates, not on day one.
- **Initial learning curve**: The team needs time to internalize the convention.
- **Implementation discipline**: Like any precise system, it rewards care and punishes sloppiness.
- **Upfront investment**: Setup and rollout cost time and attention before the returns arrive.

## Implementation

Skip the parallel file structure entirely. Instead, embed the requirement index directly in the test block
description, using a format like `RequirementLocator: requirement itself`. Everything stays simple and consolidated in
one place.

## Index Convention

Agree on a clear, simple convention for indexing requirements before you write the first test. The convention has two
halves:

- **Locator**: Use keywords that pin down where the requirement lives — `Page` for UI tests, `Flow` for end-to-end
  scenarios — and add extra locators for specific UI components and submodules.
- **Requirement description**: Use keywords like `When` and `Should` (or any set you prefer) so the intent reads
  naturally and can be parsed by automated analysis.

The locator is the address; the `When`/`Should` phrasing is the sentence. Together they read like plain English and
still machine-parse. That dual property is the whole point: humans skim it, scripts grep it.

## Example

Here is how the requirement rides inside the test descriptions:

```javascript
    describe('DashboardPage', () => {
    context('DashboardPage.List: When user navigates to the page', () => {
        before(() => {
            // Do actions, prepare conditions
        });
        it('DashboardPage.List: Should show list of applications', () => {
            // Test the requirement
        });
        it('DashboardPage.List: Should show output sorted alphabetically', () => {
            // Test the requirement
        });
    });
}); 
```

The `DashboardPage.List` locator repeats down the block, so a single search surfaces every requirement for that
component. The `When`/`Should` split separates the condition from the expected outcome — exactly the shape a
requirement should have.

## Integrating Requirements with Cypress

To wire this into a Cypress suite, follow these steps in order:

1. **Define the naming convention first** — for requirements, for conditions, and for the locators that point to the
   relevant part of the application, so the related code is easy to find.
2. **Granulate your requirements** — ideally, one requirement maps to exactly one verification statement, so coverage
   stays measurable.
3. **Apply the convention everywhere** — put a description on every `describe`, `context`, and `it` block, and add
   empty `it` blocks for requirements you have not implemented yet, so the gaps are visible.
4. **Aggregate and audit** — group similar requirements, track coverage by requirement, and find gaps using the
   locators plus automated scripts.

Step 3 is the one people skip and regret. An empty `it` block is a promise you have not kept — it shows up as a pending
test, a to-do that the runner itself keeps reminding you about, rather than a requirement quietly missing from the
suite.

This method simplifies the process, tightens traceability, and improves the accuracy of your coverage — all without a
separate hierarchical structure of requirements to maintain. Glue the catalog card to the spine, and the catalog can
never lie to you again.

## References

- Original Article: [Integrating Requirements into the Codebase](integrating-requirements-into-codebase.md)
- Naming Conventions: [Stop Sabotaging Your Tests: The Crucial Role of Naming Conventions](naming-convention)
- Tagging Strategy: [Tagging Strategy](tagging-strategy.md)