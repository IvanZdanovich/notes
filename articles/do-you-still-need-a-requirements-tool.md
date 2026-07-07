# Why the Traceability Matrix Is the First Thing to Rot — and When You Can Delete It

*The received wisdom is that a serious, auditable requirements process needs a dedicated requirements-management tool and a
traceability matrix linking requirements to tests to defects. For a code-owned test suite, that matrix is the single most
reliable component to decay into fiction. Here is the evidence, the arithmetic, and the honest short list of cases where the
tool still earns its licence.*

Ask a team how they know every requirement is tested, and you will usually be pointed at a matrix: a grid in Jira + Xray,
Zephyr, DOORS, or Polarion that links each requirement ID to a test-case ID to a defect ID. The matrix says coverage is
complete. Everyone trusts it. Almost nobody has read both sides of every link this quarter.

That trust is the problem. The matrix is a *copy* of a relationship the code already contains, kept in agreement with reality
by human discipline, out of band, with nothing watching. And the failure mode is silent: when a requirement changes but its
linked test doesn't, the grid still shows a green link while it now points at a version of the requirement that no longer
exists. Coverage *looks* complete precisely when it has stopped being true.

This article compares that traditional setup — call it **RM** — against an approach where the executable test *is* the
requirement of record and traceability falls out of the code's own import graph — call it **CES**
(constraint → example → spec). The claim is not that RM is obsolete. It is narrower and more useful: for a suite that
engineers own end to end, most of what an RM tool sells you is a second copy you now have to keep honest, and a machine can do
that job better than a matrix can.

## The two contenders, in one breath each

- **RM — a traditional requirements-management tool** (Jira + Xray, Zephyr Scale, IBM DOORS, Siemens Polarion). Requirements,
  test cases, and defects are separate records linked by a traceability matrix. The automated test is a *further* record,
  mapped back to its test-case record by a sync plugin that uploads results over an API. One platform holds everything; humans
  keep the links current.
- **CES — the constraint-example-spec approach.** A value lives in exactly one layer and every other layer imports it: a
  boundary in a constraint file, named data in an example, the requirement in a spec title plus its assertion. Bugs live in an
  issue tracker, referenced from the spec via a metadata field. Everything is plain files in git.

Every downstream difference between them follows from a single design choice.

## The root difference: does a fact have one home, or four?

The DRY principle — from Hunt and Thomas's *The Pragmatic Programmer* — is usually quoted as "don't copy code." Its actual
formulation is broader and sharper:

> "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."

They meant it to cover schemas, build systems, test plans, and documentation — not just functions. A requirement's boundary
value is exactly such a piece of knowledge. So the real question separating RM from CES is: **how many authoritative copies of
that value exist?**

- **CES — one.** A booking's minimum price is a constant in a constraint file. The example composes its payload from that
  constant. The spec's title and its assertion both read the same constant. One value, three referencing layers, zero copies.
- **RM — four.** The same value is restated in the requirement record, again in the test-case steps, again in the automated
  test, and the matrix records that they *should* agree. Four copies plus a link asserting they match.

Hunt and Thomas name the payoff of the single-home version precisely: when DRY holds, "a modification of any single element of
a system does not require a change in other logically unrelated elements." That sentence *is* the CES edit story, and its
absence is the RM edit story. CES trades away a shared, non-engineer-editable platform to buy this one property — a fact cannot
disagree with itself, because it exists once.

## The mistake that quietly costs the most: traceability you maintain by hand

Here is the hidden cost of the RM model, and it is not the licence fee. It is that traceability becomes a *curated artefact* —
a thing a person keeps true — rather than a *structural fact* the system can't violate.

