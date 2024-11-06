# Integrating Requirements into the Codebase: A Practical Guide with Cypress

## Preface

In the original article, [Integrating Requirements into the Codebase](integrating-requirements-into-codebase.md), the
approach suggested was quite complex, involving a parallel hierarchy of requirement files. Here, I propose a simpler
method: embedding requirements directly within code block descriptions. This makes the relationship
between requirements and tests clear and streamlines test management.

Managing an increasing number of tests in software projects is a challenge, especially as scenarios
grow more complex. Often, requirements are incomplete or poorly described, leading to inefficiencies and unclear
outcomes. By embedding each requirement directly within descriptions of test code blocks, you can improve clarity and streamline
test management.

## Table of Contents

- [Purpose](#purpose)
- [Pros and Cons](#pros-and-cons)
- [Implementation](#implementation)
- [Index Convention](#index-convention)
- [Example](#example)
- [Integrating Requirements with Cypress](#integrating-requirements-with-cypress)
- [References](#references)

## Purpose

Embedding requirements within test descriptions helps:

- **Simplify Management**: Eliminate the need for separate documents for requirements.
- **Improve Traceability**: Establish direct links between requirements and tests.
- **Enhance Coverage Tracking**: Identify gaps and ensure thorough testing.
- **Ensure Consistency**: Maintain a unified set of requirements.
- **Facilitate Collaboration**: Provide a single source of truth.

## Pros and Cons

### Pros

- **No third party dependencies**: No need to spent time and money on third party tools for requirement and test management.
- **Improved Traceability**: Direct links and easier impact analysis.
- **Centralized Management**: Requirements are embedded in the test descriptions.
- **Streamlined Test Suite**: Easier identification and removal of redundant requirements.
- **Facilitated Collaboration**: The whole process from .
- **Efficient Onboarding**: Encourages deep understanding of the project.
- **Improved Requirement Quality**: Requirements are described on low level, and more clear and precise.
- **Improved Test Quality**: Tests are more accurate and clear.
- **Accurate Metrics**: All the metrics based on such tests have maximum precision.
- **Chip maintenance** You can easily locate and manage any requirements and related tests.

### Cons

- **Gradual Benefits**: Benefits accrue during maintenance and updating of requirements.
- **Initial Learning Curve**: Requires time to learn the new system.
- **Implementation Complexity**: As any accurate system requires careful and accurate attitude.
- **Resource Investment**: Upfront time and resources needed for setup and implementation.

## Implementation

Instead of a separate structure, embed the requirement index directly in the test block description using a format
like `RequirementLocator: requirement itself`. This keeps everything simple and consolidated.

## Naming Convention for Requirement's locators and descriptions

Create a clear and simple convention for indexing requirements:

- **Locator**: You could use key words like `Page` for UI tests or `Flow` for e2e scenarios, provide additional locators for UI components and submodules.
- **Requirement Description**: Define key words like `When` and `Should` or any other you want for more readability and automatic analyses of requirements

## Example

Here's how you can embed requirements directly in your test descriptions:

```javascript
    describe('DashboardPage', () => {
        context('DashboardPage.List: When user navigates to the page', () => {
            before(()=>{
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

## Integrating Requirements with Cypress

To integrate with Cypress:

1. **Define a clear naming convention for requirements and conditions and for locators to easely find the related part of application**.
2. **Granulate your requirements, ideally one requirement should have one verification statement**.
3. **Follow the naming conventions and provide descriptions in each `describe`, `context` and `it` block, provide empty `it` blocks for non-implemented tests**.
4. **Gather similar requirements, track coverage based on requirements and identify gaps using the requirement locators and automated scripts**.

_
This method simplifies the process, enhances traceability, and improves test coverage accuracy, all without needing a
separate hierarchical structure of requirements.

## References

- Original Article: [Integrating Requirements into the Codebase](integrating-requirements-into-codebase.md)
- Naming Conventions: [Stop Sabotaging Your Tests: The Crucial Role of Naming Conventions](naming-convention)
- Tagging Strategy: [Tagging Strategy](tagging-strategy.md)
