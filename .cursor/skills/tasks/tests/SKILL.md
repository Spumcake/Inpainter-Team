---
name: tests
description: Decide whether automated tests are warranted, which test level they belong to, and scope useful coverage when writing, changing, or reviewing tests. Use to prevent redundant tests and unnecessary verification loops; not for unrelated implementation work.
---

# Purposeful testing

Protect meaningful behavior with the smallest useful set of checks. A test must catch a plausible failure that matters to the assignment. The existence of code that could be tested is not enough.

## Decide before writing

Use the assigned acceptance criteria, changed behavior, and existing tests. Identify the failure a proposed test would catch and why it matters. If you cannot name that failure, do not add the test.

Check whether existing coverage already addresses it. Prefer updating a relevant test over adding overlapping cases or introducing another test framework. Keep this decision brief in the task's existing communication; do not create a separate test plan or report unless requested.

## When automated tests are useful

- User-visible behavior promised by the acceptance criteria, where automation reliably detects a regression.
- Business rules with meaningful boundaries or invalid transitions: for example, a discount that starts at exactly 100, or preventing an already-returned loan from being returned twice.
- Persistence, ownership, authorization, or integration behavior where failure could lose data, expose it, or break a key workflow.
- A reproduced defect whose cause can be covered by a focused regression test, written in the commit-time pass, or at once when it could lose or corrupt stored data.

Choose the level that exposes the failure. Use unit tests for isolated rules, integration tests for real boundaries, and end-to-end tests for a small number of important user flows when lower-level checks cannot establish the behavior. Do not test the same condition at every level by default.

Select representative cases and relevant boundaries. Avoid exhaustive combinations without a specific reason they could behave differently.

## When NOT to add tests

- Merely to prove a static heading, file, scaffold, or configuration string exists when inspection or an existing build check is sufficient.
- To mirror private implementation details, reproduce the same algorithm in the expected result, or confirm a mock returns the value you configured.
- To duplicate existing coverage or add variations that do not exercise a distinct failure.
- To cover hypothetical future features, invented requirements, or every defensive branch regardless of impact.
- To satisfy an arbitrary test count or coverage target that the project has not required.
- For a low-impact wording or visual adjustment when a preview or direct inspection provides better evidence.

These are decision criteria, not exemptions from real requirements. A visible label can warrant an accessibility check when that is the behavior at risk. Honor explicit project checks; do not delete useful tests or weaken assertions merely to reduce the count.

## Match the evidence to the claim

Use a human-reproducible preview for layout, appearance, and interaction judgments where appropriate. A snapshot or DOM assertion alone does not establish that a screen looks or feels right. Conversely, a preview alone does not establish data integrity or authorization behavior.

Mock external boundaries only when needed to isolate the behavior under test. A passing mock is not proof that a real service works. State when a real integration check remains blocked.

## Put each check at the right level

A check belongs at the cheapest level that reliably catches its failure. Do not run the same condition at several levels by default.

- **After a change:** the owner's look at the affected flow in the local preview, or a goal-based pass when one is requested. Use it for interaction, layout, and states that scripts cannot judge.
- **During a slice:** add no new tests. Behavior under build and review is still changing, and a test written now pins a draft the owner may reject. Run the existing scripted suite once when the change is complete, and again only after a relevant edit, so behavior that already worked stays working. A defect that could lose or corrupt stored data is the exception: cover it when it is fixed.
- **Commit-time test pass:** when the owner asks to commit work they have approved, add tests for the approved rules the every-commit checks should protect. Normally that is three to five for a slice, each naming the failure it catches. Prefer extending an existing test to adding one. Rewrite or remove existing tests whose behavior the slice deliberately changed, and report each removal and its reason.
- **Every commit:** when configured, the local pre-commit hook runs the agreed fast deterministic suite against staged contents and blocks failures. Never bypass it. GitHub Actions runs these on pushes and pull requests to `dev` and `main`. They must be deterministic and fast, normally a few minutes. Include the build, established lint or type checks, and the unit and integration tests that protect TASK rules and stored data. Exclude browser exploration, paid or rate-limited services, model calls, and anything that needs a person to judge the result. List the commands in testing.md so the workflow and local runs agree.
- **Release candidate:** an outside tester installs one tagged commit from a fresh clone and works through goal-based journeys, an upgrade with existing data where relevant, and a usability review. Use it for what the other levels cannot show: installing from the documents, first use, navigation, and fidelity to INTERFACE. It runs only when a candidate is tagged, never per commit.

When a new test is justified, decide its level first. A rule check that could run every commit should not wait for release testing; a usability judgment should not become a brittle every-commit script.

## Run and stop

Run the relevant existing checks and any justified new tests, plus required project gates. When they pass, stop. Rerun affected checks after relevant edits; broaden coverage only when a failure or concrete unresolved concern warrants it.

If verification stalls, diagnose the specific failure. Do not respond by generating more tests, building a custom verification framework, or repeatedly running the same unchanged checks. Do not weaken correct tests to obtain a pass.

Briefly report what was actually run, the result, and material limits. Distinguish executed tests, source inspection, and human preview. If no new tests were warranted, a short reason is sufficient. No new tests can be the correct outcome.