The decay of hand-maintained matrices is well documented, not folklore. The traceability-research literature has a name for
it — **traceability decay**, "the progressive degradation of trace links as systems evolve" — and practitioners describe the
tipping point bluntly: a stale matrix is *worse* than no matrix, because it gives false confidence. A 2026 research preprint,
*ReqToCode*, puts the mechanism in one line: requirements, code, and tests "evolve independently across tools, repositories,
and revisions," creating "traceability debt that accumulates silently." Its diagnosis of *why* is the crux — in the
traditional model, "at no point does the system itself enforce consistency between these artifacts and the traceability
record."

That is the whole game. Nothing enforces it, so it rots.

And the maintenance is not free while it lasts. The same preprint reports that "80% of surveyed practitioners identified cost
as the primary barrier to traceability adoption, with manual maintenance burden cited as a dominant contributor." (Treat the
exact figure as a preprint's, not peer-reviewed gospel — but the direction is uncontroversial to anyone who has kept a matrix
current.) Too much traceability, maintained by hand, becomes a full-time job, and teams respond by deferring updates until the
artefact has quietly stopped serving its purpose.

### What CES wires instead: traceability as a side effect of the code

CES doesn't maintain traceability. It makes it structural — a consequence of how the files import each other, not a document
anyone updates.

A boundary value flows constraint → example → spec by `import`. The chain *is* the dependency graph. A literal that appears
mid-chain with no owning layer is an *orphan* — a `1` in an assertion that no constraint backs — and a custom lint rule
rejects it on commit. You cannot ship a broken link without the build noticing.

The analogy: an RM matrix is a hand-drawn map of a country, redrawn by a clerk whenever the borders move. CES has no map,
because you navigate by the roads themselves — and if a road leads nowhere, the compiler refuses to let anyone drive it. The
question "is every requirement tested?" stops being answered by *trusting the matrix* and starts being answered by *the linter
and the resolver*: a requirement with no backing example or assertion simply does not resolve.

