# Why Executable Specifications Rot — and How to Make the Spec the Requirement for Real

*The sturdiest test suites make the test itself the requirement — one source of truth instead of a spec and a test that
quietly drift apart. Here is why the most popular way of doing that broke down, and a version that lasts — where a
linter, not human discipline, keeps the requirement honest.*

Almost every test suite tells two stories that slowly stop agreeing. One story lives in a requirements document, a Jira
ticket, or a traceability spreadsheet. The other lives in the test code. On day one they match. By month six, a boundary
changed in one place and not the other, and nobody can tell you which story is true anymore.

The most celebrated answer to this is simple to state: collapse the two stories into one. Make a single artifact that is
*both* the requirement and the automated test. Update it once, and the spec and the check can never disagree, because
they are the same file.

It is a beautiful idea. It also mostly didn't work — and understanding *why* it didn't is the fastest route to a version
that does.

## The popular fix, and why it quietly broke down

The best-known attempt at "make the test the requirement" was to write requirements in a plain-English format called
**Given/When/Then** — *Given* some starting state, *When* something happens, *Then* expect this result. Tools like
Cucumber and SpecFlow let you write those sentences in a `.feature` file and wire each line to a snippet of code, so the
readable document and the runnable test are supposedly one and the same. Do this across a whole product and you get what
its advocates called **living documentation**: a spec that can't go stale, because running it is how you read it.

The *format* won. Even today, **71% of teams who work this way still write in Given/When/Then**. But the promise that
mattered — one file that is genuinely both the spec and the test — did not hold. A decade in, the approach's own
originator, Gojko Adzic, admitted it plainly:

> "The idea of specifications and tests in a single document didn't really work out as expected over the last 10 years."

The reason is worth pausing on, because it points straight at the fix. Gáspár Nagy, who built one of the most popular of
these tools (SpecFlow), diagnosed it: *"The plain-text file format of feature files is not strong enough to store all
the information required for collaboration."* A plain-text sentence can't hold everything a real requirement needs — the
exact boundary values, the test data, the priority, the linked ticket. So those details leaked back out into task
trackers like Jira, where surveys now find **57% of teams** keep them. Two stories again, drifting apart.

Here is the part everyone skips: the idea was never the problem. Teams that actually automate their examples report
better products — **26% rate them "great" versus 13%** of teams that write examples but never run them. The idea was
right. The *container* was too flimsy. So the real question is not "should the spec be the requirement?" It is: **what
container is strong enough to actually hold one?**

## The four things that rot, and what each one costs

Before the fix, name the decay precisely. Four things quietly turn a living spec back into fiction.

### The traceability matrix that becomes a map of a country that no longer exists

The traditional requirements-traceability matrix (RTM) is a spreadsheet linking requirements to tests, maintained by
hand, out of band from the code. Practitioners describe the failure mode bluntly: *"Most RTMs start useful and decay
into fiction."* When a requirement changes but its linked tests don't, "the linked tests verify a version of the
requirement that no longer exists." The map stops matching the territory, and no alarm sounds when it happens.

### The abstraction layer that turns into copy-paste plumbing

Here is the part BDD advocates rarely say out loud — so let BDD's own creator say it. Dan North, who coined
Behaviour-Driven Development, described a client's suite in a *Semaphore* interview:

> "They had thousands of… SpecFlow; they called them BDDs… the BDDs took many hours to run and there were thousands of
> them… they're practically copy paste, copy paste, copy paste with a few lines different."

All that glue code behind each plain-English sentence was supposed to buy one thing: a spec that non-programmers could
read and write. When they don't actually read or write it — the common case — you have paid for a whole layer of
indirection and received slow, duplicated plumbing in return. North's real point is that *"the stories, scenarios and
the code itself is a byproduct"* of a conversation; the framework was never the value. Defenders are right that misused
tooling isn't the tool's fault — but "misused" describes most of the installs you'll actually meet.

### The magic literal that no requirement backs

A raw `1` sits in an assertion. Is it the minimum allowed price? An array index? A retry count? Nobody knows, because
the number owns no meaning. It is like a part arriving on an assembly line with no purchase order and no bill of
materials — you cannot say what it is for, who ordered it, or what breaks if you change it. Multiply by a few thousand
assertions and your "specification" is a field of unexplained integers.

### The page object nothing keeps honest

