---
name: appropriation
description: Before specifying or building work that a reference codebase covers, map what is carried over from it (files, functions, constants, assets), where each piece lands, and the only edits allowed. Produces a reuse map the owner approves and implementation follows. Not a redesign or a rewrite.
---

# Map what to carry over from a reference codebase

When the mission or the owner names a codebase as a reference, the work is appropriation, not invention. The reference's code is the established behavior and look. New code is the exception, and each piece needs the owner's approval.

Run this before writing contracts for work a reference codebase covers, and again before each slice that reaches further into it. It is not needed for a fix inside code already carried over.

## Find what implements the scope

For each behavior in scope and each element of the views in INTERFACE, find what implements it in the reference: files, types, functions, constants such as colors, sizes, spacing, and fonts, assets such as icons, and the tests that cover it. Read the source and name paths and symbols. Do not work from memory or from documents describing the code.

## Classify each piece

- **Move:** carried over unchanged.
- **Move and edit:** carried over with named edits, only where the new structure requires it, such as a call that now crosses a process or crate boundary. Look, wording, and behavior stay as they are.
- **Wrap:** used in place through a thin adapter, only when moving it is impossible.
- **Leave:** not needed for this scope.
- **New:** needed, and the reference has nothing for it. Give the reason.

Moving is the default. Rewriting something the reference already does, even more cleanly, is not an option. A look or behavior different from the reference is a scope change for the owner.

Write each edit so it has one reading. "Commit through the record" can mean "apply and record" or "record what was already applied"; say which, and name the reference function whose behavior it keeps.

## Write the reuse map

Adapt [the template](templates/REUSE.md) and save it as `.project/documents/development/<version>/reuse.md`, updating it in place for later slices. Name the reference's path and commit. Group pieces by destination, each as source path → destination path, with its classification and edits. Put the New list last, each item with its reason. Plain prose and short lists.

## Approval and use

Bring the map to the owner with the New list first, together with the intent reviewer's verdict on it. Build nothing from it until the owner approves.

When building: copy each moved file unchanged into its destination first, then make only the mapped edits, so a diff against the source shows exactly those edits. A need that is not on the map goes back to the owner; it is not built on the spot. After the slice, check the diff against the source and record departures under "As built".
