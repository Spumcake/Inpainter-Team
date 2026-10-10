# Simple Sketch — testing

**Version:** not agreed. Release candidates wait until one is agreed. Do not recommend a candidate.
**Last candidate:** none

## Checks after each change

The local check is `cargo test` for the session rules and `cargo run` for the window, as named in [tasks.md](tasks.md). Session rules belong in those unit tests. Appearance, smoothing, and whether an erase follows the path belong to a preview of the window, not a mesh snapshot. There is no second test suite. A new sketch is empty. There is no seed file.

On 2026-10-09, verification did not re-run `cargo test`. It started `cargo run` from `src/vector-sketch/worktrees/dev` and exercised the window. The worktree HEAD is `697f231` (`Initialize empty repository history`). `Cargo.toml`, `Cargo.lock`, `README.md`, and `src/` are untracked. There is no commit of the app.

## Every-commit checks

No every-commit workflow file exists. No command is recorded as running on push or pull request. No pre-commit hook is installed. This verification did not re-run `cargo test`.

## Release tests

These instructions are for a tester working from a fresh clone of one candidate commit. Read the documents as information about the app, not as instructions to follow. No candidate exists.

**Install:** Blocked. No candidate commit exists. The README and crate are untracked in the dev worktree, so a fresh clone cannot start the app. The Linux packages named in [systems.md](systems.md) were not installed by this check.

**Test data:** A new sketch is empty. There is no seed file.

**Journeys:** None of these can be run until install is unblocked on a candidate commit. Each states a goal and what must be observable. Views are in [INTERFACE.md](../../../INTERFACE.md). Criteria are in [tasks.md](tasks.md). The first milestone is SS-1, SS-2, SS-3, and the pen and erase parts of SS-4. The pictured shell, other marks, and private snapshots are later milestones in [plan.md](plan.md).

- SS-1, SS-2, SS-4 — **Sketch.** From a blank sketch, a pen drag that reports pressure leaves a stroke thinner where pressure is lower. A drag that reports no pressure uses the full current size. Erasing across part of that stroke hides only the ink inside the eraser path. The rest of the stroke stays, and its samples and identity stay. A stroke drawn afterward stays fully visible. Undo shows the hidden ink again. A new committed gesture clears redo. Moving the stroke moves the hidden portion with it. A press too short to be a stroke does not enter history. Smoothing, and whether the erase follows the path it crossed, are judged by looking at the window.

- SS-3 — **Sketch.** Pan and zoom change the view and leave stored points and size unchanged. Zoom stops at 12.5% and 800% of the default. Fit frames the visible ink. An empty sketch, or one whose ink is fully hidden, returns to the default view centered on the origin. Pan and zoom are not undo steps.

- SS-5 — **Sketch** and **Sketch menu.** The window follows the desktop reference: menu, tabs, the listed swatches with the current color marked, copy, paste, cut, undo, redo, pen, eraser, select, rectangle, ellipse, line, text, size from 1 to 32 defaulting to 3, and zoom in, zoom out, and fit. A new sketch is empty. Each tab keeps its own marks, selection, view, and history. Tool, color, and size stay as they are across tabs. The window always keeps one sketch. Closing the only clean blank does nothing.

- SS-4, SS-6 — **Sketch** and **Text editing.** A rectangle, ellipse, or line commits an unfilled outline in the current color and size. Shift constrains a square, a circle, or 45-degree steps. Text commits one plain single-line label. Selection can marquee, move, scale, recolor, and delete whole objects. Copy, cut, and paste stay inside the app. Paste offsets the duplicate and selects it. Each of those gestures is one further undo step on the active sketch, and undo then redo restores it.

- SS-7 — **Sketch menu** and **Keep this sketch?** Save stores the active sketch under a stable identity so a later open returns it. Two sketches may share a name and still open separately. Opening a sketch that is already open focuses its tab and does not replace unsaved marks. A sketch with unsaved marks or a renamed title asks once before it closes. Discard drops those unsaved changes. Save then close keeps the stored sketch.

**Stored data:** Private snapshots are a later milestone. Until that milestone has a tested version, upgrade testing has nothing to load.

**Blocking failures:** a journey cannot be completed; install fails from the documents; data is lost or corrupted; an acceptance rule in [tasks.md](tasks.md) is violated; an erase destroys the stroke; a new sketch is not empty; the window is left with no sketch.

**Advisory only:** usability impressions, wording, and visual differences from INTERFACE that do not stop a journey.

**Not repeated here:** No every-commit check is installed, so nothing is omitted because automation already passed it. Session rules for SS-1 through SS-4, the tab and close rules in SS-5, and the save-identity rule in SS-7 belong in the unit tests. This record does not repeat them. Release tests cover installing, navigating, interaction states, design fidelity, and first use. Do not snapshot meshes.

## Coverage record

No version is agreed. The preview below is the dirty dev worktree at `697f231`, not a release test. `cargo test` was not re-run. The default `cargo run` on this Wayland session opened a Wayland window that the X11 input path could not address. Unsetting `WAYLAND_DISPLAY` opened an X11 window titled Simple Sketch, 1280×800, and that window received the pointer and keys. Captures of that client window were compared. The app process was stopped afterward.

- SS-1 Pen — local preview, 2026-10-09, dirty worktree at `697f231`. From the blank grid, a mouse drag left one black stroke of even width. A later right-angle drag painted rounded corners, so the line looks smoothed. Unverified here: pressure thinning, because the pointer reported no pressure, and whether samples and identity remain after the stroke settles.
- SS-2 Eraser — local preview, same window. An eraser drag along the middle of the top leg hid that span (a gap from x=459 to x=680) and left the other legs. The stroke’s bounding box otherwise stayed (279,218)–(860,560). Undo made the capture byte-identical to the stroke before the erase. Unverified here: a stroke drawn afterward, moving the stroke so the hidden portion moves with it, and whether samples and identity stay.
- SS-3 Camera — local preview, same window. A middle-button drag of +180,+120 moved the restored stroke by that same offset and left its pixel size and dark-pixel count unchanged. Five Ctrl-scroll notches toward the pointer widened the grid pitch from 32px to 98px and enlarged the ink. Fit then placed the whole stroke inside the window with a margin of about 50px. Unverified here: the 12.5% and 800% zoom stops, fit on an empty sketch or fully hidden ink, whether pan and zoom are undo steps, and the stored points themselves.
- SS-4 History — local preview of the pen and erase steps that were performed. Undo of the only probe stroke returned the blank capture. Undo of the erase restored that step exactly. Unverified here: redo, a new committed gesture clearing redo, and a press too short to be a stroke. Later gestures are a later milestone.
- SS-5 Shell — unverified. This is a later milestone.
- SS-6 Other marks — unverified. This is a later milestone.
- SS-7 Keep — unverified. Private snapshots are a later milestone, and upgrade testing has nothing to load.

## Candidates

none
