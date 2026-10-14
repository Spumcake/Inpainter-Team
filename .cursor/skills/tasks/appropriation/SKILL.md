---
name: appropriation
description: Before specifying or building work that a reference codebase covers, map what is carried over from it — files, functions, constants, and assets — where each piece lands, and the only edits allowed. Produces a reuse map the owner approves and implementation follows. Not a redesign, a rewrite, or new design.
---

# Map what to carry over from a reference codebase

When the mission or the owner names a codebase as a reference, the work is appropriation, not invention. The reference's code is the established behavior and look. Before contracts are written or code is assigned, identify every piece of the reference the work needs and say exactly how it is carried over. New code is the exception, and each piece of it needs the owner's approval.

## When it runs

The technical planner runs it before SYSTEMS for an increment a reference codebase covers, and again before each implementation slice whose scope reaches further into that codebase. It is not needed when no reference codebase is named, or for a fix inside code already carried over.

## Find what implements the scope

For each behavior in the approved scope and each element of the views in INTERFACE, find what implements it in the reference: files, types, functions, constants such as colors, sizes, spacing, and fonts, assets such as icons and fonts, and the tests that cover it. Read the source to be exact, and name paths and symbols. Do not work from memory or from documents that describe the code.

## Classify each piece

- **Move:** carried over unchanged.
- **Move and edit:** carried over with named edits. An edit is allowed only where the new structure requires it, for example a call that now crosses a process or crate boundary. The look, wording, and behavior stay as they are.
- **Wrap:** used in place through a thin adapter, only when moving it is not possible.
- **Leave:** in the reference and not needed for this scope.
- **New:** the scope needs it and the reference has nothing for it. Give the reason for each item.

Moving is the default. Rewriting something the reference already does, even more cleanly, is not one of these options. A look or behavior different from the reference is a scope change for the owner, not a planning choice.

## Write the reuse map

Adapt [the template](templates/REUSE.md). Honor an existing location; otherwise save `<target>/.project/documents/development/<version>/reuse.md`, with `v1.0` for a new project. Update it in place for later slices. Name the reference's path and commit. Group pieces by destination, each as source path to destination path with its classification and any edits. Put the New list last, each item with its reason. Plain prose and short lists, no tables.

## Approval and use

The coordinator brings the map to the owner, New list first. Implementation of a slice starts only after the owner approves the map for that slice. An implementer who finds a need that is not on the map returns it to the coordinator; it is not built on the spot.

Implementers copy each moved file unchanged into its destination first, then make only the mapped edits, so a diff against the source shows exactly those edits. Code review checks the change against the map and the source.

## Finish

Return the map's path, the New list, and anything in scope whose implementation could not be found in the reference, then stop. Do not write SYSTEMS contracts beyond what the assignment asks, or start implementation.
