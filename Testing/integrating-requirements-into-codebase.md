# Integrating Requirements into the Codebase: Pros, Cons, and a Practical Guide with Cypress

## Preface

Managing the increasing number of end-to-end (E2E) tests in software projects can become overwhelming, especially as
scenarios grow more complex and overlap in various steps.
This complexity often results in a reliance on the individual tester’s experience rather than a cohesive strategy.
Additionally, requirements, which form the foundation of testing, are frequently incomplete, misplaced, or inadequately
described.
Consequently, before effectively managing the test suite, teams often need to revisit and refine the requirements,
leading to a cycle of inefficiencies and unclear outcomes.

To address these challenges, we propose an enhancement to the workflow: storing each atomic requirement alongside the
test codebase during test implementation. This adjustment not only clarifies the relationship between requirements and
tests but also streamlines several aspects of test management, reducing dependency on external documentation and
improving accuracy in test coverage calculations.

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

The purpose of storing requirements within the repository is to simplify the management of test cases and test suites.
This approach enhances the traceability of requirements, ensures accurate test coverage, and fosters a consistent and
collaborative environment for the team. By centralizing requirements in the same repository as the test code, we can:

- Simplify Management: Streamline the updating and maintenance of requirements by keeping them close to the tests.
- Improve Traceability: Establish direct links between requirements and their corresponding test cases, enhancing
  visibility and accountability.
- Enhance Coverage Tracking: Identify gaps and ensure all requirements are thoroughly tested, leading to more
  comprehensive coverage.
- Ensure Consistency: Maintain a single, unified set of requirements for all team members, reducing misunderstandings
  and discrepancies.
- Facilitate Collaboration: Provide a single source of truth for requirements, improving team collaboration and making
  updates easier.

## Pros and Cons

### Pros of Integrating Requirements with Codebase
- Enhanced Test Coverage Tracking 
  - Precise Metrics: By linking tests directly to atomic requirements, coverage metrics become more accurate, allowing teams to identify gaps and ensure comprehensive testing.
  - Automatic Calculations: Eliminates the need for external tools to calculate test coverage, streamlining the process and reducing dependency on third-party integrations.
- Improved Traceability
  - Direct Linking: Test cases are directly linked to specific requirements, enhancing traceability and making it easier to identify which tests validate which requirements.
  - Easier Impact Analysis: Changes in requirements can be traced back to affected tests quickly, aiding in impact analysis and reducing the risk of untested changes.
- Centralized Requirement Management
  - Single Source of Truth: Requirements are stored alongside the tests, providing a centralized and consistent source of information for all team members.
  - Platform Independence: Removes dependency on external platforms (like Confluence) for requirement storage, making them readily accessible within the codebase.
- Streamlined Test Suite Management
  - Redundancy Detection: Easier identification and removal of redundant or outdated requirements, leading to a more streamlined and relevant test suite.
  - Clear Logging: Direct access to requirements in test descriptions reduces duplication and rephrasing, simplifying test logging and documentation.
- Facilitated Collaboration
  - Team Consistency: Ensures all team members work with the same set of requirements, promoting consistency in test development and execution.
  - Simplified Updates: Requirements and test cases can be updated in tandem, reducing the likelihood of discrepancies and easing collaboration across roles.
- Efficient Onboarding and Learning
  - Encourages Deep Understanding: Forces team members to understand the application structure and functionality, enhancing their overall comprehension and engagement with the project.
- Improved Requirement Quality
  - Iterative Refinement: Continuous integration of requirements with test cases encourages regular review and refinement, leading to higher-quality requirements over time.

### Cons of Integrating Requirements with Cypress
- Graduated Implementation and Delayed Benefits
  - Incremental Adaptation: Integrating requirements into existing test cases is a gradual process. The benefits of precise coverage and streamlined management will only be fully realized after refactoring the entire test suite.
- Initial Overhead and Learning Curve
  - Steep Learning Curve: Significant initial investment in learning the new conventions and structures, which can slow down productivity during the onboarding process.
