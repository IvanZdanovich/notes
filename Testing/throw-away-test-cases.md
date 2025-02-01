# This World is Doomed While You Write Test Cases

There is no chance to save the Amazon forests, endangered species, or the clean air around us while we continue to
create test cases for the sake of creating test cases. It seems we have lost our way in building quality products. By
setting ourselves a huge list of abstract test cases consisting of billions of steps, we strive to shift the burden of
performing these repetitive actions to automation. Here, Bill Gates' quote is more relevant than ever: “The first rule
of any technology used in a business is that automation applied to an efficient operation will magnify the efficiency.
The second is that automation applied to an inefficient operation will magnify the inefficiency.” To save the world, you
don't need to go to the Amazon forests; start by refusing to write test cases before the specification.

First, let's consider the following concepts for accuracy in further discussions:

- **Test Case**: A set of input values, execution preconditions, expected results, and execution postconditions,
  developed for a particular objective or test condition, such as to exercise a particular program path or to verify
  compliance with a specific requirement.
- **Specification**: A detailed and precise description of a system’s behavior, features, and constraints. It serves as
  a basis for designing, developing, and testing the system. Specifications can include functional requirements,
  non-functional requirements, and design specifications.

I hope you caught the difference. A specification is an exact description of what we design, then create, and then test.
The specification is created before the first programmer writes a single line of code. Yes, it may not be formalized in
any form, but its interpretation will be implemented by the developer; the tester's interpretation will be tested. At
this stage, differences between the tester's and developer's interpretations are found; differences between the
interpretation and the specification itself will be discovered when the customer receives the product.

Now stop! Ask yourself and try to honestly answer: are there formalized specifications on your project? Are all
specifications up-to-date, and how do you keep them current? How many specification checks are automated?

I am sure that on most projects, specifications are either ignored or have gaps in the description of existing
functionality. Unfortunately, this has become the standard and the rule, not the exception.

Now I will try to guess a few problems you are facing:

- **No Single Source of Truth**: Specifications are not formalized, not structured, or may be stored in different
  places. Therefore, your team has disputes: is it a bug or a feature? Knowledge transfer to new team members is a
  complex and lengthy process.
- **You Do Not Formalize Specifications Before Writing Code**: Therefore, it is very difficult for you to assess the
  priority and complexity of the task of creating new functionality, testing it, and automating testing.
- **Test Cases Can Cover Any Number of Specifications**: Therefore, metrics based on test cases do not reflect reality.
- **The Name of the Test Case Does Not Reflect All the Specifications It Contains**: Therefore, it is difficult for you
  to find a specific test case covering a specific specification.
- **You Do Not Use Automation to Check the Format and Content of Specifications**: Therefore, you can find redundant or
  incorrect specifications only manually.
- **Your QA Specialist Is Your Knowledge Base**: Therefore, when they leave the position, no one will be able to
  understand your test suite.
- **When Editing Existing Test Cases, It Is Difficult to Highlight Specific Specifications**: Therefore, you need to
  keep additional conditions and restrictions in mind, which causes apathy and a drop in efficiency.

On most projects I have worked on, requirements were most accurately described at the stage of implementing test
automation. This in no way brought the product closer to the desired result; since the feature is already implemented
and resources are spent. The role of automation in this case is reduced to laminating what is there: at best, protection
from changes; at worst, blocking implementation from changes.

A possible approach to building an effective product creation cycle is described in the book "Specification by Example"
by Gojko Adzic. It is suitable for specialists even without coding experience. The book also well describes the idea of
creating and maintaining living documentation based on specifications.

The foundation is specifications; test cases are built on their basis. Test cases are secondary. In practice, test cases
are often written without a formalized specification. What do you think: if I wrote 100 test cases, what part of the
functionality did I cover? And if I write 1000 test cases, will I cover all the functionality? What number of test cases
guarantees sufficient coverage? How many test cases do I need to rewrite when part of the functionality changes? Only by
relying on specifications can you accurately calculate the coverage of functionality. Having only specifications, you
always know what you need to check and how. Based on specifications, it is easy to implement automation from validating
the specifications themselves to collecting metrics.

Why don't we write specifications? I constantly ask myself this question. To shift testing to the left, we must rely on
the formalization of specifications, but instead, we automate test cases. Performing actions and checks based on
interpretation rather than specification leads to inefficient use of machine time, which forces us to extensively
increase the number of servers and their power. The carbon footprint of such decisions is felt when we go outside every
day. I assume that the industry does not focus on specifications because it is aimed at frequent changes, which is due
to the quality of planning. The lack of an exact building project may be cheaper and faster at the initial stage... If
you are building a shed from improvised means. After the first disappointment, you will start counting boards, nails,
and minutes spent on this venture. The difficulties you may face when implementing a specification-based approach will
pay off many times over. Unfortunately, I cannot recommend other effective alternatives for building a quality assurance
system.

P.S.: If you are still worried that we live in the Matrix, don't worry! Humanity constantly lacks computing power; there
cannot be a computer capable of withstanding the full power of the inefficiency of our solutions.

Write specifications
Follow specifications
Make the world better
