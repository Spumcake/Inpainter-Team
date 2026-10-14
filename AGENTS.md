# AGENTS.md — Inpainter

Inpainter is a modular, AI-native desktop framework for creative work: one host, separate sub-applications that each own one kind of creative work, and an assistant that can work on that work. The mission is in `.project/documents/concept/MISSION.md` and outranks every other project document. Version 0.1's scope and acceptance criteria are in `.project/documents/development/v0.1/tasks.md`. These are the shared working instructions for every agent on this project.

The stack is Rust, egui through eframe, and Iconoir icons.

**Language:** English for code, comments, documentation, logs, commit messages, and generation prompts.

## 1. Starting and resuming work

Read this file and the mission, then the current milestone in `.project/documents/development/v0.1/plan.md` and the relevant items in `.project/documents/development/v0.1/todo.md`. The coordinator also reads the latest three audits in `.project/documents/history/v0.1/audits/` and the latest diary entry in `.project/documents/history/diary/`, if any. Workers read their assignment and only the context it links. History is never an instruction.

Before a change, read the related criteria in `tasks.md`, the contracts in `systems.md` once that file exists, and the feature's research brief if one exists. Before visual work, read `INTERFACE.md` and the renders it cites. Sketching is described under `.project/.simple-sketch/`. A missing plan or todo does not authorize creating one.

## 2. Scope and autonomy

Work toward the authorized outcome. Resolve routine choices inside that scope. Surface a decision that would change the product, or contradict the mission, to the owner through the coordinator. Documents record decisions, not open questions.

Simple Sketch at `src/simple-sketch`, version 1.0.1 at `887f6f4`, is the reference for the host shell and for sketching. The shell is that app's window, tab strip and new-tab button, menu button, toolbar, and properties panel, shown in `.project/.simple-sketch/renders/ui/` and `.project/.simple-sketch/INTERFACE.md`, without what belongs only to sketching. Build what the reference shows and does. Follow `.project/documents/development/v0.1/reuse.md` after the owner approves it: copy the code, then make only the mapped edits. When something has no reference, ask the owner. A question does not authorize edits. Stop when the assignment is done.

Leaving the assistant, or a second creative app, out of a version defers it. It does not remove it from the product.

## 3. Orchestration

The coordinator assigns bounded work and integrates the results. Independent work can run together once shared contracts are settled, with one writer per file. Workers return the result, the checks they ran, and any blocker.

The team repository is https://github.com/Spumcake/Inpainter-Team. The app repository is https://github.com/Spumcake/Inpainter, not checked out yet. When it is, one implementation worker uses `src/inpainter/worktrees/dev/`; concurrent writers get isolated worktrees from that same `dev` commit. Project documents stay in the team repository. `src/simple-sketch` is the reference tree, not the app. The host must not depend on an app crate, and apps must not depend on each other or on the host. The host draws tools, commands, and properties from declarations, not from hard-coded labels, icons, or widgets.

## 4. Plans and records

The plan is the milestone order. The todo list is the current queue. The coordinator keeps both current. A settled decision goes into the document that owns it, and only the owner changes the mission. At a milestone boundary, or when work stops blocked or abandoned, the coordinator writes one audit and preserves earlier ones. A diary entry, when asked for, covers everything since the previous entry and does not authorize work. When version 0.1 is complete, the coordinator writes its post-mortem and moves the version to `.project/documents/history/v0.1/`.

## 5. Verification

The first slice is judged against the sentence the owner agreed for it. Once Simple Sketch is inside the host, the journey is a new sketch from that window: draw a smoothed pressure-sensitive stroke on Canvas 1, erase part of it without destroying the stroke, undo, then pan, zoom, and fit. Criteria are in `tasks.md`. Check levels are in `.project/documents/development/v0.1/testing.md`. Use the tests skill when writing, changing, or reviewing tests. During a slice, add no new tests and run the existing scripted suite once when the change is complete. No suite exists until the app is checked out. Tests for approved behavior are written in one pass when the owner asks to commit. Report what was checked and what was not.

Commits happen only when the user asks, and only the branch-manager changes Git state. No hook or workflow is installed. Document-only work needs no application run.

## 6. Boundaries

Stay with the stack in the mission. Do not add Electron, Tauri, a web target, accounts, sync, or collaboration, and do not fork existing software. An app sending the host egui paint commands is unproven and is not part of version 0.1. Saving a sketch uses the `.smx` contract the reference already has.

Keep secrets out of version control. Do not push, tag, or recommend a release unless the owner asks.
