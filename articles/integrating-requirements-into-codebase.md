# Requirements as Code: Stop Losing Test Coverage in Confluence and Store Requirements Next to Cypress Tests

## The problem: your requirements and your tests live in different worlds

On most projects, requirements live in one place and the tests that prove them live in another. Requirements sit in
Confluence, Jira, a spreadsheet, or someone's memory. Tests sit in the repository. Nobody keeps the two in sync, because
keeping them in sync is nobody's explicit job.

As the end-to-end (E2E) suite grows, this gap widens. Scenarios multiply, steps overlap, and the suite starts to depend
on the individual tester's memory rather than a shared strategy. When a new engineer asks "what does this test actually
prove?", the honest answer is often "ask the person who wrote it." That person may have left.

The requirements themselves make it worse. They are frequently incomplete, misplaced, or vaguely worded. So before a
team can even manage its test suite, it has to stop and reconstruct what the requirements were supposed to say. That is
a cycle of rework: refine the requirements, adjust the tests, discover the requirements were wrong again, repeat.

The mainstream assumption is that requirements belong in a dedicated management platform, separate from code. This
article challenges that. The cost of the split is invisible until an audit or a coverage question forces someone to
manually cross-reference two systems that were never designed to agree.

The proposal is simple: store each atomic requirement alongside the test codebase, in the same repository, versioned
with the same commits. This makes the link between a requirement and the test that validates it explicit, removes the
dependency on external documentation, and lets coverage be computed from the code itself instead of estimated by hand.

## Table of Contents

