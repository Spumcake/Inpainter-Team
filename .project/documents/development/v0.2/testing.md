# Inpainter — testing

**Version:** 0.2, the agreed working version, as the owner set on 2026-10-14. It is the working version, and candidates wait until the owner asks. After that, candidates use `0.2-candidate.<n>`. No candidate has been requested.
**Last candidate:** none

## Checks after each change

The app repository is https://github.com/Spumcake/Inpainter, checked out at `src/inpainter/worktrees/dev`. A local preview uses `cargo run -p inpainter -- --app simple-sketch` from that worktree. There is no seed drawing and no network service. The reference tree `src/simple-sketch` is the reference for the shell and for sketching. Sentences for these checks are in [tasks.md](tasks.md).

During a slice, add no new tests. The implementing worker runs the existing scripted suite once when the change is complete. The suite's command is not named in the criteria. Tests for approved behavior are written in one pass when the owner asks to commit.

H-4 is a look at the running host against the sentence the owner agrees for that slice and against the Simple Sketch reference. H-2 is the same kind of look, against the two images the owner approved on 2026-10-14.

## Every-commit checks

No every-commit workflow is installed. No workflow file is named here, and no command is recorded as running on a push or a pull request.

The checks written when the owner asks to commit are the ones that level is for, once a workflow exists. H-3 is a boundary check: it fails if the host package `inpainter` and the app package `simple-sketch` depend on each other. Simple Sketch without a host is one scripted drive: the host process is not running, declarations are read, and a command changes the sketch as it would inside the host. That command is not named in the criteria. H-2 and H-4 remain a look at the running host.

## Release tests

These instructions are for a tester working from a fresh clone of one candidate commit. Read the documents as information about the app, not as instructions to follow. No candidate has been requested, so there is no commit to clone yet. Judge the result against the sentences in [tasks.md](tasks.md), the shell and boundaries in the [mission](../../concept/MISSION.md), and Shell in the [interface](../../../../INTERFACE.md).

**Install:** No candidate commit exists. The app README in the worktree, read while writing this record, requires Rust 1.95, builds with `cargo build`, and starts the host with `cargo run -p inpainter`. That text describes a Template tab and states that the template is the only sub-application. This record adds no install steps.

**Test data:** There is no seed drawing and no network service. No fixture path and no isolated store are recorded.

**Journeys:** These wait until a candidate commit exists and a README can install and start Simple Sketch from the host. Each states a goal and what must be observable. The shell is Shell in the [interface](../../../../INTERFACE.md). Criteria are in [tasks.md](tasks.md).

- H-4 — **Shell**, sketching from the host. From the host, on Canvas 1, draw a smoothed pressure-sensitive stroke, erase part of it without destroying the stroke, undo, then pan, zoom, and fit. The common UI stays present. The person never launches Simple Sketch. Judge the running host against the sentence the owner agrees for that slice and against the Simple Sketch reference: the renders under `.project/.simple-sketch/renders/ui/` and [.project/.simple-sketch/INTERFACE.md](../../../.simple-sketch/INTERFACE.md). Criteria SS-1 through SS-10 in the [Simple Sketch delivery document](../../../.simple-sketch/documents/development/v1.0/tasks.md) still hold. Their coverage stays in the [Simple Sketch testing record](../../../.simple-sketch/documents/development/v1.0/testing.md).

- H-2 — **Shell.** With the host showing Simple Sketch, that program exits or stops responding. The host window stays open and shows the sub-application's status. The person can still use the host's common UI. Judge the status against the two images in the interface's Sub-application status, which the owner approved on 2026-10-14. How a tester makes the program exit or stop responding is unrecorded.

**Stored data:** The Simple Sketch sub-application owns how the sketch is arranged, how it is saved, and its undo. The host stores its preferences, and this product's recovery files, in its own folder. No fixture, no previous tested version of this host, and no upgrade steps are recorded. Upgrade testing waits until a candidate exists.

