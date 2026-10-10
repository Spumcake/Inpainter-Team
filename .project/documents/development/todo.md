# Simple Sketch todos

The previous list is [archived](archive/2026-10-10T01-25-57Z-slice-plan/todo.md).

- [x] The branch-manager created local `dev` at `697f231` in `src/vector-sketch/worktrees/dev`. GitHub still has no commit, so this history is not on the remote.
- [x] Task-operations built the stroke model and infinite canvas in `src/vector-sketch/worktrees/dev`. Eight session tests passed. The app files are uncommitted. SS-1, SS-2, SS-3, and the pen and erase parts of SS-4 are in [tasks.md](tasks.md).
- [x] Verification ran that journey in the window and recorded it in [testing.md](testing.md). A mouse stroke looked smoothed, an erase hid the span it crossed, and undo restored the ink.
- [x] A review fixed triangle fill on self-overlapping strokes, cached derived ink, and turned on 4× MSAA. Nine session tests passed. The owner confirmed the anti-aliasing. See [the review audit](audits/2026-10-10T15-12-57Z-stroke-canvas-review-fixes.md).
- [ ] The owner chooses the toolbar contents: the reference picture shows New, Open, Save, and 9 swatches, while [INTERFACE.md](../../../INTERFACE.md) lists copy, paste, cut, and 11 swatches. This blocks the pictured shell.