- [Purpose](#purpose)
- [Pros and Cons](#pros-and-cons)
- [Structure](#structure)
- [Index Convention](#index-convention)
- [Creation](#creation)
- [Updating](#updating)
- [Deleting](#deleting)
- [Example JSON Structure](#example-json-structure)
- [Integrating Requirements with Cypress](#integrating-requirements-with-cypress)

## Purpose

Storing requirements inside the repository exists to do one thing: make test cases and test suites easier to manage by
keeping the requirement and its proof in the same place. Think of it like keeping a recipe taped inside the cupboard
where the ingredients live, instead of in a binder in another room. You stop walking back and forth, and you stop
guessing whether the binder is up to date.

Centralizing requirements with the test code lets the team:

- **Simplify management**: update and maintain requirements right next to the tests, in the same pull request.
- **Improve traceability**: link each requirement directly to the test cases that validate it, so responsibility is
  visible.
- **Enhance coverage tracking**: surface gaps and confirm every requirement is actually tested.
- **Ensure consistency**: keep one unified set of requirements for everyone, cutting down misunderstandings.
- **Facilitate collaboration**: give the team a single source of truth that is easy to update.

## Pros and Cons

### What you gain

- **Sharper coverage tracking**
    - Precise metrics: because tests link directly to atomic requirements, coverage stops being a guess. You can see
      exactly which requirements have no test behind them.
    - Automatic calculation: coverage is derived from the code, so you no longer need a separate tool or a third-party
      integration to report it.
- **Real traceability**
    - Direct linking: each test case points at a specific requirement, so anyone can answer "which test proves this?" in
      seconds.
    - Faster impact analysis: when a requirement changes, you can trace straight to the affected tests, which shrinks
      the risk of shipping an untested change.
- **One place for requirements**
    - Single source of truth: requirements sit beside the tests, consistent for every team member.
    - Platform independence: no dependency on Confluence or any external system to hold the requirements; they travel
      with the code.
- **A leaner suite**
    - Redundancy detection: outdated or duplicate requirements are easier to spot and remove, keeping the suite
      relevant.
    - Clear logging: test descriptions read the requirement directly, so there is no rephrasing or duplicated wording to
      maintain.
- **Smoother collaboration**
    - Team consistency: everyone works from the same requirements, so tests are developed and run against one shared
      understanding.
    - Updates in tandem: a requirement and its test change together in the same commit, which reduces drift between
      them.
- **Faster, deeper onboarding**
    - Newcomers have to understand the application's structure and behavior to place a requirement correctly, which
      builds real comprehension instead of surface familiarity.
- **Better requirements over time**
    - Integrating requirements with test cases forces regular review. Vague requirements get caught and refined the
      moment someone tries to test them.

### What it costs you

Be honest about this. The approach is not free, and the payoff is delayed.

- **Benefits arrive gradually**
    - This is incremental work. Precise coverage and streamlined management only fully materialize once the whole suite
      has been refactored to use the convention. Early on, you carry the cost without the full reward.
- **An upfront learning curve**
    - There is a real initial investment in learning the conventions and structure. Expect productivity to dip during
      onboarding before it recovers.
- **More to manage**
    - Management overhead: you now maintain both requirements and tests, and you have to keep them correctly linked.
    - Risk of duplication: without discipline, requirements can overlap or duplicate, which breeds confusion rather than
      clarity.
- **Risk of drift**
    - Mismatch risk: if someone updates a requirement without touching its test (or vice versa), validation quietly
      becomes wrong. The system is only as trustworthy as the discipline behind it.
    - Version control friction: keeping requirements in step with fast-changing tests and application code is genuinely
      hard on large or rapidly moving projects.
- **A concrete setup bill**
    - Setup time: creating requirement files, indexing them, and wiring them into test cases takes meaningful upfront
      effort.
    - Resource allocation: that effort competes with other priorities and can pull attention from delivery deadlines.
- **Adaptation as the project grows**
    - Scalability: as the project evolves, the initial structure and conventions may need reworking, and that rework is
      not always cheap.

The honest summary: this pays off when the suite is large enough that manual coverage tracking already hurts, and when
the team is disciplined enough to update requirements and tests together. On a small, short-lived project, the overhead
may not earn its keep.

## Structure

Mirror the requirements folder to the test folder. If the tests are split into `api` and `ui`, the requirements should
be too. When the two trees look alike, finding the requirement behind a test is navigation, not detective work.

```  
requirements/  
├── api/  
├── ui/  
│ ├── req-common.json  
│ ├── req-action.json  
│ ├── req-audit-type.json  
│ └── req-audit-round.json  
```  

## Index Convention

The index is what makes a specific requirement findable, and it is what will later allow automated verification of the
indexes themselves. Treat the index like a postal address: each segment narrows the location, and the format is
predictable enough that a machine can validate it. Define every part in a convention. Here is an example convention and
index template:

- **Prefix**: distinguish UI from API requirements.
- UI: `UI-`
- API: `API-`
- **Section Code**: a unique code per section.
- Common: `COMMON-`
- Dashboard: `DASH-`
- Settings: `SET-`
- Notifications: `NOT-`
- **Sub-Component Code**: a sub-component code where applicable.
- **Requirement Number**: a specific number in hierarchical format, e.g., `1-1`.

## Creation

The indexing system should make specific requirements easy to find, and every part of the index must follow the
convention so that automatic index verification becomes possible later. Below is an example of the convention and an
index template in use:

### Example

- **Common Requirements Section**
- `COMMON-BUTTON-1-1`: All buttons must have a consistent style.
- `COMMON-ERROR-2-1`: Error messages should be displayed in red.
- **Subcomponent References**
- `UI-DASH-FILT-1-1`: Filter buttons must adhere to `COMMON-BUTTON-1-1`.
- `UI-DASH-FILT-1-2`: Filter error messages must adhere to `COMMON-ERROR-1-1`.

Notice how the subcomponent requirements reference the common ones by index. A shared rule like button styling is
written once and pointed to, rather than copied into every component. That is the same instinct as a function you call
in many places instead of pasting its body everywhere.

### Instructions for Creation

1. Identify the requirement and decide whether it is common or specific to a subcomponent.
2. If common, add it to the `req-common.json` file with a unique ID and description.
3. If specific, add it to the appropriate JSON file under the relevant section and sub-component.
4. Use the index convention to assign it a unique identifier.

## Updating

### Instructions for Updating

1. Locate the requirement in the appropriate JSON file.
2. Update the description or details as needed.
3. If the change touches a common requirement, make sure every reference to it stays consistent.

The third step is the one that bites. A common requirement is referenced by many others, so editing it without checking
its references is how silent misalignment creeps in.

## Deleting

### Instructions for Deleting

Delete only outdated or obsolete requirements. A requirement not yet covered by tests is a coverage gap to close, not
dead weight to remove, so it must **not** be deleted.

1. Locate the requirement in the appropriate JSON file.
2. Remove the requirement entry.
3. If it is a common requirement referenced by others, update those references accordingly.

## Example JSON Structure

Here is what the JSON files look like in practice. The files are plain key-value maps: the index is the key, the
requirement text is the value.

### `req-common.json`

```JSON  
{
  "COMMON-BUTTON-1-1": "All buttons must have a consistent style.",
  "COMMON-ERROR-1-1": "Error messages should be displayed in red."
}  
```  

### `req-action.json`

```JSON  
{
  "UI-ACT-1-1": "Admin-specific action must be logged.",
  "UI-ACT-1-2": "Action buttons must adhere to COMMON-BUTTON-1-1.",
  "UI-ACT-1-3": "Action error messages must adhere to COMMON-ERROR-1-1."
}  
```  

## Integrating Requirements with Cypress

With the requirements stored and indexed, the payoff comes from wiring them into the test framework so coverage falls
out of the code automatically. The integration has four steps:

1. **Load requirements** from the JSON files into the Cypress tests.
2. **Tag tests** with the corresponding requirement IDs.
3. **Validate requirements**, so each test actually proves the requirement it claims.
4. **Analyze results**, running a script that extracts requirement IDs and reports coverage.

### 1. Load the requirements

Create a utility function to load requirements from the JSON files.

```JavaScript
// cypress/support/requirements.js  
const fs = require('fs');
const path = require('path');

function loadRequirements(filePath) {
    const fullPath = path.resolve(__dirname, filePath);
    const rawData = fs.readFileSync(fullPath);
    return JSON.parse(rawData);
}

module.exports = {
    loadRequirements
};
```

### 2. Format descriptions from requirement IDs

Add a helper that turns a requirement ID into a readable test description. This is the mechanism that puts the
requirement text directly into the test name, so the description never has to be hand-written or kept in sync
separately.

```JavaScript
//cypress/support/descriptionFormatter.js  
function formatDescription(requirementId, requirements) {
    return `should validate ${requirementId}: ${requirements[requirementId]}`;
}

module.exports = {
    formatDescription
};
```  

### 3. Tag the tests

Tag each Cypress test with its requirement ID through the formatter. The test title now carries both the ID and the
requirement text, which is exactly what the analysis step will read back out.

```JavaScript  
// cypress/integration/action_spec.js  
const {loadRequirements} = require('../support/requirements');
const {formatDescription} = require('../support/descriptionFormatter');
const actionRequirements = loadRequirements('../requirements/ui/req-action.json');

describe('Action Tests', () => {
    it(formatDescription('UI-ACT-1-1', actionRequirements), () => {
        // Test implementation  
        cy.get('button').should('have.class', 'consistent-style');
    });

    it(formatDescription('UI-ACT-1-2', actionRequirements), () => {
        // Test implementation  
        cy.get('.error-message').should('have.css', 'color', 'red');
    });
});  
```

### 4. Analyze results and compute coverage

Because every test title embeds its requirement ID, coverage becomes a text-matching problem, not a bookkeeping one. The
script below scans the test results, extracts the IDs, and sorts every requirement into three buckets: `coverage` (
proved by at least one test), `uncovered` (the gaps), and `redundant` (proved by more than one test). This is the
reporting that would otherwise require an external tool.

```JavaScript
// analyzeRequirements.js
const fs = require('fs');
const path = require('path');

function extractRequirementIds(testResults) {
    const requirementIdPattern = /UI-[A-Z-]+\d{1,3}\-\d{1,3}/g;
    const requirementIds = new Set();

    testResults.forEach(test => {
        const matches = test.description.match(requirementIdPattern);
        if (matches) {
            matches.forEach(id => requirementIds.add(id));
        }
    });

    return Array.from(requirementIds);
}

function analyzeRequirements(requirementsFilePath, testResults) {
    const requirements = JSON.parse(fs.readFileSync(requirementsFilePath, 'utf-8'));
    const requirementIds = Object.keys(requirements);
    const usedRequirementIds = extractRequirementIds(testResults);

    const usageCount = {};
    requirementIds.forEach(id => {
        usageCount[id] = usedRequirementIds.filter(usedId => usedId === id).length;
    });

    const coverage = requirementIds.filter(id => usageCount[id] > 0);
    const uncovered = requirementIds.filter(id => usageCount[id] === 0);
    const redundant = requirementIds.filter(id => usageCount[id] > 1);

    return {
        coverage,
        uncovered,
        redundant
    };
}

// Example test results (replace with actual test results)
const testResults = [
    {description: 'should validate UI-ACT-1-1: Admin-specific action must be logged.'},
    {description: 'should validate UI-ACT-1-2: Action buttons must adhere to COMMON-BUTTON-1-1.'},
    {description: 'should validate UI-ACT-1-1: Admin-specific action must be logged.'}
];

const requirementsFilePath = path.resolve(__dirname, '../requirements/ui/req-action.json');
const analysis = analyzeRequirements(requirementsFilePath, testResults);

console.log('Coverage:', analysis.coverage);
console.log('Uncovered:', analysis.uncovered);
console.log('Redundant:', analysis.redundant);
```

## The takeaway

The requirements and the tests started in different worlds, and every audit, coverage question, and onboarding session
paid the tax on that split. Bringing them into the same repository, linked by a disciplined index and read straight out
of test titles, turns coverage from a manual guess into a number the code hands you. Start with one section of the
suite, adopt the index convention, and let the analysis script tell you where the gaps are. The first honest coverage
report you get back is the moment the walking-between-rooms stops.