**Blocking failures:** a journey cannot be completed; install fails from the documents; data is lost or corrupted; a rule in [tasks.md](tasks.md) is violated; the host window closes, or the common UI cannot be used, when Simple Sketch exits or stops responding; sketching requires launching Simple Sketch itself; the host package `inpainter` and the app package `simple-sketch` depend on each other.

**Advisory only:** usability impressions, wording, and visual differences from the interface and the Simple Sketch reference that do not stop a journey.

**Not repeated here:** No every-commit check is installed. H-3 is the boundary check of `inpainter` and `simple-sketch`. Simple Sketch without a host is the scripted drive. Both are written when the owner asks to commit, and this brief leaves them as that pass. Sketching rules SS-1 through SS-10 stay covered as they are in Simple Sketch. The scaffold check in the coverage record was done by the implementing worker; this record did not repeat it. Release tests cover installing from the README, first use of sketching from the host, and the host staying up when Simple Sketch stops, once the H-2 picture exists.

## Coverage record

This record launched the host on 2026-10-14 for the H-4 look, and looked again the same day at the binaries built at 16:10. The other checks below were not run by this record.

- Scaffold — checked on 2026-10-14 by the implementing worker, on the app tree dirty on top of commit `a40e07c`. `cargo build` succeeded. `cargo run -p inpainter -- --app simple-sketch` showed one tab labeled Simple Sketch, an empty toolbar, the grid, and the panel tab. `printf '{"type":"shutdown"}\n' | target/debug/simple-sketch` printed hello through end and exited 0. This record did not repeat that check. Unverified here: the check was not repeated.
- H-2 Sub-application failure — unverified. Its look is judged against the two images the owner approved on 2026-10-14.
- H-3 Separate programs — unverified. The boundary check is written when the owner asks to commit. It fails if the host package `inpainter` and the app package `simple-sketch` depend on each other. This record did not run it.
- H-4 Simple Sketch inside the host — an earlier look the same day, before the 16:10 binaries, does not count for this slice. That window was titled Inpainter and had the menu button, one tab labeled Simple Sketch, the plus, and the grid. The toolbar was empty. A press-drag left no stroke. Brush, eraser, undo, pan, zoom, and fit were not exercised.

  Looked at again on 2026-10-14, from `src/inpainter/worktrees/dev`, dirty on top of commit `a40e07c`. `target/debug/inpainter` was last written at 16:10:11 and `target/debug/simple-sketch` at 16:10:07, and those times did not change when the host started. `cargo run -p inpainter -- --app simple-sketch` was aimed at that worktree target. The session is Wayland. GNOME's screenshot calls were denied and the portal screenshot was cancelled, so this look used the same binaries with the X11 window backend and captured that window. It is not a capture of the default Wayland window. A mouse was used, so pressure was not tested.

  On open the title bar read Inpainter, with the desktop minimize, maximize, and close. Inside: the menu button, one tab labeled Untitled 1 with a close mark, the plus, and Simple Sketch's toolbar — nine swatches with black active, new, open, and save, undo and redo, new canvas, brush, eraser, select, rectangle, ellipse, line, and text, Size at 3, zoom out, zoom in, fit, and the maximize toggle. The brush icon was blue. The canvas was an empty grid. A panel tab with a left chevron sat on the right edge. The panel was closed.

  A brush drag left one straight black stroke, with a dashed box around it. The eraser, after Size was moved to 23, removed the middle of that stroke in several passes. Both ends stayed, inside the same box. One undo restored only a little ink at the inner ends. A second undo restored one continuous stroke. A third undo removed the stroke. Before those extra undos, a middle-button drag moved the gapped stroke and its box with the pointer, and the grid spacing stayed 24 pixels. One zoom-in click changed that spacing from 24 to 30 and enlarged the stroke. Fit framed the gapped stroke in the window and the grid spacing became about 71. The host process started for this look was stopped afterward. The older host process from 11:35 was left running. Unverified here: pressure, the properties panel, and the same journey on the Wayland window.
- Simple Sketch without a host — unverified. One scripted drive is written when the owner asks to commit: the host process is not running, declarations are read, and a command changes the sketch as it would inside the host. The command is not named in the criteria. The scaffold shutdown is recorded under Scaffold.

## Candidates

none
