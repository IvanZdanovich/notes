# One Check Per Test: The Golden Rule Your Suite Is Quietly Breaking

You already know the rule. You'd nod along if someone said it out loud in a stand-up. And yet your test suite almost
certainly violates it right now, in dozens of places, and it's costing you more than you think.

Here's the belief worth challenging: that a "thorough" test is one that does a lot. Log in, create a record, edit it,
verify five fields, check a date format, assert a pass rate, all inside a single `it` block. It feels efficient. Fewer
tests, more coverage, less boilerplate. It's the wrong instinct, and it silently erodes every signal your suite is
supposed to give you.

The cost isn't abstract. When a 200-line test fails on line 180, you don't get a diagnosis, you get a crime scene. You
spend twenty minutes reconstructing which of the fifteen things it did actually broke. Multiply that by every flaky run,
every reviewer squinting at a monolithic test, every metric that reports "1 test failed" when really one of nine
independent checks failed. That's the tax you pay for ignoring the rule.

## The Rule, Stated Plainly

Your tests should be small, atomic, and focused. Summarized: **one check per test.**

Not one *assertion* dogmatically split across a dozen anemic tests, that's the opposite mistake, and I'll come back to
it. One *check*: one coherent thing you're verifying, expressed as a single test that can pass or fail on its own terms.

## Why Bricks Beat Boulders

Think about how large buildings actually get built: from small, standardized bricks, not monolithic slabs.

You might point to the pyramids of Giza, built from enormous blocks, as proof that big works. But I have no ambition to
be the office hero hauling a massive block across the floor. Try to build that way and you're far more likely to end up
with a Stonehenge, a few impressive stones and a lot of gaps, than a Giza pyramid.

A brick is small, standardized, and easy to handle. It's also, thanks to the law of squares, *stronger* than a single
large block of the same material. Small prefabricated pieces let you build almost any shape you want. Make a mistake
with one brick and you replace one brick. Make a mistake carving one huge stone and you're heading back to the quarry to
start over.

The proof is in the skyline. The Woolworth Building, the last great skyscraper built of brick, went up in **three years
** and stands **241 meters** tall. The Great Pyramid of Giza took roughly **27 years** to build and reaches **147 meters
**. Smaller units, built faster, standing taller. Your tests work the same way.

## What You Get When You Follow It

Follow the rule and the benefits compound, from useful to genuinely transformative:

1. **Precise metrics.** When each test verifies exactly one thing, a failure count means what it says. "Three tests
   failed" describes three real problems, not one messy test that tripped over itself.
2. **Consistency.** Uniform, single-purpose tests make deviations obvious. Anything that looks different *is* different,
   and worth a second look.
3. **Clarity.** Focused tests read as their own documentation. You don't need an extra abstraction layer, the kind BDD
   bolts on for non-technical readers, to explain what a test does when the test does one legible thing.
4. **Maintainability.** The code stays simple enough that it needs no explanatory comments. A focused test is
   self-evident; there's nothing to clarify because there's nothing tangled to begin with.
5. **Reliability.** A flaky test that checks one thing is trivial to identify and quarantine. A flaky test that checks
   nine things poisons all nine.
6. **Performance.** Small, focused tests run significantly faster, and they parallelize cleanly because none of them is
   secretly doing the work of ten.

## The Mistake in the Other Direction

Here's where people overcorrect. They hear "one check per test," take it literally, and shatter a single element's
verification into a cloud of one-line tests, each asserting a different property of the *same* thing.

That's not atomicity, it's fragmentation. Don't decompose the verification of different properties of the same element
into separate tests. Combine checks for one element, or even for closely related ones, into a single focused test.

I was inspired by the Cypress article
on [Creating Tiny Tests With A Single Assertion](https://docs.cypress.io/guides/references/best-practices#Creating-Tiny-Tests-With-A-Single-Assertion),
and I agree with the examples there. But the real point isn't "one assertion, always." It's this: don't write a test
like an unstructured, lengthy poem that rambles from setup to teardown in one breath. Decompose your suite along its
*milestones*, the meaningful checkpoints of a flow, and let each test own one of them.

## What This Looks Like in Code

Here's an adapted example. Notice how each `context` marks a milestone in the flow, and each `it` owns exactly one
focused check:

```javascript
describe('TestingFlow: Test form', () => {
    context('TestingFlow: When manager creates the Test form', () => {
        before(() => {
            // steps for creation 'Test form'
        });
        it('TestingFlow: Then manager can see created form on the Dashboard', () => {
            cy.get('formsPage.formItem').should('have.text', johnny
            ');
        });
        it('TestingFlow: Then manager can see title of "Test form" on the Dashboard', () => {
            cy.get('formsPage.formTitle').should('have.text', 'Test form title').and('be.visible');
        });
        it('TestingFlow: Then manager can see creation date and time of "Test form" on the Dashboard', () => {
            cy.get('formsPage.formdate').should('match', 'dateRegex');
        });
    });
    context('TestingFlow: When employee fills the form', () => {
        before(() => {
            // steps for filling the 'Test form'
            // steps for navigation
        });
        it('TestingFlow: Then manager can see results for "Test form"', () => {
            cy.get('formResultPage.passRate').should('match', 'passRateRegex');
        });
    });
});
```

The title check uses two chained assertions, `have.text` and `be.visible`, and that's correct: they verify one element.
But the title, the creation date, and the pass rate each get their own test, because each is a distinct milestone. When
the date format breaks, you know instantly, without the title check or the pass-rate check clouding the report.

## Start With Your Longest Test

You already knew the rule. The question was never whether you agree with it, it's whether your suite obeys it. So go
find the answer. Open your longest, most heroic test, the one that logs in and does eight things before it asserts
anything, and count the independent checks buried inside it. Every one of those is a brick you welded into a boulder.

Pull it apart. Build your suite from small, focused, single-purpose tests, and you'll get the same result the
brickmakers did: something you can raise faster, maintain more easily, and that stands taller and stronger than the
monolith you were tempted to carve.