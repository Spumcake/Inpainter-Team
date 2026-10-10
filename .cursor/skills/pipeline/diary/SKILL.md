---
name: diary
description: Write a developer diary entry covering all project work since the previous entry and the next steps intended in the current discussion. Coordinator-owned; written when the user asks or accepts an offer at the end of a working session.
---

# Write a developer diary entry

The developer diary is the project's running story. Each entry picks up where the previous one stopped, records what was done and why, and ends with the next steps the conversation settled on or proposed. A reader who opens the diary folder can follow the project from one entry to the next without the chat history.

The diary is not an audit. An audit is a brief evidence checkpoint at a milestone boundary or a blocked effort. The diary covers all work in between, including reviews, small fixes, discussions that changed direction, and decisions still open. Link audits rather than repeating their evidence.

## When to write one

Write an entry when the user asks for one. At the end of a working session that changed the project, the coordinator may offer one in a single line if there is work since the latest entry; write it only if the user accepts. Do not write entries on a schedule, per worker, or per edit, and do not write one for a session with no project work.

## Gather what happened since the previous entry

Find the latest entry in `.project/documents/development/diary/` unless the project already has a canonical location. Its timestamp is the start of this entry's span. With no previous entry, cover the project from its start, briefly, and say so.

Use only sources that already exist:

- the current conversation, for what was asked, done, found, and decided;
- audits written since the previous entry;
- changes to PLAN, TODO, TASK, SYSTEMS, INTERFACE, and testing records since then;
- team-repository and app-repository history since that time, and each repository's current commit and dirty state.

Do not rerun tests, relaunch the app, or reopen investigations to fill the entry. Report what the sources show. When an earlier part of the span is known only from documents, say that the entry relies on them. Do not invent work, measurements, or approvals.

## Record intended next steps

Close the entry with the next steps from the conversation. Distinguish what the user agreed to, what was proposed and is awaiting a decision, and what is blocked and on whom. Name decisions that belong to the user. Recording a next step does not authorize it; TODO stays the work queue.

## Write the entry

Adapt [the template](templates/DIARY.md). Obtain the actual current UTC time from the environment and create:

`<team>/.project/documents/development/diary/YYYY-MM-DDTHH-mm-ssZ-short-topic.md`

Create the folder when needed. Use a numeric suffix on collision; never overwrite or edit an earlier entry. Link the previous entry. A correction to an earlier entry goes in the new one.

Write in plain past tense, first person plural or neutral, as a developer would. Use natural sentences and short lists, no tables. Name who did the work: the user, the coordinator, named roles, or a single agent. Separate observed results from intentions. Usually 200–500 words; a long span may need more, a short one less. Exclude secrets and unnecessary personal data.

## Finish

Do not update PLAN, TODO, or other documents from a diary entry; their owners keep them current. Return the entry's path and stop.
