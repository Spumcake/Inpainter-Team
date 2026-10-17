# Inpainter — version 0.2 delivery

**Sources:** [Mission](../../concept/MISSION.md), [pitch](PITCH.md), [interface](../../../../INTERFACE.md), and [systems](systems.md). Sketching behavior is Simple Sketch 1.0.1, as its code carries it into `apps/simple-sketch` and the host, and as the Simple Sketch 1.0.1 documents, which the owner removed from the project on 2026-10-15 once Simple Sketch was part of Inpainter; they stay in the team repository's history up to `f0e146c`. The shell is the renders [INTERFACE.md](../../../../INTERFACE.md) cites, under `.project/renders/ui/`. The reference code is Simple Sketch at `887f6f4`. What carries over is [reuse.md](reuse.md). The owner approved a first map on 2026-10-14; its code faked the host and sub-application boundary, and was set aside on the branch `scrap/v0.2-shared-session`. The map was rewritten the same day on the line the owner chose, and awaits the owner's approval.

**Objective and exclusions:** Version 0.2 makes Simple Sketch 1.0.1 a sub-application of Inpainter, as the owner set on 2026-10-14. Simple Sketch keeps looking and behaving as it does today. Nothing about it is redesigned. The person launches only the host. The host starts Simple Sketch. The criteria are the ones deferred at the end of [the 0.1 task list](../../history/v0.1/tasks.md): the host stays up and shows status when Simple Sketch exits or stops responding, sketching still holds, Simple Sketch can be driven with no host, and a check fails if the host and the app can import each other. The assistant and any second creative app are left for later versions. They are not removed from the product. An app drawing its own view by sending the host paint commands stays unproven and is not part of this version.

**Version convention:** 0.2 is the agreed working version, as the owner set on 2026-10-14. It is not the full product. Do not tag a candidate until the owner asks. After that, candidates use `0.2-candidate.<n>`.

## Planning and execution

Milestones are in [plan.md](plan.md). Current actions are in [todo.md](todo.md).

## Acceptance criteria

Sketching is defined by the Simple Sketch reference, not by a separate research brief. The shell is defined by the mission and by that same chrome.

H-2 Sub-application failure. With the host showing Simple Sketch, that program exits or stops responding. The host window stays open and shows the sub-application's status. The person can still use the host's common UI. The status looks like the two images in INTERFACE's Sub-application status, which the owner approved on 2026-10-14.

H-4 Simple Sketch inside the host. The person launches the host and sketches. The sketching behavior of Simple Sketch 1.0.1 still holds, criteria SS-1 through SS-10 of Simple Sketch 1.0.1, including a smoothed pressure-sensitive stroke on Canvas 1, erasing part of it without destroying the stroke, undo, and pan, zoom, and fit. They never launch Simple Sketch itself. Drawing, erasing, shapes, text, and ordering run in the host so pen input does not cross processes. The Simple Sketch sub-application owns the document: how the sketch is arranged, how it is saved, and its undo, as the owner set on 2026-10-14. The host's copy for live editing follows it. Simple Sketch keeps its preferences and recovery files in its own folder inside Inpainter's data folder, as the owner set on 2026-10-14. Undo and saving happen only in Simple Sketch's process; the host keeps no undo history and only replaces its display copy with the document Simple Sketch sends.

Simple Sketch without a host. Simple Sketch is started with no host and driven entirely through the protocol: its declarations are read, and a command the protocol exposes changes the sketch in the way that command does inside the host. The host process is not running.

H-3 boundary check. A check of the crate and program boundaries fails if the host package and the simple-sketch package can import each other, or if the shared drawing crate holds a session, registry, schema, store, or preferences. A sub-application that is not Simple Sketch gets its toolbar, menu, and panel from its declarations alone. These are the mission's tests of what would fake Inpainter, as [systems.md](systems.md) lists them.

Scaffold, as the owner agreed on 2026-10-14: `cargo run -p inpainter -- --app simple-sketch` opens the shell with one tab labeled Simple Sketch, the empty toolbar, the grid, and the panel tab, and nothing else happens.

H-7 Two sub-applications. As the owner set on 2026-10-14, one window holds tabs of more than one sub-application at once, each started by the host with its own process. The toolbar, menu, panel, and contents are always the selected tab's sub-application's. Selecting a tab of another sub-application swaps all of them. The host starts every sub-application `--app` names, and `--app` may be given more than once.

The window slice, as the owner chose on 2026-10-14, after Simple Sketch works with no host: `cargo run -p inpainter -- --app simple-sketch` opens Inpainter with the menu button, one tab labeled Untitled 1, and the plus. The toolbar is Simple Sketch's: swatches, file commands, undo and redo, the drawing tools, Size, and zoom. On Canvas 1, draw a smoothed pressure-sensitive stroke, erase part of it without destroying the stroke, undo, then pan, zoom, and fit. The grid and the panel tab stay. The person never launches Simple Sketch.

## Development and preview

The team repository is https://github.com/Spumcake/Inpainter-Team. The app repository is https://github.com/Spumcake/Inpainter. The permanent checkout is `src/inpainter/worktrees/dev`. On 2026-10-14 the committed revision is `a40e07c`, "Register sub-applications from their manifests." The scaffold of `apps/simple-sketch` and the scaffolding-skill fix are uncommitted on top of it. Project documents stay in the team repository. Simple Sketch 1.0.1's own code is no longer checked out beside it; it lives on in the host and `apps/simple-sketch`.

From that worktree, `cargo build` places `inpainter`, `template`, and `simple-sketch` in `target/debug`. `cargo run -p inpainter -- --app simple-sketch` opens the window described in the scaffold paragraph. Both were run on 2026-10-14. `printf '{"type":"shutdown"}\n' | target/debug/simple-sketch` printed the declaration and exited 0 the same day.

A later preview of sketching uses the Simple Sketch preview — draw, erase, undo, pan, zoom, and fit — started from the host, with the common UI present. There is no seed drawing and no network service. That preview is not this scaffold.

## Verification and delivery

Check levels and the release-test brief are in [testing.md](testing.md). H-4 is judged by looking at the running host, against the sentence the owner agrees for that slice and against the Simple Sketch reference. H-2 is judged the same way once a picture of the status exists. H-3 is a boundary check of the crates. Simple Sketch without a host is one scripted drive of the sub-application. During a slice, add no new tests. Tests for approved behavior are written in one pass when the owner asks to commit.

No every-commit workflow is installed. Nothing is deployed. The owner pushes and tags. Commits happen only when the owner asks.
