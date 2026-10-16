# AGENTS.md — Inpainter

Inpainter is a modular, AI-native desktop framework for creative work: one host, separate sub-applications that each own one kind of creative work, and an assistant that can work on that work. The mission, in `.project/documents/concept/MISSION.md`, says what the whole effort is: the open-source software and the closed platform. What the software is in detail is in its overview, `docs/overview.md` in the app repository, and the fakes it is judged against are in `.project/documents/guides/intent-guide.md`. The mission outranks every other project document, and the overview and the intent guide outrank every other document about the software. Only the owner changes these three. Versions 0.1, 0.2, and 0.3 are closed; their post-mortems are in `.project/documents/history/<version>/post-mortem.md`. No later version is open yet. The next one opens with the version skill, in `.project/documents/development/<version>/`.

The stack is Rust, egui through eframe, and Iconoir icons.

**Language:** English for code, comments, documentation, logs, commit messages, and generation prompts.

## Starting and resuming work

Read the mission, the overview, and the intent guide, then the open version's plan and todo in `.project/documents/development/<version>/` when one is open, the latest post-mortem, and the latest diary entry in `.project/documents/history/diary/`. History is never an instruction.

Before a change, read the related criteria in `tasks.md` and the contracts in `systems.md`, including their "As built" sections. Before visual work, read `INTERFACE.md` and the renders it cites, under `.project/renders/`.

## References

Simple Sketch 1.0.1, at `887f6f4`, is part of Inpainter: its chrome is the host shell, and its sketching is the `apps/simple-sketch` sub-application, both carried over as `.project/documents/history/v0.2/reuse.md` mapped them. The shell is the window, tab strip and new-tab button, menu button, toolbar, and properties panel that `INTERFACE.md` describes and the renders in `.project/renders/ui/` show. Build what those references show and what the carried-over code does; change that code only with edits the owner has agreed to, not by rewriting it.

Leaving the assistant, or a second creative app, out of a version defers it. It does not remove it from the product.

## Repositories

The team repository is https://github.com/Spumcake/Inpainter-Team. The app repository is https://github.com/Spumcake/Inpainter, worked on in `src/inpainter/worktrees/dev/`, which has its own AGENTS.md and a scaffolding skill for new sub-applications. The strategies repository is https://github.com/Spumcake/Inpainter-Strategies, worked on in `src/inpainter-strategies/worktrees/dev/`. It holds the assistant's strategies, and its runtime prompt once that moves out of the code, as the owner set on 2026-10-16. Project documents stay in the team repository, except the software's overview.

The host must not depend on an app crate, and apps must not depend on each other or on the host. The host draws tools, commands, and properties from declarations, not from hard-coded labels, icons, or widgets.

## Verification

Version 0.3 was judged against the sentence the owner agreed on 2026-10-15, in `.project/documents/history/v0.3/tasks.md`. The chat panel sits on the left. The assistant does on the open sketch anything a person can do to its contents by hand, while the tools wait, and one undo takes a request back. The template's skill proves the assistant learns only from declarations. A workspace saved from the logo menu reopens with the same tabs and chat. The owner tried it with real models on 2026-10-16 and closed the version; its post-mortem says what was not tried.

Run tests with `TMPDIR=$PWD/target/tmp` set, from the app worktree, so test files never land in a RAM-backed `/tmp`. Run `cargo build` before `cargo test`: the host's checks start the built programs and use a scripted stand-in for OpenRouter, so they spend nothing. `target/debug/sketch-client` is the no-host check: it drives the real Simple Sketch program, including every skill tool, beside an in-process copy, and compares every reply and saved file. The assistant reads `OPENROUTER_API_KEY` from the environment; each real request costs money, so make real model calls only when the owner asks. No commit hook or CI workflow is installed. File dialogs go through the desktop portal and reach the owner's real screen; do not script them.

## Boundaries

Stay with the stack in the overview. Do not add Electron, Tauri, a web target, accounts, sync, or collaboration, and do not fork existing software. An app sending the host egui paint commands is unproven and has not been built. Saving a sketch uses the `.smx` contract the reference already has.
