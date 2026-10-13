---
name: post-mortem
description: Write post-mortem.md when a version's work is complete. Summarize the development and how long it took. Use when the owner says a version is finished, or when closing version 1.0 as the full realization of the pitch.
---

# Write the post-mortem

Write `post-mortem.md` when a version's work is complete. It is a summary of the development and how long it took. The coordinator writes it. Do not write one for a single slice, a diary request, or an audit.

## Where it goes

`<team>/.project/documents/development/<version>/post-mortem.md`

A project that starts from nothing uses `v1.0` unless the owner stated otherwise. Version 1.0 is the full realization of the pitch. Do not overwrite an existing post-mortem.

## How long it took

Take the start and the end from the records and the environment. The start is the earliest of the first diary entry, the first audit, and the first commit in the team or app repository for this version. The end is the actual UTC time when the version is closed. State the span in ordinary language. Do not invent a start, an end, or a duration.

## What to summarize

Use the pitch, the task list, the audits, the diary, and the commit history. Cover what was built, what the owner decided, and what stayed outside the version. Link the pitch and the task list. Do not copy the audits. Usually 300–600 words. Name who confirmed the version was complete. An implementation by itself does not close the version.