- Increased Complexity in Test Management
  - Management Overhead: Adds complexity to the testing process by requiring ongoing management of both requirements and test cases, including keeping them up-to-date and correctly linked.
  - Risk of Duplication: Potential for duplication or overlapping of requirements if not managed carefully, leading to confusion and inefficiencies in test execution.
- Potential for Misalignment
  - Mismatch Risks: There's a risk of misalignment between requirements and test cases, especially if requirements are updated without corresponding changes in the tests. This can reduce the accuracy of test validation.
  - Version Control Challenges: Synchronizing requirements with evolving test cases and application changes can be difficult, particularly in large or rapidly changing projects.
- Initial Resource Investment
  - Setup Time: Requires a substantial upfront investment in time and resources to create requirement files, index them, and integrate them with test cases.
  - Resource Allocation: Allocating resources to implement and maintain this approach may divert attention from other critical tasks, impacting project timelines and priorities.
- Adaptation to Project Dynamics
  - Scalability Concerns: As the project scales or evolves, the initial structure and conventions may need significant adjustments, potentially leading to rework and adaptation challenges.

## Structure

The structure of requirements should reflect the structure of the tests. For instance:

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

Indexes should simplify the search of particular requirement. Each part of index should be defined in convention. In future it will allow to implement automatic verification of indeces. Here is an example of convention, and index template:

- **Prefix**: Use a prefix to distinguish between UI and API requirements.
- UI: `UI-`
- API: `API-`
- **Section Code**: Assign a unique code for each section.
- Common: `COMMON-`
- Dashboard: `DASH-`
- Settings: `SET-`
- Notifications: `NOT-`
- **Sub-Component Code**: Use a sub-component code if applicable.
- **Requirement Number**: Use a specific requirement number in a hierarchical format, e.g., `1-1`.

## Creation

The indexing system should streamline the search for specific requirements. Each part of the index must adhere to a defined convention, facilitating future implementation of automatic index verification. Below is an example of the convention and an index template:

### Example

- **Common Requirements Section**
- `COMMON-BUTTON-1-1`: All buttons must have a consistent style.
- `COMMON-ERROR-2-1`: Error messages should be displayed in red.
- **Subcomponent References**
- `UI-DASH-FILT-1-1`: Filter buttons must adhere to `COMMON-BUTTON-1-1`.
- `UI-DASH-FILT-1-2`: Filter error messages must adhere to `COMMON-ERROR-1-1`.

### Instructions for Creation

1. Identify the requirement and determine if it is common or specific to a subcomponent.
2. If common, add it to the `req-common.json` file with a unique ID and description.
3. If specific, add it to the appropriate JSON file under the relevant section and sub-component.
4. Use the index convention to assign a unique identifier to the requirement.

## Updating

### Instructions for Updating

1. Locate the requirement in the appropriate JSON file.
2. Update the description or details as needed.
3. If the update affects common requirements, ensure all references are consistent with the changes.

## Deleting

### Instructions for Deleting

Delete only outdated or obsolete requirements. Requirements not yet covered by tests should NOT be deleted.

1. Locate the requirement in the appropriate JSON file.
2. Remove the requirement entry.
3. If the requirement is common and referenced by other requirements, update those references accordingly.

## Example JSON Structure

Here's an example of how the JSON files might look:

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

To integrate requirements into the Cypress testing framework, follow these steps:

Load Requirements: Load requirements from JSON files into Cypress tests.
Tag Tests: Tag your Cypress tests with the corresponding requirement IDs.
Validate Requirements: Ensure each test validates the corresponding requirement.
Analyze Test Results: Create a script to analyze test results and extract requirement IDs.

1. **Load Requirements**
   Create a utility function to load requirements from JSON files.

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

2. **Use the Helper Function**
   Create a helper function to format test descriptions using requirement IDs.

```JavaScript
//cypress/support/descriptionFormatter.js  
function formatDescription(requirementId, requirements) {
    return `should validate ${requirementId}: ${requirements[requirementId]}`;
}

module.exports = {
    formatDescription
};
```  

3. **Tag Tests**
   Tag your Cypress tests with the corresponding requirement IDs.

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

3. **Extract Requirement IDs from Test Descriptions**

4. **Analyze Test Results**
   Create a script to analyze the test results and extract the requirement IDs from the test descriptions.

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
