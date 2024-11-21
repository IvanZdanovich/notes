# The Worthlessness of Automating Manual Test Cases

## Introduction

One of my biggest regrets as an SDET is striving to implement automated tests that could replace manual tests entirely.
I desperately tried to keep them aligned to avoid missing anything and to precisely calculate test coverage. However,
this approach doesn't work and offers no real benefits.

## Root Cause

We often seek clear metrics to measure what an SDET has achieved, how much work remains, and the effort required to
complete automation. The simplest and worst solution is to implement manual test cases in code and calculate coverage
based on the number of implemented test cases. Here's why this approach is flawed:

## Cons

1. **Impractical Implementation**: It is nearly impossible to implement manual test cases as they are. The only way is
   to rework initial manual test cases to adapt to the limitations and conditions of the testing framework.
2. **Maintenance Risks**: There is a huge risk that after an update to the functionality under test, the test cases
   themselves won't be updated accordingly.
3. **Tool Dependency**: It pushes you to use test management tools and integrate them into your test automation. More
   about the worthlessness of such tools and the risks of usage can be read here: [Useless and Expensive Test Management
   Tools](test-management-tools.md).
4. **Imprecise Metrics**: All metrics based on such automation, from coverage to time spent, are not truly precise and
   therefore useless.
5. **Useless Reports**: You will not have a chance to understand the issue by the generic name of a failed test case.
6. **High Maintenance Costs**: Maintenance of automated test cases based on manual test cases is time-consuming and
   expensive.
7. **Low Performance**: Performance of such cases is low because they are mostly not adapted for automatic execution but
   manual. Low performance will affect the whole CI/CD process, and you will need to find compromises to stick around
   this additional limitation.
8. **Lack of Flexibility**: Automated tests based on manual test cases lack the flexibility to adapt to new requirements
   or changes in the application.

## Pros

If you automate all the manual test cases regardless of the price and common sense, what are the benefits?

1. **Illusion of Progress**: Automating manual test cases might give the illusion of progress and thoroughness, but this
   is often misleading.
2. **Initial Satisfaction**: There might be initial satisfaction in seeing manual tests automated, but this is
   short-lived as the maintenance burden grows.
3. **Superficial Metrics**: You might achieve superficial metrics that look good on reports, but these metrics do not
   reflect true test coverage or quality.
4. **Temporary Reduction in Manual Effort**: There could be a temporary reduction in manual testing effort, but this is
   offset by the increased effort required to maintain the automated tests.

## Solution

Here are some tips to help you avoid my mistakes:

1. **Cover Requirements, Not Manual Tests**: Focus on covering the requirements, not the manual test cases. Practical
   example review here: [Integrating Requirements into the Codebase: A Practical Guide with Cypress](requirements-integration-practical-approach.md).
2. **Keep Tests Small and Focused**: Make your automated tests as small and focused as possible. Here I described the
   issue: [The Golden Rule of Automated Testing: Are You Violating It?](golden-rule-of-automated-testing.md).
3. **Define Structure and Naming Conventions**: Since you will have a lot of atomic checks, it is crucial to define the
   structure of tests and follow it. The first action is to define responsibility or scope for automated tests. Based on
   that, define and describe naming conventions. Here are details: [Stop Sabotaging Your Tests: The Crucial Role of
   Naming Conventions](naming-convention.md). Avoid using generic tags; read more about the issue here: [Test Tagging Strategy Without Tags](tagging-strategy.md).

## Conclusion

Automating manual test cases is not a viable strategy. Instead, focus on covering requirements, keeping tests small and
focused, and defining clear structures and naming conventions. This approach will lead to more effective and
maintainable test automation.