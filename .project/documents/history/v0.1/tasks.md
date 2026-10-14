# Inpainter — version 0.1 delivery

**Sources:** [Mission](../../concept/MISSION.md), [pitch](PITCH.md), [interface](../../../../INTERFACE.md), and [systems](systems.md) when that contract exists. Sketching behavior that this version carries over is Simple Sketch 1.0.1, specified in [.project/.simple-sketch/documents/development/v1.0/tasks.md](../../../.simple-sketch/documents/development/v1.0/tasks.md) and [.project/.simple-sketch/INTERFACE.md](../../../.simple-sketch/INTERFACE.md). The shell's visual reference is the renders listed there, under `.project/.simple-sketch/renders/ui/`. The reference code is `src/simple-sketch` at `887f6f4`.

**Objective and exclusions:** Version 0.1 is the framework's foundation, as the owner re-scoped it on 2026-10-14. The person launches only the host. The host draws the common UI the mission names around a separate sub-application, talks to it through the protocol, and finds it by its manifest. The template is the smallest correct sub-application, and the Inpainter repository's scaffolding skill starts new ones from it. Simple Sketch as a sub-application moves to version 0.2, with the criteria listed under Deferred to version 0.2. The assistant and any second creative app are left for later versions. They are not removed from the product. An app drawing its own view by running egui and sending the host what to paint stays unproven and is not part of this version.

**Version convention:** 0.1 is the agreed working version, as the owner set on 2026-10-14. It is not the full product. Do not tag a candidate until the owner asks. After that, candidates use `0.1-candidate.<n>`.

## Planning and execution

Milestones are in [plan.md](plan.md). Current actions are in [todo.md](todo.md).

## Acceptance criteria

The shell and the process boundary are defined by the mission and by Simple Sketch's chrome. Sketching is defined by the Simple Sketch reference, not by a separate research brief.

H-1 Common UI. Starting with Inpainter not running, the person launches the host. The window shows the common UI for whatever sub-application is loaded, including one that declares nothing: the window, a tab strip with a new-tab control, a menu, and a tool area. The sub-application does not create or remove those areas. The properties panel is a shared component of the host, as the owner set on 2026-10-14. The host draws it, always the same way, only for a tab whose sub-application declares it. The sub-application declares whether a tab has the panel, which side it sits on, and what it holds. Only the right side has a reference, so only the right side is built. The grid is the same kind of shared component. The host draws them from the sub-application's declarations, and an empty declaration leaves the corresponding area without that app's tools, commands, or properties. Each tab is a document its sub-application reports, labeled with the name of the file it manages or, when it manages none, with the sub-application's name. The menu's contents are the commands declared by the sub-application of the selected tab. The title is always Inpainter, whatever tab is selected, and the application id is `io.github.spumcake.Inpainter`, as the owner set on 2026-10-14. The finished version loads Simple Sketch.

First slice, as the owner set on 2026-10-14: launching the host opens one window that matches [shell-demo.png](../../../renders/ui/shell-demo.png), titled Inpainter. The tab strip shows the menu button, one tab labeled Template, and the plus. The toolbar is empty. The contents area shows the shared grid, which the template declares. The template declares the properties panel on the right, so the panel tab opens and closes the empty panel column. The menu button, the plus, and the tab's close mark do nothing when clicked, and nothing else happens. The template is a separate program. Appearance follows the shell in [INTERFACE.md](../../../../INTERFACE.md) and the Simple Sketch chrome it cites.

H-3 Separate programs. The host and each sub-application are separate programs. The host does not import a sub-application, and sub-applications do not import each other. One process serves every tab of a sub-application.

H-5 Protocol without a host. The template is started with no host and driven entirely through the protocol: it sends its declaration, starting with `hello` and protocol version 1, and exits on `shutdown`. Tests hold the protocol's rules: a label with spaces survives, a declaration without `hello` and version 1 fails, and a manifest with a bad id, a path as its executable, or another protocol version is refused.

H-6 Sub-applications by manifest. The host finds each sub-application by its `apps/<id>/manifest.json` and starts the one `--app <id>` names or, without `--app`, the first by id, as the owner set on 2026-10-14. The Inpainter repository's `skills/scaffolding/` copies the template to start a new sub-application, and the repository's `AGENTS.md` points to it.

## Deferred to version 0.2

The owner re-scoped version 0.1 on 2026-10-14 and moved these to version 0.2, which makes Simple Sketch a sub-application. They are written here as they stood.

H-2 Sub-application failure. With the host showing a sub-application, that program exits or stops responding. The host window stays open and shows the sub-application's status. The person can still use the host's common UI.

H-4 Simple Sketch inside the host. The person launches the host and sketches. The sketching behavior of Simple Sketch 1.0.1 still holds, criteria SS-1 through SS-10 in the Simple Sketch delivery document, including a smoothed pressure-sensitive stroke on Canvas 1, erasing part of it without destroying the stroke, undo, and pan, zoom, and fit. They never launch Simple Sketch itself. Drawing, erasing, shapes, text, and ordering run in the host so pen input does not cross processes. The Simple Sketch sub-application owns the document: how the sketch is arranged, how it is saved, and its undo, as the owner set on 2026-10-14. The host's copy for live editing follows it. The host stores its preferences, and this product's recovery files, in its own folder, as the owner set on 2026-10-14.

Simple Sketch without a host. Simple Sketch is started with no host and driven entirely through the protocol: its declarations are read, and a command the protocol exposes changes the sketch in the way that command does inside the host. The host process is not running.

The boundary check from H-3: A check of the crate and program boundaries fails if either side can import the other.

## Development and preview

The team repository is https://github.com/Spumcake/Inpainter-Team. The app repository is https://github.com/Spumcake/Inpainter. The permanent checkout is `src/inpainter/worktrees/dev`. On 2026-10-14 that worktree's committed revision is `2595cfb`, "Start the Inpainter repository." The first slice is uncommitted on top of it, and GitHub still has no commits. Project documents stay in the team repository. The reference tree `src/simple-sketch` stays where it is.

From that worktree, `cargo build` places `inpainter` and `template` in `target/debug`, and `cargo run -p inpainter` opens the window. Both were run on 2026-10-14. `cargo test` passed the same day. The window that opened matched the first-slice paragraph of H-1.

A later preview of Simple Sketch inside the host uses the Simple Sketch preview — draw, erase, undo, pan, zoom, and fit — started from the host, with the common UI present. There is no seed drawing and no network service.

## Verification and delivery

Check levels and the release-test brief are in [testing.md](testing.md). H-1 and H-2 are judged by looking at the running host, against the sentence the owner agreed for that slice and against the shell reference. H-3 is a boundary check of the crates. H-4's sketching rules stay covered as they are in Simple Sketch; the new failure this version must catch is that those rules no longer hold once the host and the sub-application are separate, or that pen input crosses a process. H-5 is one scripted test that drives the sub-application with no host. During a slice, add no new tests. Tests for approved behavior are written in one pass when the owner asks to commit.

No every-commit workflow is installed. Nothing is deployed. The owner pushes and tags. Commits happen only when the owner asks, and only the branch-manager changes Git state.
