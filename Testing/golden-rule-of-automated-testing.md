# The Golden Rule of Automated Testing: Are You Violating It?

You might be familiar with the fundamental rule of automated testing, yet you might be breaking it without even
realizing it! While the current title might not be the most clickbait-worthy, it certainly highlights a critical issue
in automation. Let's delve into the significant consequences of ignoring this rule and how it impacts your team's
efficiency and productivity.

## The Benefits of Adhering to This Rule

Following this rule brings numerous benefits, from the least to the most valuable:

1. **Precision of Metrics**: Metrics derived from such tests are inherently precise, and this will become clearer with
   the following points.
2. **Consistency**: By following this rule, you can easily identify any deviations, ensuring uniformity across your
   tests.
3. **Clarity**: Adhering to this rule eliminates the need for additional abstract layers, such as those used in BDD (
   Behavior-Driven Development) for non-technical personnel, thereby enhancing visibility and transparency.
4. **Maintainability**: This point needs no further explanation since the resulting code is simple and doesn't need
   additional clarifications or comments; it will be evident as we proceed.
5. **Reliability**: This principle allows you to quickly identify and discard faulty tests.
6. **Performance**: Tests created in accordance with this rule are significantly faster.

## The Rule Revealed

So, what is this rule? Here it is in 3... 2... 1... Your tests should be small, atomic, and focused. The rule can be
summarized as: **one check per test**.

Just as massive buildings are constructed from small bricks, you can build a robust testing solution by following this
rule. You might argue that real wonders, like the pyramids of Giza, were built with huge blocks. However, I don't aspire
to be a hero in everyday office work, lugging around massive blocks. There's a significant risk of ending up with a
Stonehenge rather than a Giza pyramid.

Each brick is small, standardized, and relatively easy to handle. Interestingly, a brick is stronger than a large block
made of the same material, thanks to the law of squares. You can create various beautiful forms using prefabricated
small pieces. Conversely, if you make a mistake with a huge stone, you'll need to start over from the quarry.

Consider the Woolworth Building, the last skyscraper built of bricks. It was constructed in just three years and stands
241 meters tall, compared to the Great Pyramid of Giza, which took approximately 27 years to build and is 147 meters
tall.

---

## Inspired by Cypress

Yes, I was inspired by the Cypress article
on [Creating Tiny Tests With A Single Assertion](https://docs.cypress.io/guides/references/best-practices#Creating-Tiny-Tests-With-A-Single-Assertion).
I agree with the examples provided there, emphasizing that we should not decompose the verification of different
properties of the same element. Instead, I'm encouraging you to combine checks for one element or even closely related
ones. The main point is to avoid writing tests like an unstructured, lengthy poem. Instead, decompose and describe the
milestones of the tests.

---

## Example Code

Here is an adapted example of code:

```javascript
describe('TestingFlow: Test form', () => {
    context('TestingFlow: When manager creates the Test form', () => {
        before(() => {
            // steps for creation 'Test form'
        });
        it('TestingFlow: Then manager can see created form on the Dashboard', () => {
            cy.get('formsPage.formItem').should('have.text', johnny');
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

---

## Conclusion

In summary, adhering to the golden rule of automated testing—keeping your tests small, atomic, and focused—brings
numerous benefits. It enhances the precision of metrics, ensures consistency, improves clarity, boosts maintainability,
increases reliability, and optimizes performance. By following this rule, you can build a robust and efficient testing
framework that is easy to manage and scale. Remember, just like constructing a skyscraper with small bricks, building
your tests with single, focused checks will lead to a stronger and more reliable testing solution.
