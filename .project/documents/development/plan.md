# Simple Sketch plan

Simple Sketch is a native desktop canvas for pressure-sensitive vector sketching. No developer diary existed when this plan was written. This order follows the pitch, the interface, the systems contract, and the acceptance criteria. The stroke slice is in the local dev worktree and is not committed. The bare clone of https://github.com/Spumcake/vector-sketch is in place, and that remote has no commits. The checkout attempt is recorded in [the checkout audit](audits/2026-10-10T01-19-15Z-app-checkout.md). The previous plan is [archived](archive/2026-10-10T01-25-57Z-slice-plan/plan.md).

## Stroke model and infinite canvas

This comes first, and the slice is now in the dirty worktree at `697f231`. A person can open one blank sketch, draw a smoothed stroke, erase part of it, undo, pan, zoom, and fit. Eight session tests passed. The window journey is recorded in [testing.md](testing.md), and the result is in [the stroke audit](audits/2026-10-10T02-37-47Z-stroke-canvas.md). A later review fixed fill artifacts, per-frame cost, and anti-aliasing, and nine tests now pass ([review audit](audits/2026-10-10T15-12-57Z-stroke-canvas-review-fixes.md)). Pen pressure still does not reach the app on Linux. The pictured toolbar, shapes, text, selection, and saving are not started.

What becomes usable is one blank sketch: draw a smoothed stroke, erase part of it without destroying the stroke, undo, then pan, zoom, and fit. The pictured toolbar, shapes, text, selection, and saving wait.

It is complete when [SS-1](tasks.md), [SS-2](tasks.md), [SS-3](tasks.md), and the pen and erase parts of [SS-4](tasks.md) hold. Session unit tests cover those rules. The same journey in the running window is the check for smoothing and for whether the erase follows the path.

## Pictured shell

After those types have landed, one worker adds the toolbar, tabs, and menu so the window matches the desktop reference. Open and Save wait until sketches can be stored. This milestone is complete when [SS-5](tasks.md) holds on a running window.

## Shapes, text, and selection

Shapes come next, then text, then selection and the in-app clipboard, because each one edits the same session files. This milestone is complete when [SS-6](tasks.md) holds and [SS-4](tasks.md) includes those gestures.

## Saving a sketch

Private snapshots then give Save and Open a stable identity. This milestone is complete when [SS-7](tasks.md) holds. A published file format is not part of it.

The first milestone is in the working tree and is not a commit. GitHub still has no commit. No release version is agreed, so this plan does not include a candidate. The later milestones are not started.
