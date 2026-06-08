## Locator choice: You don't eliminate fragility. You just move it somewhere else.

Claim: accessibility locators `getByRole('button', { name: 'Submit' })` survives some DOM refactors, reflects what users
actually see, and nudges the team toward better accessibility. All true.

So here's a decision rule you can use instead — one question that settles it:

> **Who owns this locator?**

`getByRole('button', { name: 'Submit' })` breaks when a PM renames the button to "Confirm." When the app ships a new
locale and "Submit" becomes "Enviar." When a designer sets `aria-hidden` on a live element. None of those people own
your tests. None of them will tell you before they break them. You've outsourced the stability of your suite.

`data-testid="submit-btn"` breaks when *you* change the testId. That's it. You own the thing that breaks it.

Run the rule and the choice is obvious: **target with a testId, written as a plain CSS selector.**

```js
const reportingPage = {
    submitReport: '[data-testid="submit-btn"]'
}

cy.get(reportingPage.submitReport).click()
```

Ownership isn't the only thing this buys you. Four more, all concrete:

**It's readable.** The selector lives in a named variable — `reportingPage.submitReport`. Anyone who reads English
knows what it targets before they ever see the DOM. The test reads like a sentence: get the submit report, click it.
No decoding `div > span:nth-child(3)`, no guessing what role a widget exposes.

**It's debuggable.** The selector is a plain string, not a framework abstraction. When something breaks, paste it
straight into DevTools. Element highlights — locator is fine, look at the logic. Nothing highlights — locator is the
problem. Ten seconds, not ten minutes.

**It's traceable.** A testId is grep-able. Rename or delete a component and you can find every test that touches it
instantly. Try that with `getByRole` calls scattered across the codebase.

**It's fast.** `getByRole` does extra work CSS never touches: during the query itself it resolves the ARIA role and
runs the full accessible-name algorithm on every candidate. `cy.get('[data-testid]')` is a near-native lookup. You pay
that a11y overhead on every query. Microseconds each. Until you multiply it across a big suite, retry loops that
re-query on every attempt, and CI running everything in parallel.

Pick the locator whose fragility you control — and stop expecting any of them to be refactor-proof.
