---
name: mission
description: Draft a project's MISSION.md, or propose a change to it, for the owner's approval. The mission is the product's lasting identity across versions and outranks every other project document. Use when a project starts, when the owner asks for a mission, or when the owner wants an idea added to it.
---

# Draft or change the mission

MISSION.md says what the product is, independent of any version. Every other document must agree with it. A version decides what it delivers; the mission decides what the product is.

It lives at `.project/documents/concept/MISSION.md`, one file.

## Draft from the owner's words

Work from what the owner has said: the conversation, a founding brief, or diary entries they point to. Adapt [the template](templates/MISSION.md). Keep the owner's phrasing where it is clear. Do not research, invent goals, or add features. Where the owner's words leave something consequential open, ask.

- Name each reference the owner has given, a codebase or an image, with its path and what it is the reference for.
- For the few things the product exists to prove, write what it is, the fake that would pass every check while missing the point, and the test that tells them apart. The intent reviewer judges work against these.
- State lasting structure concretely enough to check, such as the regions an interface always shows or the boundary between two programs.
- A part a version leaves out is still part of the product; say so where it matters.

Plain prose and short lists, at most about 800 words. Leave out version scope, schedules, implementation details that may change, and history.

## Approval

Show the owner the full draft or, for a change, the exact lines added, removed, or replaced, with one sentence on why. Save only after the owner explicitly agrees, and save exactly what they agreed to; if they edit the text, their text is what was agreed. Ideas from discussions, diary entries, or your own suggestions stay proposals until then. Never change the mission to make a version or an implementation fit.

A change can make version documents wrong. List the ones that now conflict, and fix them only as a separate step the owner agrees to.
