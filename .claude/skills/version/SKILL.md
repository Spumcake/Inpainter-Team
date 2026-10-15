---
name: version
description: Open a version of the product (its folder, tasks, systems, and plan, and a new project's AGENTS.md and CLAUDE.md), keep its plan current, and close it with a post-mortem. Use when the owner starts a project or a version, asks for planning documents, or says a version is finished.
---

# Open, run, and close a version

A version is one step toward the mission. Its working documents live in `.project/documents/development/<version>/`. A project that starts from nothing works toward `v1.0`, the full realization of its pitch, unless the owner says otherwise. Honor a project's existing paths and documents.

## Starting a project

When the team repository has no AGENTS.md, write one from [the template](templates/AGENTS.md), with only established facts: aim for 300 to 600 words, plain prose, no tables. Add a CLAUDE.md at the root holding two lines, `@AGENTS.md` and `@.claude/development.md`, so every session loads both. The mission comes before everything else; draft it with the **mission** skill.

## Opening a version

Write only what the owner has asked for, in this order, because each depends on the one before:

1. **Pitch,** with the **pitch** skill, when the owner wants one.
2. **References:** INTERFACE views from approved images (the **interface** skill), and a reuse map (the **appropriation** skill) when a reference codebase covers the work. Research with the **journeys** skill only for features no reference covers and only when the owner wants it.
3. **tasks.md,** from [the template](templates/TASKS.md): scope, acceptance criteria, how to run it, and how each criterion is checked. Criteria describe the journeys a person completes, not only data rules.
4. **systems.md,** from [the template](templates/SYSTEMS.md), when the work has boundaries or contracts worth fixing before code: one owner for each important state, and the checks that would catch the mission's fakes.
5. **plan.md,** from [the template](templates/PLAN.md): the milestones in order, and at the top a short "Now" list of the next three to seven actions.

Run the **intent-reviewer** agent on the reuse map, systems, and the first slice's criteria before the owner approves them. Bring its verdict along with the documents.

Write plain prose and short lists, no tables. Do not invent decisions to fill a template; ask the owner instead.

## Keeping the plan current

Update plan.md in place as work starts, finishes, or gets blocked. Check an item only when its result is achieved. When the work departs from tasks, systems, or the reuse map, add a short "As built" section to that document saying what was done instead and why. Git keeps the history; do not copy documents into archives before editing them.

## Closing a version

When the owner says the version is finished, write `.project/documents/history/<version>/post-mortem.md`:

- How long it took. Take the start from the first diary entry or commit for the version, and the end from the current UTC time. Never invent either.
- What was built, what the owner decided, and what stayed outside the version, linking the pitch and tasks.
- What went wrong and what was learned. Name who confirmed the version was complete.

Usually 300 to 600 words. Then move the version's folder from `development/` into `history/<version>/`, fix links the move breaks, and report any other document that pointed at the old paths.
