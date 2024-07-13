# Test Tagging Strategy

## Overview

Organizing and executing tests efficiently is crucial for maintaining a robust and scalable test suite. Tagging tests
helps streamline test runs for different purposes, such as integration, regression, smoke testing, and more. Let's dive
into a strategy for using tags effectively.

## Silly Tags Alert

Before we go, a quick note on tags like `@smoke` and `@regression`. While they sound fancy, they often add little value
beyond making your test suite sound like a bad detective novel. Instead, focus on meaningful tags that directly relate
to the functionality and purpose of your tests.

## Integration Tests and Full Regression

Integration tests and full regression tests will be organized based on the structure of the folders. Tests will be
grouped and run according to the specific modules or components they are designed to test.

## Smoke Test Run

The smoke test run will consist of:

- **End-to-End (E2E) Tests**: Cover the entire application workflow from start to finish, ensuring all critical paths
  function as expected.
- **Core Tests**: Focus on essential functionalities, verifying that core features work correctly.

## Tags

### 1. **@e2e**

- **Description**: End-to-end tests covering complete user workflows.
- **Usage**: Validate entire user journeys.

### 2. **@negative**

- **Description**: Tests validating behavior with invalid inputs or unexpected conditions.
- **Usage**: Ensure error handling.

### 3. **@core**

- **Description**: Tests covering essential and critical functionalities.
- **Scope**: Ensure core features work as expected.
- **Important!** Use sparingly to avoid confusion. This tag may overlap with others but should only be used for crucial
  tests.

### 4. **@performance**

- **Description**: Tests measuring application performance.
- **Usage**: Run periodically to ensure performance standards.

### 5. **@accessibility**

- **Description**: Tests ensuring the application meets accessibility standards.
- **Usage**: Validate accessibility features and compliance.

### 6. **@security**

- **Description**: Tests ensuring application security.
- **Usage**: Validate security features and compliance.

### 7. **@usability**

- **Description**: Tests ensuring the application is user-friendly.
- **Usage**: Validate user experience and interface.

### 8. **@notImplemented**

- **Description**: Tests for requirements that are not yet implemented.
- **Usage**: Mark tests for unimplemented features to track and prioritize future work.
- **Link**: Refer to [Integrating Requirements into the Codebase](integrating-requirements-into-codebase.md) for more
  details on managing requirements within test descriptions.

## Running Tests with Tags

To run tests with specific tags, use your test runner's tag filtering feature. For example, to run all tests tagged
with `@e2e`:

```bash
test-runner --tags='@e2e'
```

You can also combine or exclude specific tags:

- **Combine Tags**:
    ```bash
    test-runner --tags='@e2e,@performance'
    ```
- **Exclude Tags**:
    ```bash
    test-runner --tags='@e2e' --exclude='@negative'
    ```