## Test Tagging Strategy Without Tags

Organizing and executing tests efficiently is crucial for maintaining a robust and scalable test suite. Tagging tests
helps streamline test runs for different purposes, such as integration, regression, smoke testing, and more.

While tags like @smoke and @regression sound useful, their application often reflects the subjective judgment of the
tester. To make test filtering more meaningful, tags should directly relate to the functionality and purpose of the
tests.

Instead of using abstract tags like smoke or regression, consider filtering tests by clear, functional categories such
as API, UI, E2E, or component tests. Embedding the type of test into the test file name can also help maintain
consistency and clarity.

Here are some examples of more functional tags:

- @negative
- @performance
- @accessibility

While these tags are not inherently bad, a consistent approach based on project structure and proper grouping of tests
within test files can often eliminate the need for any tags.
