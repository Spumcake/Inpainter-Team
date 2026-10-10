# Simple Sketch — delivery

**Sources:** [Pitch](../PITCH.md), [interface](../../../INTERFACE.md), and [systems](systems.md). The desktop reference is [ui.png](../../renders/ui/ui.png).

**Objective and exclusions:** A person can open the desktop app, draw pressure-sensitive smoothed vector strokes on an infinite canvas, erase part of a stroke without destroying it, and undo. The first version also includes the pictured shell, shapes, text, selection, clipboard, tabs, and saving a sketch inside the app. Phone layouts, collaboration, accounts, cloud sync, a published file format, and printing are out.

**Version convention:** No release version is agreed. Do not tag a candidate until one is. After a version is agreed, candidates use `<version>-candidate.<n>`.

## Planning and execution

Milestones are in [plan.md](plan.md). Current actions are in [todo.md](todo.md).

## Acceptance criteria

SS-1 Pen. From a blank sketch, a pen drag that reports pressure leaves a vector stroke that is thinner where pressure is lower. A drag that reports no pressure uses the full current size. After the stroke settles, its samples and identity are still the stylus input.

SS-2 Eraser. Dragging the eraser across part of an existing stroke hides only the ink inside the eraser path. The rest of that stroke stays. Its samples and identity stay. A stroke drawn afterward stays fully visible. Undo shows the hidden ink again. Moving the stroke moves the hidden portion with it.

SS-3 Camera. Pan and zoom change the view and leave stored points and size unchanged. Zoom stops at 12.5% and 800% of the default. Fit frames the visible ink. An empty sketch, or one whose ink is fully hidden, returns to the default view centered on the origin. Pan and zoom are not undo steps.

SS-4 History. Each committed pen stroke or erase is one undo step for the active sketch. Undo then redo restores that step. A new committed gesture clears redo. A press that is too short to be a stroke does not enter history. Later gestures — a shape, label, move, scale, recolor, deletion, paste, or cut — are further steps in that same history.

SS-5 Shell. The window follows the desktop reference: menu, tabs, the listed swatches with the current color marked, copy, paste, cut, undo, redo, pen, eraser, select, rectangle, ellipse, line, text, size from 1 to 32 defaulting to 3, and zoom in, zoom out, and fit. A new sketch is empty. Each tab keeps its own marks, selection, view, and history. Tool, color, and size stay as they are across tabs. The window always keeps one sketch. Closing the only clean blank does nothing.

SS-6 Other marks. A rectangle, ellipse, or line commits an unfilled outline in the current color and size. Shift constrains a square, a circle, or 45-degree steps. Text commits one plain single-line label. Selection can marquee, move, scale, recolor, and delete whole objects. Copy, cut, and paste stay inside the app. Paste offsets the duplicate and selects it.

SS-7 Keep. Save stores the active sketch under a stable identity so a later open returns it. Two sketches may share a name and still open separately. Opening a sketch that is already open focuses its tab and does not replace unsaved marks. A sketch with unsaved marks or a renamed title asks once before it closes. Discard drops those unsaved changes. Save then close keeps the stored sketch.

## Development and preview

The app repository is `vector-sketch` at https://github.com/Spumcake/vector-sketch. The bare clone is `src/vector-sketch/worktrees/.bare`. The remote has no commits, so `src/vector-sketch/worktrees/dev` does not exist yet. The branch-manager adds that checkout from `origin/main` after `main` has a commit, and before the first code change.

These commands were not run. From that dev worktree, once the crate exists: `cargo test` for the session rules, and `cargo run` for the window. The Linux packages named in systems were not installed here.

Preview: open the window, draw a stroke, erase part of it, undo, pan, zoom, and fit. A new sketch starts empty. There is no seed drawing and no network service.

## Verification and delivery

The session rules in SS-1 through SS-4, the tab and close rules in SS-5, and the save-identity rule in SS-7 belong in unit tests. Appearance, smoothing, and whether an erase follows the path it crossed are a human preview of `cargo run`, not a mesh snapshot. Check levels and the release-test brief are in [testing.md](testing.md). No every-commit workflow is installed. No version is agreed, so no candidate is recommended.

The first implementation slice is the stroke model and canvas: SS-1, SS-2, SS-3, and the pen and erase parts of SS-4. Task-operations owns that slice and later integration in this app. The branch-manager is the only role that initializes the repository or commits. Nothing is deployed. The owner pushes and tags.