This is not a fringe idea. A small ecosystem of tools now treats requirements as version-controlled, build-checked artefacts:
[Doorstop](https://github.com/doorstop-dev/doorstop) stores each requirement as a YAML file in git and validates the trace
tree; [StrictDoc](https://strictdoc.readthedocs.io/) and
[OpenFastTrace](https://github.com/itsallcode/openfasttrace) run tracing as part of a Maven or Gradle build. CES is the same
instinct pushed all the way into the test code: don't store the requirement *next to* the artefact that verifies it — make
them the same artefact.

## The arithmetic: how many places does one change touch?

"Traceability that can't rot" is qualitative. The cost gap is not — you can count it. Each number below is one distinct place a
person opens and saves: create, edit, or delete a file or record. System-generated output counts zero. The gap comes entirely
from where a fact lives — one home in CES, four linked copies in RM.

| Change                                      | CES      | RM       |
|---------------------------------------------|----------|----------|
| Add a requirement                           | 2        | 6        |
| Change a requirement (e.g. expected result) | 1        | 4        |
| Remove a requirement                        | 2        | 5        |
| Change one shared value used by N tests     | 1        | 1 + N    |
| Log a bug                                   | 2        | 3        |
| Coverage report / audit                     | 0 (auto) | 0 (auto) |

**Average per change: CES ≈ 1, RM ≈ 3.**

The rows that look ordinary hide the real story. Take *change one shared value used by N tests* — a limit shared by twenty
tests. In CES it has one home; you edit the constraint once and all twenty specs re-read it: **1 edit**. In RM the value was
copied into each test-case record, so you touch the definition plus all twenty copies: **21 edits**. The gap isn't a constant
tax; it *scales with the suite*, which is exactly backwards from what you want as the suite grows.

The other counts, unpacked:

- **Add a requirement** — CES: the spec (title + assertion) plus its constraint/example data. RM: requirement record +
  test-case record + their mapping link + automated test code + code-to-test-case link + placement in a plan or cycle.
- **Change a requirement** — CES: edit the one spec in place (or the one constraint it reads). RM: requirement record +
  test-case steps + automated test code + a re-sync so the mapping and last result stay consistent.
- **Remove a requirement** — CES: delete the spec, drop its now-unused constraint/example data. RM: deprecate the requirement
  record + test-case record + mapping link + automated test code + execution history.
- **Log a bug** — CES: create the issue, reference it from the spec via `req.bugs`. RM: defect record + link to test case +
  link to requirement.

These counts model drift risk and coordination cost, not wall-clock time; exact RM counts move ±1 with the platform and its
sync plugin. What they capture is the number of independent chances for a copy to be updated wrongly — or not at all.

## Drift: why the copies diverge, and why one approach can't

Drift is a copy disagreeing with the fact it was copied from. It is the failure mode the two approaches handle most
differently, and it follows directly from the arithmetic above.

RM stores the same fact in several records and relies on discipline to keep them equal. Every one of those 4, 5, or 21 edits
is an opportunity for one copy to change and the others to be missed. The matrix still shows a link, so the drift is invisible
until someone reads both records side by side — which, per the decay literature, is exactly when the team has stopped doing so.

CES has nothing to drift *to*. A fact exists once and is imported; a change is applied at the source and observed everywhere at
access time. The only "drift" that can even occur is an orphan literal, which the linter rejects. Drift risk isn't *managed* by
process — it is *structurally bounded* by the design. That is the difference between a diet you have to stick to and a kitchen
with no junk food in it.

## The hidden dependency RM adds to every test run

There is a subtler cost that the edit table doesn't show. An RM setup makes each CI run reach out to a live external service:
the sync plugin uploads results to Xray or Zephyr over a REST API, authenticated with a token, through a specific plugin
version, against a schema the platform owns.

Count the ways that pipeline can break independently of your test quality: the API is down, the token expired, the plugin
lagged a platform upgrade, a link went stale, the result schema changed. That's five failure surfaces bolted onto the one that
matters — a genuinely failing test. When any of the five trips, your tests can pass while their results never arrive: a red
build caused by plumbing, not by the software under test. CES has no such dependency — results are a local artefact of the run,
sitting in the same git tree as everything else.

And then the licence. RM tooling is paid, per seat, and scales with headcount: Xray and Zephyr start around **\$10/month for up
to 10 Jira users** and then bill per user in tiers (Zephyr Scale runs roughly **\$1.82–\$2.30 per user per month** in the
11–100 band); DOORS and Polarion are enterprise, quote-based, and materially more. CES's tooling cost is an issue tracker you
almost certainly already run. Neither number is large for a small team — but one grows with every engineer you hire and every
run you make, and the other doesn't.

## Where the RM tool genuinely wins — and it's not close

If the article stopped here it would be marketing, not a reference. There is a real and non-negotiable set of cases where a
dedicated RM tool is the correct choice, and they share a single root: **someone other than the build must be able to trust,
author, or audit the requirement.**

**Regulation that mandates a validated system of record.** This is the decisive one. Safety- and health-critical standards
don't merely suggest traceability — they *require* it, bidirectionally, as certifiable evidence:

- **DO-178C (avionics)** requires verifiable trace data in both directions across system needs, software requirements, source
  code, test cases, and results — and explicitly to prove no orphan or dead code reaches a certified build.
- **ISO 26262 (automotive)** requires bidirectional traceability from every safety requirement to its origin and to its
  verification evidence, backed by a safety-case argument.
- **IEC 62304 (medical-device software)** requires traceability across requirements, risk analysis, and change control.
- **FDA 21 CFR Part 11** requires a validated, access-controlled system with a computer-generated, time-stamped audit trail of
  every change — who changed what, when.

A git history plus a linter is a strong engineering argument, but it is not, today, an FDA-accepted audit trail with named
electronic signatures. This is precisely why DOORS and Polarion dominate aerospace, automotive, and medical devices — the tool
*is* the compliance evidence. If you operate here, use the tool; the rest of this article is a footnote.

**Non-engineers must author or approve requirements.** If business analysts genuinely write scenarios and stakeholders sign
off by name, they need a platform they can edit and a formal approval workflow. CES puts the requirement in typed code, which
closes that door — only engineers read the requirement of record.

**Manual and automated test cases must live side by side**, or **one requirement spans several teams and repositories.** A
single code import graph can't reach across repos or hold test cases that were never automated; a shared platform can.

Notice the pattern: RM wins wherever the *audience or author* of the requirement is outside the codebase. CES wins wherever the
codebase *is* the system of record.

## The hybrid that takes most of both

You don't have to choose all-or-nothing. Keep CES as the source of truth and auto-publish read-only extracts into the platform
for dashboards and sign-off. A small extractor turns the spec titles and their requirement metadata into JSON, YAML, or
Markdown reports — always current, because the report and the tests are the same artefact.

Engineers still edit only code; the platform receives a generated, never hand-edited view. You get most of RM's visibility and
audit surface at CES's low edit cost — at the price of one more generated integration to keep running. It's the right answer
when you need the dashboard for humans but refuse to pay the drift tax of humans maintaining it.

## What the arithmetic still leaves out

Honesty about the model: "edits per change" counts places to save, not time or effort. It says nothing about how long each
edit takes, the real value of one shared platform for non-engineers, the maturity of a platform's sign-off and audit tooling,
or whether *your* regulator accepts code-and-git as a system of record. Those depend on your organisation, and no table settles
them.

## The one question that decides it

Pick your most-changed requirement — the limit, the boundary, the expected result that moves every other sprint. Now ask: **if
that value changed tomorrow, how many separate places would a person have to open and save to keep the whole system honest —
and what warns you if they miss one?**

If the answer is "one place, and the build fails if it's wrong," you are already living in CES and a requirements tool would
add copies to maintain, not truth to trust. If the answer is "several places, and nothing warns us — but a regulator demands
the paper trail anyway," then the tool isn't overhead, it's the deliverable. Everyone else is paying to keep a matrix honest
that a linter would keep honest for free.

## Sources

- **DRY / single source of truth** — "Every piece of knowledge must have a single, unambiguous, authoritative representation
  within a system"; "a modification of any single element of a system does not require a change in other logically unrelated
  elements." Andy Hunt & Dave Thomas, *The Pragmatic Programmer*, as summarized on
  [Wikipedia: Don't repeat yourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself).
- **Structural / compile-enforced traceability, traceability decay, and the 80% maintenance-cost figure** — *ReqToCode:
  Embedding Requirements Traceability as a Structural Property of the Codebase* (2026 preprint),
  [arxiv.org/html/2603.13999](https://arxiv.org/html/2603.13999). Preprint, not peer-reviewed — cite the framing, treat the
  exact statistic as indicative.
- **Requirements-as-code tooling** — [Doorstop](https://github.com/doorstop-dev/doorstop) (requirements as YAML in version
  control), [StrictDoc](https://strictdoc.readthedocs.io/),
  [OpenFastTrace](https://github.com/itsallcode/openfasttrace) (build-integrated requirement tracing).
- **Regulatory traceability mandates** — overview of DO-178C, ISO 26262, and IEC 62304 bidirectional-traceability
  requirements: [Trace.Space, *Traceability in Compliance Projects*](https://www.trace.space/blog/traceability-in-compliance-projects);
  [Jama Software, *Bidirectional Traceability*](https://www.jamasoftware.com/requirements-management-guide/requirements-traceability/bidirectional-traceability/).
- **FDA 21 CFR Part 11** — validated system of record and computer-generated audit trail requirements:
  [SimplerQMS, *21 CFR Part 11 Requirements*](https://simplerqms.com/21-cfr-part-11-requirements/).
- **RM tool pricing (order of magnitude)** — [Xray Test Management pricing](https://qaskills.sh/blog/xray-test-management-pricing-2026)
  and Atlassian Marketplace tiered per-user model; confirm live pricing before committing.
</content>
</invoke>