A Page Object is the standard UI-testing pattern: a class that wraps a screen, exposing methods like `loginPage.submit()`
so tests read cleanly and selectors live in one place. It sounds like exactly the kind of single-source-of-truth this
article argues for — it even shares the goal outright. Which is precisely why it belongs here: it is the case study in
how that goal decays when nothing enforces it. Be precise about the ways it rots, because each one has a named critic on
record.

**It grows into a God object.** The sharpest structural charge comes from the authors of the rival Screenplay pattern —
John Ferguson Smart, Antony Marcano, Jan Molak and Andy Palmer — writing in *InfoQ*: page objects *"tend to grow,
becoming bigger and harder to maintain as the test suite grows,"* because they *"violate both the Single Responsibility
Principle (SRP) and Open-Closed Principle (OCP)."* The violation is concrete: one class changes when a selector changes
*and* when a task's sequence of steps changes — two reasons to change, fused together. Their companion write-up put a
tape measure to the drift. A page with two fields and one button already runs about **45 lines**; a real screen with
tens of elements *"can grow considerably… over 200 lines,"* carrying the *Large Class* smell and the duplication that
rides along with it — *"a bug being fixed in one place but recurring elsewhere."*

**Even its canonical author hedges on what belongs in it.** Martin Fowler, who wrote the definitional description of the
pattern, is firm that page objects *"should not make assertions themselves"* — because mixing assertion logic into a
page accessor *"leads to a bloated page object."* The pattern's own reference entry reads, in part, as a list of things
not to let it become.

**It keeps a second copy of the app's state.** Here Cypress's own team weighs in — Gleb Bahmutov, a former distinguished
engineer there, lists the runtime failures. The page object tracks "what page am I on, what's selected" separately from
the application's real state, and the two can silently disagree. Worse, the wiring underneath — the selectors — is, in
his words, *"NOT checked by any linter or code compiler,"* so a renamed element rots quietly until a test fails for a
reason that looks nothing like the cause.

**It forces every case through one interface.** Real screens have variations; a uniform page-object method absorbs them
as `if` branches, until the "clean" wrapper is a thicket of conditionals.

**It makes tests slow by routing everything through the UI.** Even pure setup — "log in, create three records, now test
the fourth" — gets clicked out one screen at a time. That cost is measurable: when Bahmutov replaced UI-driven steps
with **App Actions** — reaching into the running app to set state directly instead of clicking through it — a "mark all
complete" step dropped from **4–5 seconds to just over 1** (roughly 3×), and a full example suite fell **from 34 seconds
to 17** — half the wall-clock, gone.

Read the two loudest critics with their motives in view: Cypress sells the App Actions architecture, and the Screenplay
authors sell the pattern they'd rather you adopt. The critiques are motivated — and, on the mechanics, correct. Every
one rots for the same underlying reason: **the link between meaning and code is maintained by human discipline, out of
band, with nothing watching.** Discipline is the thing that always runs out first.

## The fix: a directed chain a machine can audit

The alternative keeps the promise and swaps the flimsy container. Instead of a plain-text feature file, the requirement
lives in **typed executable code, split across three layers, wired into a single directed chain that a linter audits on
every commit.**

The layers, and what each one *owns* — not just where code goes, but which kind of decision lives there:

- **Constraints** own boundary values, formats, enums, required-field lists. Declared once as frozen
  `SCREAMING_SNAKE_CASE` objects, imported everywhere.
- **Examples** own composition. Named test-data instances built *from* constraints — one key per tested state. The key
  names the case; the value is the payload.
- **Specs** own assertions and requirement grammar. Test titles form the Given/When/Then statement; `it` blocks carry
  requirement metadata.

Here is the same boundary value — a minimum booking price — flowing through all three.

**Layer 1 — the constraint declares the boundary once:**

```javascript
// constants/api/rb.booking.api.constraints.js
export const PRICE = { MIN: 1, MAX: 100_000 };
```

**Layer 2 — the example composes a named instance from it, never restating the number:**

```javascript
// integration-examples/api/rb.booking.api.examples.js
import { PRICE } from '../../constants/api/rb.booking.api.constraints';

export const booking_examples = {
  namePrefix: 'API.Booking',
  validBookings: {
    // key names the tested case; value is the executable payload
    allFieldsWithMinimalPrice: {
      firstname: `API.Booking.${utils.generateRandomString(6)}`,
      totalprice: PRICE.MIN,
      depositpaid: true,
      bookingdates: { checkin: utils.getFutureDate(1), checkout: utils.getFutureDate(7) },
    },
  },
};
```

