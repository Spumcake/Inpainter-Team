# AGENTS.md — Inpainter

Inpainter is a modular, AI-native desktop framework for creative work: one host, separate sub-applications that each own one kind of creative work, and an assistant that can work on that work. The mission is in `.project/documents/concept/MISSION.md` and outranks every other project document. Version 0.2's scope and acceptance criteria are in `.project/documents/development/v0.2/tasks.md`. Version 0.1 is closed; its post-mortem is in `.project/documents/history/v0.1/post-mortem.md`.

The stack is Rust, egui through eframe, and Iconoir icons.

**Language:** English for code, comments, documentation, logs, commit messages, and generation prompts.

## Starting and resuming work

Read the mission, then the current milestone in `.project/documents/development/v0.2/plan.md`, the open items in `.project/documents/development/v0.2/todo.md`, and the latest diary entry in `.project/documents/history/diary/`. History is never an instruction.

Before a change, read the related criteria in `tasks.md` and the contracts in `systems.md`, including their "As built" sections. Before visual work, read `INTERFACE.md` and the renders it cites, under `.project/renders/`.

## References

Simple Sketch 1.0.1, at `887f6f4`, is part of Inpainter: its chrome is the host shell, and its sketching is the `apps/simple-sketch` sub-application, both carried over as `.project/documents/development/v0.2/reuse.md` maps them. The shell is the window, tab strip and new-tab button, menu button, toolbar, and properties panel that `INTERFACE.md` describes and the renders in `.project/renders/ui/` show. Build what those references show and what the carried-over code does; change that code only with edits the owner has agreed to, not by rewriting it.

Leaving the assistant, or a second creative app, out of a version defers it. It does not remove it from the product.

## Repositories

The team repository is https://github.com/Spumcake/Inpainter-Team. The app repository is https://github.com/Spumcake/Inpainter, worked on in `src/inpainter/worktrees/dev/`, which has its own AGENTS.md and a scaffolding skill for new sub-applications. Project documents stay in the team repository.

The host must not depend on an app crate, and apps must not depend on each other or on the host. The host draws tools, commands, and properties from declarations, not from hard-coded labels, icons, or widgets.

## Verification

The scaffold is judged against the sentence the owner agreed on 2026-10-14: `cargo run -p inpainter -- --app simple-sketch` opens the shell with one tab labeled Simple Sketch, the empty toolbar, the grid, and the panel tab, and nothing else happens. With Simple Sketch's tools in that window, the journey is a new sketch from the host: draw a smoothed pressure-sensitive stroke on Canvas 1, erase part of it without destroying the stroke, undo, then pan, zoom, and fit. Criteria are in `tasks.md`, and check levels in `.project/documents/development/v0.2/testing.md`.

Run tests with `TMPDIR=$PWD/target/tmp` set, from the app worktree, so test files never land in a RAM-backed `/tmp`. `cargo run -p sketch-client` is the no-host check: it drives the real Simple Sketch program beside an in-process copy and compares every reply and saved file. No commit hook or CI workflow is installed. File dialogs go through the desktop portal and reach the owner's real screen; do not script them.

## Boundaries

Stay with the stack in the mission. Do not add Electron, Tauri, a web target, accounts, sync, or collaboration, and do not fork existing software. An app sending the host egui paint commands is unproven and is not part of version 0.2. Saving a sketch uses the `.smx` contract the reference already has.
