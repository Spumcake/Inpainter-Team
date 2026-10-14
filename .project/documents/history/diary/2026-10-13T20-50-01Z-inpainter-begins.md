# Inpainter begins

**UTC:** 2026-10-13T20:50:01Z
**Covers:** the project from its start. This is the first entry.
**Repositories:** The team repository is on `master` at `fe2ad92` (Organize version 1.0 documents together and close the version with a post-mortem.), the same as its `origin`, which still points to `simple-sketch-team`. The working tree has uncommitted changes: the Simple Sketch documents removed from `.project/documents/` and archived under `.project/.simple-sketch/`, and this entry. The app `src/simple-sketch` is a plain clone of `simple-sketch` on `main` at `887f6f4` (Package Simple Sketch 1.0.1 as an AppImage and an installable tarball.), tagged `v1.0.1`, the same as `origin/main` and `origin/dev`, and clean. `src/inpainter/worktrees/` is empty; there is no Inpainter repository yet.

## What we did

This codebase is a fork of [simple-sketch-team](https://github.com/Spumcake/simple-sketch-team), the orchestration pipeline built for [Simple Sketch](https://github.com/Spumcake/simple-sketch). Development of Simple Sketch concluded with version 1.0, released as 1.0.0 and repackaged as 1.0.1 for Linux installation. The owner forked the team repository and archived its documents under `.project/.simple-sketch/`, with the renders, the version 1.0 pitch, systems, tasks, audits, diary, and [post-mortem](../../../.simple-sketch/documents/development/v1.0/post-mortem.md). This version starts from a clean set of documents. Claude Code examined the codebase and wrote this entry.

## What Inpainter is

The goal of this fork is to evolve the foundation established by Simple Sketch into a desktop-class agentic harness. It should be able to read, understand, and modify digital artifacts managed by modular, custom-built sub-applications residing within the same host. Under this architecture, Simple Sketch becomes one of many supported sub-applications rather than the central focus of the platform.

This new effort is called Inpainter, a modular, AI-native framework for building apps for creative use cases. The host application provides the shared interface shell:
- window management;
- tab navigation;
- a persistent chat assistant in a left-hand panel, about one quarter of the window's width.

Apps declare their tools, commands, properties, views, and agent skills as data through one protocol. The host draws simple UI from those declarations with egui. An app can also draw its own custom view by running egui itself and sending the host what to paint.

The framework should lay the foundation for more sub-applications that the agentic harness uses to carry out a sequence of tasks across different apps. For example, a user asks the chat assistant for a story with pictures. It writes a short story in a writing app, turns the story's scenes into image prompts, and splits the resulting images into layers in a paint program.

There are many applications, but the user only ever launches the host. The host starts sub-applications for the user. There is one sub-application process per app, which serves every tab of that app.

## Constraints

Keep Rust, egui, and Iconoir. No Electron, Tauri, or web target. Don't fork existing software, but reuse good components.

No monolith: the code structure enforces the boundaries, not just the documents. That means separate crates and programs, and a host that cannot import an app.

Simple Sketch keeps working throughout. No second app comes before Simple Sketch runs well inside the host.

## How the pieces divide

These are the positions reached in the discussions recorded in the archived diary:
- **Live drawing stays in the host.** Drawing, erasing, shapes, text, and ordering are fundamentals that many apps can reuse. A shared drawing library holds them, and the host uses it to edit live, so pen input never crosses between processes.
- **An app owns its arrangement and its saving.** Simple Sketch arranges drawings as one infinite canvas and saves them as `.smx`. A future animatic app would arrange drawings in frames on a timeline. Each app owns its own format.
- **Undo lives with the app,** because each app has its own opinions about it. The host sends each finished edit to the app. Undo and redo are app commands, and the app returns the reverse edit for the host to apply.
- **Two kinds of UI.** Declared UI (tools, icons, commands, menus, panel sections) is data that the host draws. A custom view, such as a timeline, is drawn by the app.
- **The protocol works without the host.** An app can run headless and be driven entirely through the protocol, which is how tests and, later, agents use it.
- **Crashes are contained.** If an app crashes or hangs, the host stays up and can restart it.

## What we learned from the code

Simple Sketch 1.0.1 is one Rust crate of about 24,700 lines, with 120 unit tests. It depends on eframe and egui 0.36.2, `perfect_freehand`, `i_overlay`, `earcutr`, `kurbo`, a patched `octotablet` for Wayland pen pressure, `iconflow` for Iconoir, `rfd` for file dialogs, and `serde`.

The 1.0 work left a real seam between the core and the window:
- **No egui:** `session.rs`, `document.rs` with its snapshot, `ink.rs`, `schema.rs`, `registry.rs`, `defaults.rs`, `prefs.rs`, and `store.rs`.
- **egui:** `canvas.rs`, `tabs.rs`, `toolbar.rs`, `panel.rs`, `outliner.rs`, `widgets.rs`, `text.rs`, `tablet.rs`, `dialogs.rs`, and `main.rs`.

Text is measured through egui at paint time, so text layout touches both sides.

The session, at about 7,300 lines, is where the split is hardest. It mixes state the host would own (tabs, the current tool and its settings, the panel, the clipboard) with state the app would own (each sketch's tree, selection, camera, and history), and calls the `.smx` store directly.

## Where things stand

Nothing is built for Inpainter yet. The root `AGENTS.md` and `INTERFACE.md` still describe Simple Sketch and point to version 1.0 paths that are now archived. There is no version 0.1 pitch, systems, tasks, plan, or todo.

## Next steps

We are working towards version 0.1.0. It successfully clones and partitions Simple Sketch into two builds: the Inpainter host, and the Simple Sketch sub-application.

The first milestone is the split itself: the same Simple Sketch, now running as an app inside the host, plus a test that drives it through the protocol with no host running. Treat the custom-view approach as unproven until a small experiment shows that scrubbing a frame strip feels smooth.

Recording these steps does not authorize them. The owner decides when to write the version 0.1 documents, how to update `AGENTS.md` and the team repository's remote, and where the Inpainter repository will live.