**Layer 3 — the spec references the instance; the title and assertion cite the constraint, never a raw `1`:**

```javascript
// integration/api/rb.booking.api.spec.js
import { booking_examples as testData } from '../../integration-examples/api/rb.booking.api.examples';
import { PRICE } from '../../constants/api/rb.booking.api.constraints';

describe('RestfulBooker.Booking: Given no preconditions', { testIsolation: false }, () => {
  context(`RestfulBooker.Booking.Create.POST: When booking with price of ${PRICE.MIN} is provided`, () => {
    it(`RestfulBooker.Booking.Create.POST: Then return 200 and totalprice equals ${PRICE.MIN}`,
      { req: { p: 'P1' } },
      () => {
        cy.restfullBooker__createBooking__POST(testData.validBookings.allFieldsWithMinimalPrice)
          .then((res) => {
            expect(res.status).to.eq(200);
            expect(res.body.booking.totalprice).to.eq(PRICE.MIN);
          });
      });
  });
});
```

The value `1` now flows unbroken: `PRICE.MIN` → `allFieldsWithMinimalPrice.totalprice` → the context/it titles → the
assertion. Change the minimum in one file and every layer that depends on it moves with it. Nothing to sync by hand.

## What replaces the page object

Notice what the spec above did *not* do: it never clicked through a screen to create the booking. It called
`cy.restfullBooker__createBooking__POST(...)` — a **custom Cypress command** that hits the API directly. This is where
Cypress's real advantages show up, and they map one-to-one onto the page-object failures listed earlier.

- **Setup runs through the API, not the UI.** To test the fourth record you create the first three with a single POST
  apiece, in milliseconds, instead of clicking each into existence. This is the App Actions idea from earlier applied to
  every precondition — the reason a suite can shed half its wall-clock. The UI is exercised only where the UI is the
  thing under test.
- **The wiring is checked.** The failure that sinks page objects — selectors and method names nothing verifies — is
  closed here by a custom ESLint rule that enforces the command grammar `resource__action__METHOD`
  (`createBooking__POST`, `deleteByNames__DELETE`). A command that doesn't match the shape fails the build, so the glue
  can't silently rot.
- **One command, no conditional thicket.** Because a command does one thing, there is no uniform interface straining to
  cover five cases with `if` branches. Variation lives in the named examples, not in the helper.

The page object tried to be a single source of truth for a screen and failed because nothing enforced it. A named
command whose shape a linter checks, driving state through the API, is the same intent with the enforcement bolted on.

## What actually makes the chain hold — the mechanics worth stealing

Splitting code into layers is ordinary. What makes this survive where the plain-text version didn't is a handful of
specific, non-obvious mechanisms.

### Two-sided orphan detection, enforced by a repurposed smoke detector

A mid-chain literal — a raw number in a spec that *could* trace to a constraint — is an **orphan**: no requirement backs
it, no boundary governs it. The clever part is how orphans get caught. ESLint's generic `no-magic-numbers` rule is set
to *warn on every spec file* — and then reinterpreted: **a magic-number warning means a boundary value escaped its
constraint file.** A rule shipped to nag about unnamed integers has been rewired into a chain-gap sensor, like using a
smoke detector to prove nobody left a burner on.

The detection runs both directions. Custom rules also catch the *reverse* orphan — an example or selector that's defined
but never consumed by any spec — so dead requirements can't accumulate unnoticed.

### Typed sentinel placeholders for values that don't exist yet

Some fields hold an ID the server won't mint until you POST. Instead of `null` or `''`, the field is assigned the native
constructor:

```javascript
const ids = {
  auditRoundId: String, // filled in after the record is created
  questionCategoryId: Number, // filled in after the record is created
};
```

The constructor does double duty: a type annotation *and* an unmistakable "not populated yet" marker — like a name tag
reserved at a conference desk before the guest walks in. The slot's identity is fixed before its value exists.

### One runtime write, propagated by getters

When the ID does arrive, the spec writes it **once** to the source instance. Dependent instances read it lazily through
ES getters:

```javascript
const examples = {
  ids: { questionCategoryId: Number }, // the one place the created ID is written
  createAllFields: { /* ...the base payload... */ },
  // a dependent instance re-reads the source ID every time it is accessed
  get childCategory() {
    return { ...this.createAllFields, parentQuestionCategoryId: this.ids.questionCategoryId };
  },
};
```

