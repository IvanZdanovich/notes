# Why Your Automated Tests Rot Without a Naming Convention

Open a test suite you haven't touched in six months. Can you tell what `it('Verify user is registered')` actually checks
without reading the body? If not, you don't have a test suite. You have a pile of guesses that happen to run.

The popular belief is that naming is cosmetic: get the assertions right and the labels are just decoration you can tidy
up later. That belief is wrong, and it quietly costs you every time someone reads, debugs, or extends the suite. A
test's name is the only part most people ever read. When it lies or says nothing, the reader has to open the code,
reconstruct the intent, and hope they guessed right. Multiply that by a few hundred tests and a few new hires, and "
later" never comes.

## Vague Names Are a Recipe With No Ingredients

Imagine baking a cake from a recipe that reads like this:

- Ingredient A: 2 cups
- Ingredient B: 1 cup
- Step 1: Mix A and B
- Step 2: Add C

You have no idea what you're making or whether you got it right. Now give the ingredients real names:

- Flour: 2 cups
- Sugar: 1 cup
- Butter: 1/2 cup
- Eggs: 2

Steps:

- Mix flour and sugar.
- Add butter and eggs.

The instructions didn't change. Only the labels did, and suddenly the recipe is followable.

Now picture the worst case: "Ingredient A" means flour in step 1 and sugar in step 3. That single inconsistency turns a
recipe into a trap. This is exactly what happens when one term drifts across a test suite, meaning different things in
different files. A convention exists to kill that ambiguity by binding each term to a single, clear definition.

Setting up these patterns feels tedious the first time. But the convention is the backbone of the suite, not a coat of
paint on top of it.

## Two Suites, Same Feature

Here is a registration flow written without a convention.

```javascript
// registration.spec.js
describe('Registration', () => {
    it('Corresponds to mock-up registration page', () => {
        // complete mess of actions, all you probably like
    });
    it('Verify user is registered', () => {
        // another "clear" check
    });
});
```

Nothing tells you what a passing test proves. "Corresponds to mock-up" could assert anything. "Verify user is
registered" bundles the whole flow into one opaque check. When it fails, the name gives you no starting point.

Here is the same feature under a convention.

```javascript
// registration-page.ui.spec.js
describe('RegistrationPage', () => {
    before(() => {
        // navigation, preconditions
    });
    context('RegistrationPage.RegistrationForm: When user opens the Registration Form', () => {
        before(() => {
            // navigation, preconditions
        });
        it('RegistrationPage.RegistrationForm: Should display email field with placeholder and label', () => {
            // clear atomic check
        });
        it('RegistrationPage.RegistrationForm: Should display password field with placeholder and label', () => {
            // clear atomic check
        });
    });
    context('RegistrationPage.RegistrationForm: When user fills Registration Form', () => {
        before(() => {
            // fill the form
        });
        it('RegistrationPage.RegistrationForm: Should display active Register button', () => {
            // clear atomic check
        });
    });
    context('RegistrationPage.RegistrationForm: When user clicks Register button', () => {
        before(() => {
            // click Register button
        });
        it('RegistrationPage.RegistrationForm: Should display Success Registration Notification', () => {
            // clear atomic check
        });
    });
});
```

Read any single line and you know the page, the component, the trigger, and the expected outcome. The filename encodes
the target and the layer (`registration-page.ui.spec.js`). The `context` blocks map to user actions. Each `it` asserts
one atomic thing. When a test fails, its name already tells you where to look before you open the file.

## The Harsh Truth

Let's be blunt: if your automated tests lack consistent naming conventions, you're setting yourself up for failure. Your
test suite becomes a labyrinth of confusion, making it nearly impossible to navigate or maintain. This isn't about
aesthetics. It's about functionality and efficiency.

## What a Convention Buys You

1. **Well-structured code.** A naming rule forces structure on you. File names, describe blocks, contexts, and test
   titles all line up, because the convention won't let them drift apart.
2. **Automatable tooling.** Predictable names let scripts do the grunt work — grouping suites, generating reports,
   flagging tests that break the pattern. Names that follow a grammar can be parsed; names that don't must be read by a
   human.
3. **Faster onboarding.** A new hire reads the test names and infers the map of the app without a walkthrough. The
   convention is the documentation, so nobody has to write or maintain a separate one.
4. **Fewer logical mistakes.** When a name states one intent clearly, you notice immediately if the assertion doesn't
   match it. Vague names hide the mismatch until production does.
5. **Meaningful failure output.** A failing run reads like a sentence: page, component, action, expected result. You
   triage from the report instead of bisecting the code.
6. **Real maintainability.** You change a test because you understand it at a glance, not after ten minutes of
   archaeology.
7. **Metrics that mean something.** Consistent names let you slice results by feature, layer, or flow. Track flakiness,
   spot bottlenecks, and decide from data instead of hunches.
8. **Less mental load.** You stop reinventing a naming scheme per test. The decision is already made, so your attention
   goes to the logic.

## Pick the Rule Before You Write the Next Test

The choice isn't whether to have a convention — every suite has one, either chosen on purpose or accreted by accident.
The accidental one is the labyrinth. The deliberate one is the map.

So go back to that six-month-old test you couldn't read. If its name had told you the page, the trigger, and the
expected result, you'd never have had to open it. That's the whole payoff: a name you can trust is a test you don't have
to reread. Define the pattern once, apply it to the next test you write, and stop making your future self guess.