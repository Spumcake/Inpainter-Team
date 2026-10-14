---
name: intent-review
description: Judge whether a plan or a finished slice achieves the point of the effort, not only its checklist. Compares the work with the mission's purpose and the fakes it names, looks for shortcuts that pass every check while missing the point, and reports "works, but misses the point" as a failure. Use before the owner approves a plan that decides structure, and when a slice is called done.
---

# Review the work against its point

Checks measure the letter of a requirement. When a shortcut passes them, agents reliably take it, and every role that stays inside its own assignment can do its job correctly while the purpose falls between them. This review is the one place where looking past the checklist is the job. Its question is not "does it work?" but "is this what the effort exists to achieve, or something that only looks like it?"

## When it runs

- Before the owner approves a plan that decides structure: a reuse map, a systems contract, or a slice's criteria. A fake is cheapest to stop before code exists.
- Once a slice's structure first exists in code, at its first integration, before the bulk of the work is built on it.
- When a slice is called done, before the owner is asked to try it or to commit it. Verification does not pass a slice without this review.
- Whenever the owner asks.

## What to read

Read the mission first: what is always true, its references, and its section on what the product is and what would fake it, if it has one. Then read the plan or the change, and the code or documents it actually touches. History explains how the work got here; it is never a reason to accept it. An approved document is not evidence that the point is met, because an approved plan can be where the fake started.

## Steps

1. **State the point.** For each thing the effort exists to prove, write one sentence in your own words: what must be true for it to have succeeded, beyond "it runs".
2. **Name the fake.** For each point, name the cheapest way the checks could pass while the point fails. Start with the fakes the mission lists, then think of others the work makes easy.
3. **Look for each fake in the work itself.** Read the code or the plan, not its summary: what each part actually depends on and calls, where state and history actually live, what the interface is actually drawn from, and what would happen with a different case. Prefer a substitution test: replace the one example the work was built around with a different one, such as another app, another file, or an empty case, and ask whether the work still holds.
4. **Give a verdict for each point:** achieved; faked, with the evidence; or unclear, with what would settle it.

## Judge the likely shortcut, not every conceivable cheat

Any finite set of checks can be beaten by someone set on beating it, so "a fake could pass" is not by itself a failure. A plan or slice **fails** only when its easy path leads to a fake, or when its checks would miss a shortcut an implementer acting in good faith is likely to take. A fake that needs deliberate effort against the plan's own words is a **residual risk**: list it with the test that would catch it, and the owner decides whether to accept it.

Shared code is not a fake. When the mission names a shared library or a shared component, judge who holds authority, not who holds capability: who decides, commits, records history, and writes files. A host that can draw a document is fine; a host that changes the document without the owner's reply is the fake.

## Report

- A fake on the easy path is a failure even when every check passes. Say plainly "this works, but it misses the point" when that is what you found. Do not soften it into a suggestion.
- Separate failures from residual risks, and end with one verdict: passes, passes with the listed residual risks, or fails.
- Give evidence: the file and symbol, and what it shows.
- Say where the shortcut came from: the document, rule, or instruction that made it the easy path, so the cause can be fixed along with the work.
- Give the smallest test that would fail on the fake and pass on the real thing.
- Return the report to the coordinator, who brings it to the owner. Do not fix the work, redesign it, or rewrite the documents yourself.

## Limits

This review does not rerun the verification suite, judge style, or reopen settled product decisions. It judges one thing: whether the work achieves the point the mission states. When the mission does not state the point clearly enough to judge, report that as a finding too.