Assign the same ID to five instances by hand in setup and you've quietly created five sources of truth for one fact. The
getter keeps it at one: write to the source, and every dependent re-resolves at access time. The same discipline derives
aggregates from structure (`get totalQuestions() { return Object.keys(this.FHRSQuestions).length; }`) rather than
hardcoding a count that a sixth question would silently falsify.

### The test title *is* the machine-readable requirement

Titles follow a lint-enforced grammar — `Module.Submodule.Operation.METHOD: When/Then …` — that is globally
deduplicated, normalized to a canonical vocabulary (`show` → `display`, `btn` → `button`), and cross-checked against
generated module/component registries that also drive coverage reports. Fourteen custom ESLint rules, all set to
`error`, hold this together, under a **zero-tolerance ratio**: the pre-commit hook blocks on any new warning or error.
Each `it` carries a `req` object — priority (`P1`/`P2`/`P3`), story `refs`, `bugs`, a free-text `note` — that machines
extract into requirement and coverage reports (the coverage gate fails below an 80% threshold). This is the stronger
container in practice: the requirement metadata that leaked out into Jira now lives *in the test*, typed and enforced.

### The honesty channel — assert the bug, don't hide it

When behaviour is wrong, the reflex is to write a failing test. This methodology inverts it: assert the **actual, broken
** behaviour so the suite stays green and order-independent, and record the *expected* behaviour in an append-only bug
log referenced from `req.bugs`. The suite never lies about what the system does today, and the deviation is tracked
rather than buried in a red test everyone learns to ignore.

## The honest trade-offs — where this fights the consensus

A reference worth trusting names what it gives up. Three positions here run ahead of, or against, the mainstream.

**You lose the non-coder-readable spec.** Gherkin's whole reason to exist is that a business analyst can read and even
write a scenario. Move the spec into typed Cypress code and that door closes — only engineers read the requirement of
record now. If your product analysts genuinely author scenarios, BDD's abstraction earns its keep and this approach
costs you something real. If they never did — again, the common case — you're shedding ballast, not value.

**You're choosing the pattern the experts argued against.** A file of named, pre-composed instances is the **Object
Mother** pattern. Nat Pryce, who literally wrote the alternative, warns that Object Mothers become *"bloated, messy and
hard to maintain… full of duplicated code,"* and prefers fluent Test Data Builders with per-test overrides. The defense
here is specific: constraints-as-single-source plus composition attacks exactly the duplication Pryce feared — instances
share boundary values by import rather than by copy. But it *is* the pattern he cautioned against, and builder advocates
have a case.

**"The spec replaces the RTM" is more radical than the sources support.** The industry consensus is to move the
traceability matrix into a live test-management tool, not to eliminate it. The claim that a constraint-to-assertion
chain removes the need for any matrix at all is this methodology's own thesis — corroborated in spirit (traceability as
a byproduct of execution) but not stated outright by an authoritative source. Hold it as a considered bet, not received
wisdom.

And treat the scary numbers with suspicion. The widely quoted "flaky tests cost a 100-person team \$2.6M/year" and "a
production defect costs 100× a requirements-phase fix" are vendor marketing and disputed folklore respectively — cite
neither. The trustworthy figure is Google's: **~1.5% of test runs flake, ~16% of tests show flakiness over a month,
and ~84% of pass→fail transitions are flakes, not real regressions.** That is the real tax an untrustworthy suite
levies, and it's the tax a machine-audited chain is built to lower.

## When to reach for this — and when not to

Adopt it when your suite is code-owned, your requirements keep drifting from your tests, and your team is engineers
who'll never read a `.feature` file anyway. The payoff scales with size: the larger the suite, the more a
machine-audited chain beats human discipline that was always going to run out.

Skip it — or blend it — when non-technical stakeholders truly co-author scenarios (keep the BDD layer there), or when a
suite is small enough that a spreadsheet genuinely stays honest. Be warned that the discipline is real: typed
placeholders, getter-propagated IDs, and title grammars are enforced precisely because they're easy to get wrong by
hand.

## The one thing to take away

The original idea was right: the spec should *be* the requirement. The mistake was the container — a plain-text feature
file couldn't hold one, so the requirement leaked back into trackers and the two stories drifted apart again, exactly as
before.

