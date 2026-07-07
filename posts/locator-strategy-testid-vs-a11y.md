# Locator Choice: You Don't Eliminate Fragility. You Just Move It Somewhere Else.

Everyone sells accessibility locators as the fragility cure. `getByRole('button', { name: 'Submit' })` survives DOM refactors, reflects what users actually see, and nudges the team toward better accessibility. All true. And it changes nothing about my point.

Because there's one question that settles the whole debate:

> **Who owns this locator?**

`getByRole('button', { name: 'Submit' })` breaks when a PM renames the button to "Confirm." When a new locale turns "Submit" into "Enviar." When a designer drops `aria-hidden` on a live element. None of those people own your tests. None of them will warn you before they break them. You've outsourced the stability of your suite to strangers.

`data-testid="submit-btn"` breaks when *you* change the testId. That's it. You own the thing that breaks it.

Run the rule and the choice is obvious: **target with a testId, written as a plain CSS selector.**

```js
const reportingPage = {
    submitReport: '[data-testid="submit-btn"]'
}

cy.get(reportingPage.submitReport).click()
```

Ownership is the headline. Four more perks come for free, all concrete.

**It's readable.** The selector lives in a named variable — `reportingPage.submitReport`. Anyone who reads English knows what it targets before they ever see the DOM. The test reads like a sentence: get the submit report, click it. No decoding `div > span:nth-child(3)`.

**It's debuggable.** The selector is a plain string, not a framework abstraction. When something breaks, paste it straight into DevTools. Element highlights — the locator is fine, look at your logic. Nothing highlights — the locator is the problem. Ten seconds, not ten minutes.

**It's traceable.** A testId is grep-able. Rename or delete a component and you find every test that touches it instantly. Try that with `getByRole` calls scattered across the codebase.

**It's fast.** `getByRole` does work CSS never touches: it resolves the ARIA role and runs the full accessible-name algorithm on every candidate. `cy.get('[data-testid]')` is a near-native lookup. Microseconds each — until you multiply them across a big suite, retry loops that re-query on every attempt, and CI running everything in parallel.

No locator is refactor-proof. So stop chasing the one that is. Pick the locator whose fragility you control.