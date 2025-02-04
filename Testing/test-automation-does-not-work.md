## Test Automation Doesn't Work

Hi, if you feel that you're not fully leveraging modern test automation, you're probably right. Here are some signs that
your project might be falling short:

1. **Unstable Environment**: It's obvious that reliable tests can't be built without a stable environment.

2. **Not defined goals and structure**: If there are no defined goal of automation, scope of tests, conditions and
   limitations. There is no chance to complete automation and get practically applicable result.

   Here are typical mistakes that follow unstructered automation:
    - **Mirroring Manual Test Cases**: Automated tests should not simply replicate manual test cases. This approach
      requires extra effort for implementation and maintenance without added benefits. It also necessitates additional
      preconditions to keep tests independent, which can lead to performance issues.
    - **Lack of Naming Conventions**: Consistent naming conventions for application parts, test descriptions, titles,
      functions, and methods are crucial. Without them, understanding what part of the application a test verifies
      becomes difficult. Inconsistent naming is like making a pizza with whatever ingredients are currently in the
      fridge,
      leading to a chaotic and unstructured test suite. Here is my
      article: [Stop Sabotaging Your Tests: The Crucial Role of Naming Conventions](naming-convention)
    - **Generic Test Tags**: Using generic tags like smoke or regression for filtering your test suite is not effective.
      Instead, filter tests using clear and straightforward characteristics that are not subject to change. For more
      insights, see my post on [Test Tagging Strategy Without Tags](tagging-strategy.md).
    - **Outdated Test Cases**: Since the project is not structered very well, tests cover hidden logic, that means that
      there is no way to manage auto-tests, and spotting outdated tests becoming nearly impossible. getting rid of non
      relevant cases. test suite is managing you, not the other way around. Modern projects require continuous updates
      to test cases to
      reflect changing requirements. Like a surgeon, you need to accurately identify outdated tests, remove them, and
      seamlessly
      integrate replacements, but this os possible only if you defined and follow structure.

3. **Scattered Requirements**: If you're looking for requirements in various sources like chats with POs, BAs,
   developers, or testers, you lack stable expected results. In modern projects, especially those with limited budgets
   for business analysts, the best approach is to store requirements within the test code base as expected results. For
   more details, check out my approach
   in [Integrating Requirements into the Codebase](requirements-integration-practical-approach.md).

4. **Multiple Checks in One Block**: Follow the rule of one requirement per check. Checks should be atomic and granular.
   Without this, any metrics based on tests, from coverage to duration, will be inaccurate, and maintaining test cases
   will become an endless task. Here is my
   article: [The Golden Rule of Automated Testing: Are You Violating It?](golden-rule-of-automated-testing.md)

5. **Lack of a Test Repository Pipeline**: Without a pipeline for your test repository, you can't ensure the quality of
   tests or track their condition after updates.

### Subsequencies or characteristics of typical automation that can not

1. Complexity of Test Solution: The complexity of the test solution requires a lot of dedicated development resources
   for implementation, maintenance, and onboarding. 
2. Blocks the Delivery: Time-consuming testing phases with inconsistent and unclear results often block the CI/CD process, requiring
   manual approvals and reviews. 
3. Low Value: Most of the uncovered issues have minor priority, and the number of issues is low as well.

### Summary

Do you know any testing project without these issues? If so, you're lucky. Most developers are not as meticulous with
source code as they should be, which is why tests and test automation are necessary. To bring value, test automation
must be more precise than the code being tested. This requires more restrictions and responsibility. If these properties
can't be provided in your test automation, it might not be worth starting.