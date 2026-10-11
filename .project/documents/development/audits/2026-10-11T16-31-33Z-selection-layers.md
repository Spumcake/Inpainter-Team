# Selection and Layers

**UTC:** 2026-10-11T16:31:33Z
**Scope/baseline:** App repository `vector-sketch`, branch `dev` at `4398ea6` (`Add the properties panel for the brush, eraser, and palette colors.`). The slice is uncommitted. Nothing was pushed.
**Outcome:** Completed for selection and the Layers list in the dirty worktree. Not a clean candidate. Dragging a row into or out of a group stays unwired.

## Goal and participants

The goal was a select tool and a Layers list the owner can try on canvases and groups. One task-operations worker wrote it. The selection, the list, and the toolbar share the session and the frame, so a second implementer was not started. Verification and code review then ran together. The same worker fixed the three defects review found.

## Attempts and result

Press V, click the ink, and drag the box. Undo puts it back and leaves the samples. New canvas, group, and ungroup work. F4 opens Layers with the edge tab and without a Properties title, a close button, or a tool card. The header is the New canvas command.

Review found three defects, and they were fixed. A selected child was transformed again on top of a selected ancestor. Undo or nudge during a drag could drop history. A locked canvas still toggled its color eye and lock.

The drop that moves a node into or out of a group is still unspecified, so that drag was not built. Sibling reorder works. Group and Ungroup are how objects change parent.

## Evidence and limits

`cargo test --offline` passed 46 tests after the fixes. Verification did not repeat that suite. The window journey is in [testing.md](../testing.md), on an X11 relaunch because the Wayland window could not be captured. Pick, move, nudge, scale, rotate, marquee, group, ungroup, hide, lock, zoom to selection, delete, and the Layers chrome passed. Unchecked in the window: bring forward, send backward, align, flip, the rotate menu, renaming, reordering rows, and a Shift nudge.

## Next action

Shapes and text are the next milestone in [plan.md](../plan.md) and are not started. The cross-parent drop rule is the owner's. Committing this work is a separate request.
