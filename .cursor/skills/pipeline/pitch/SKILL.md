---
name: pitch
description: Write a short, public-readable pitch for one version of a product from a few sentences of intent, covering the app's solutions, concrete use, reference-product lessons, and presentation, consistent with the project's mission. Not the mission, architecture, project administration, or a research report.
---

# Write a product pitch

Turn the user's brief into a clear explanation of the app. Write for an ordinary person considering the product, not for agents managing a project. Use [the template](templates/PITCH.md). Honor the explicit destination or existing canonical pitch. Otherwise save `<target>/.project/documents/development/<version>/PITCH.md`. A project that starts from nothing is working toward version 1.0, folder `v1.0`, unless the owner states otherwise. Version 1.0 is the full realization of its pitch.

## The mission comes first

When `.project/documents/concept/MISSION.md` exists, the pitch explains this version of the product the mission describes: what the version delivers toward it, and what it leaves for later. Describe the product as the mission does. Do not narrow, rename, or redefine it to fit the version's scope; something the version leaves out is named as later work, not removed from the product. If the brief conflicts with the mission, return the conflict to the caller instead of choosing. The mission is the owner's; this skill never writes or edits it.

## Strict scope and size

Produce only the requested pitch. Aim for 450–650 words; never exceed 700 unless the user explicitly requests a longer document. Shorter is fine. Do not generate other specifications, audits, mockups, implementation, or research notes.

Start with the product and its value. No status, draft/version banner, ownership, decision authority, timestamps, approval disclaimers, label legends, tables, or administrative metadata. No architecture, schemas, stack, roadmap, testing plan, deployment procedure, prerequisite matrix, or inventory of unknowns. Do not repeatedly qualify ordinary sentences with Fixed/Proposed/Open labels.

Use natural language, short paragraphs, and at most a few short bullets. Do not turn the brief into elaborate fictional scenes or repeat the same requirements under multiple headings. Do not invent user circumstances, existing files, research findings, or quantitative results.

## Inputs and reference research

Read the mission when it exists, the user's brief, this template, and explicitly relevant supplied material. Do not inspect Git, scan the codebase, search archives/session history, or inventory the workspace. Check only whether the destination or a supplied input exists when necessary. Use an existing pitch only to preserve actual accepted intent, not its verbosity or unsuitable structure.

Research only reference products relevant to the brief. Prefer the named product's official overview and one relevant feature page. Maximum for the entire pitch: **one search query and three fetched pages total**, including retries and pages in batched fetches. Stop sooner once one useful adaptation is supported. Do not open PDFs, manuals, release-note collections, related-product lists, or recursively follow links. A failed lookup does not restart the budget. If the user supplies adequate source material, use it instead of browsing.

Explain the verified mechanism and the proposed adaptation; do not merely name a competitor. Link the supporting source briefly beside the claim. When access fails or the budget is exhausted, finish using supported information and mention the research limitation briefly in the handoff, not as a long disclaimer inside the pitch. Never invent the missing claim or quietly extend research.

## Substance

- **Solutions:** What the app lets someone accomplish, what practical problem each main capability solves, and the visible result that makes it useful. Write about the product, not measurement machinery.
- **Use:** A few short action → app response → result examples. For example: “Search the sticker ID, select the borrower and due date, and confirm the loan. The tool now shows who has it and when it is expected back.” Avoid invented room conditions, lengthy backstories, or a catalogue of every edge case.
- **Reference lessons:** What the relevant part of an existing app does, how it helps, and what this app should adapt or simplify. One focused paragraph can be enough; a feature catalogue is not useful.
- **Presentation:** What is visible first, how primary actions are reached, how users distinguish important states, and the relevant interaction/visual qualities. Use concrete descriptions rather than “modern and intuitive.” Leave detailed layouts to INTERFACE.md. When a visual reference exists, describe what it shows; do not invent presentation that INTERFACE would then have to follow.

Conclude with this version's boundary: what it delivers toward the mission, what it leaves for later, and supplied constraints. Express unaccepted ideas naturally as suggestions. Do not invent agreement. If a missing decision prevents a coherent pitch, return that specific question to the caller; when asked to draft with unknowns, leave the behavior open in one sentence rather than enumerating every unspecified detail.

## Finish and stop

Check once for length, repetition, and unsupported claims; shorten before saving if necessary. Save the pitch, return its path and at most a few relevant limitations, and stop. No follow-on research or optional improvement cycle. Skills do not expand the invoking role's authority; if pitch creation is outside that role, it must route or decline rather than use this procedure itself.