The fix isn't more BDD tooling or a better spreadsheet. It's a stronger container and a machine that watches the seams:
put the requirement in typed code, make every boundary value trace from a single constraint through a named example into
an assertion, and let a linter — not anyone's discipline — fail the build the moment a literal appears that no
requirement backs. Do that, and for the first time the spec and the test can't drift, because there was only ever one
story.

Pick one magic number in your test suite this week and ask: what requirement backs it? If you can't trace it to an
owner, you've just found where your spec stopped being true.

## Sources

**Quotes**

- "The idea of specifications and tests in a single document didn't really work out as expected over the last 10 years."
  — Gojko Adzic, *Specification by Example, 10 years later* (2020). <https://gojko.net/2020/03/17/sbe-10-years.html>
- "The plain-text file format of feature files is not strong enough to store all the information required for
  collaboration." — Gáspár Nagy (creator of SpecFlow), quoted in Adzic's 2020 retrospective (link above).
- "They had thousands of… SpecFlow… the BDDs took many hours to run… copy paste, copy paste, copy paste with a few
  lines different" and "the stories, scenarios and the code itself is a byproduct" — Dan North, interview, *Semaphore*.
  <https://semaphore.io/blog/dan-north-testing>
- "…use selectors to find elements, which is NOT checked by any linter or code compiler," plus the second-copy-of-state,
  conditional-thicket and slow-through-the-UI failure modes — Gleb Bahmutov, *Stop Using Page Objects and Start Using App
  Actions*, Cypress blog (vendor advocacy for the alternative — read as motivated).
  <https://www.cypress.io/blog/stop-using-page-objects-and-start-using-app-actions>
- "tend to grow, becoming bigger and harder to maintain… violate both the Single Responsibility Principle (SRP) and
  Open-Closed Principle (OCP)" — John Ferguson Smart, Antony Marcano, Jan Molak, Andy Palmer, *Beyond Page Objects: Next
  Generation Test Automation with Serenity and the Screenplay Pattern*, InfoQ (authors of the rival pattern — read as
  motivated). <https://www.infoq.com/articles/Beyond-Page-Objects-Test-Automation-Serenity-Screenplay/>
- "over 200 lines of code," the *Large Class* smell and "a bug being fixed in one place but recurring elsewhere" — same
  authors, *Page Objects Refactored: SOLID Steps to the Screenplay/Journey Pattern*, DZone.
  <https://dzone.com/articles/page-objects-refactored-solid-steps-to-the-screenp>
- Page objects "should not make assertions themselves" / mixing assertion logic "leads to a bloated page object" —
  Martin Fowler, *PageObject* (bliki). <https://martinfowler.com/bliki/PageObject.html>
- "bloated, messy and hard to maintain… full of duplicated code" (on the Object Mother pattern) — Nat Pryce, *Test Data
  Builders: an alternative to the Object Mother pattern*. <http://www.natpryce.com/articles/000714.html>
- "Most RTMs start useful and decay into fiction" / "the linked tests verify a version of the requirement that no longer
  exists" — composite of practitioner writing on traceability-matrix decay (TestRail, PractiTest, testRigor, and
  similar). No single canonical source; treat as widely-held practitioner consensus rather than an authoritative claim.

**Statistics**

- 71% of teams write examples in Given/When/Then; 57% keep requirements in a task tracker (12–20% in version-controlled
  text files); teams that automate their examples rate products "great" 26% vs 13% — Adzic's 2020 survey, reported in
  *Specification by Example, 10 years later* (link above).
- ~1.5% of test runs flake, ~16% of tests show flakiness over ~30 days, and ~84% of pass→fail transitions are flakes
  rather than real regressions — John Micco, *Flaky Tests at Google and How We Mitigate Them*, Google Testing Blog
  (2016). <https://testing.googleblog.com/2016/05/flaky-tests-at-google-and-how-we.html>
- App Actions performance (a step ~4–5s → ~1s; a suite 34s → 17s) — Bahmutov, *Stop Using Page Objects…* (link above).

**Figures deliberately not relied on**

- "Flaky tests cost a 100-person team \$2.6M/year" (vendor marketing, unverified methodology) and "a production defect
  costs 100× a requirements-phase fix" (widely repeated but of disputed provenance, often traced to an unpublished IBM
  Systems Sciences Institute figure). Named in the article only to warn against citing them.
