---
name: diary
description: Write a developer diary entry covering all project work since the previous entry and the next steps from the current discussion. Use when the owner asks for one, or accepts the offer of one at the end of a working session.
---

# Write a developer diary entry

The diary is the project's running story, one stream across projects and versions in `.project/documents/diary/`. Each entry picks up where the previous one stopped, records what was done and why, and ends with the next steps. Someone reading the folder from start to end can follow the project without the chat history.

Write one when the owner asks, or when they accept an offer at the end of a session that changed the project. Not on a schedule, and not for a session with no project work.

## Gather what happened

Find the latest entry; its timestamp starts this entry's span. With no previous entry, cover the project from its start, briefly, and say so. Use sources that already exist: this conversation, document changes since then, and each repository's history since then, with its current commit and whether it has uncommitted changes. Do not rerun tests or relaunch the app to fill the entry. When part of the span is known only from documents, say so. Never invent work, measurements, or approvals.

## Write it

Adapt [the template](templates/DIARY.md). Get the current UTC time from the environment and save `YYYY-MM-DDTHH-mm-ssZ-short-topic.md`. Never overwrite or edit an earlier entry; link the previous one, and put corrections in the new one.

Write in plain past tense, as a developer would, with short lists and no tables. Say who did the work: the owner, Claude Code, or another tool. Separate what was observed from what was intended. Usually 200 to 500 words.

End with the next steps from the conversation, marking each as agreed, proposed and awaiting the owner, or blocked and on whom. Ideas the owner has not adopted are proposals; an entry never adds anything to the mission, an overview, a guide, or a version's scope. When an entry is mostly such a discussion, say so in its first lines.

Do not update other documents from a diary entry. Return the path.
