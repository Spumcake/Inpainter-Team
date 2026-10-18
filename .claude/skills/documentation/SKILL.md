---
name: documentation
description: Decide where a document belongs and who may change it, and write a project's overview, its guides, and its public documentation such as the README. Use before creating or moving a document, when a project needs an overview or an intent guide, when lasting knowledge needs a guide, and when documentation goes into a public repository. Not the mission (the mission skill) or a version's working documents (the version skill).
---

# Place and write documentation

Every fact has one home, and other documents link to it. A document's home follows from who reads it and how long it stays true.

## Where documents live

**In each project's own repository,** public, written for someone who opens that repository alone:

- `README.md`: what the project is and its scope in a few paragraphs, drawn from its overview, then how to build, run, and install it.
- `docs/overview.md`: what the project is in detail, in every version.
- Further documentation under `docs/` as the project grows, such as how to use or extend it, written from what is built, never ahead of it.
- The repository's own AGENTS.md and developer skills.

**In the team repository,** for developing the effort:

- the team's own documents in `.project/documents/`: the mission, `MISSION.md`, and the diary, `diary/`;
- for each project, in `.project/projects/<project>/documents/`: its guides in `guides/<subject>-guide.md`, the version in progress in `development/<version>/`, and its history in `history/<version>/`. Material that is not a document, such as presets, workflows, or scripts, sits beside `documents/` in the project's folder;
- INTERFACE.md, renders and other references, and AGENTS.md.

When a project's code lives inside the team repository, its README and `docs/` sit at the team repository's root. A project with no repository of its own, such as a set of plans, keeps its overview at `documents/overview.md` in its folder.

A public document holds no owner conversations, open questions, unfinished decisions, costs, or secrets. Its links resolve for someone reading that repository alone: link other repositories by their public address, and when a reference lives only in the team repository, say what the reader needs instead of linking it.

## Authority

1. The mission outranks every document.
2. A project's overview outranks every other document about that project, and agrees with the mission.
3. Standards guides, such as the intent guide, hold what the owner approved; version documents meet them.
4. Version documents decide what one version delivers.
5. Findings guides and research briefs are evidence; the owner decides scope from them.
6. History explains how the project got here and is never an instruction.

The mission, each overview, and each standards guide are the owner's, and follow the approval rule in development.md. A change to one can make other documents wrong: list them, and fix them only as a separate step the owner agrees to.

## The overview

Adapt [the template](templates/OVERVIEW.md). It says what one project is, independent of any version: what it is and for whom, where it is going, the lasting structure every version keeps, its references, what it will never be, and the words every agent must use the same way.

Work from the owner's words and the mission. Do not research, invent goals, or add features; ask where the owner's words leave something consequential open. State lasting structure concretely enough to check, such as the regions an interface always shows or the boundary between two programs. A part a version leaves out is still part of the project. Plain prose and short lists, at most about 1,000 words. Leave out version scope, schedules, implementation details that may change, and history. The overview is public, so a stranger should understand the project from it.

## Guides

A guide holds lasting knowledge about one subject that outlives any version. Adapt [the template](templates/GUIDE.md). Its opening says which kind it is:

- **Standards:** what the owner approved, which work is judged against. Only the owner changes them.
- **Findings:** what research or experience established, each claim with its source and the date. Findings are suggestions, never scope. Update them in place when newer evidence replaces older, and say what changed.

**The intent guide,** `documents/guides/intent-guide.md` in each project's folder, is the standards guide the intent reviewer judges that project's work against. Adapt [its template](templates/INTENT-GUIDE.md): three to eight things the project exists to prove, each with what it is, the fake that would pass every check while missing the point, and the test that tells them apart. Write it with the project's overview, from the owner's words, and ask where they leave a point open.

## Before saving

Check that the document's home matches its readers and how long it stays true, that nothing in it is already said elsewhere, and that its links resolve for its readers. Report any document that now disagrees with it.
