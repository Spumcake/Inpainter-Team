# Simple Sketch plan

Selection and the Layers outliner are committed on `dev` as `5567860`, not pushed. A window check met the journey, review fixes followed it ([audit](audits/2026-10-11T16-31-33Z-selection-layers.md)), and the owner's review reshaped the outliner, toolbar, and handles ([review audit](audits/2026-10-11T19-54-52Z-selection-layers-review.md)). The previous plan is [archived](archive/2026-10-11T15-32-10Z-selection-outliner/plan.md).

## Selection and the Layers outliner

With the select tool, a person can pick a canvas by its ink, then move, scale, rotate, nudge, or delete it. Undo restores that change and leaves the stroke samples alone. A second canvas and a group take the same actions. The Size slot becomes the context actions, one row of toolbar buttons: bring to front, forward, backward, and to back, align, group, ungroup, and Rotate and Flip menus. Zoom to selection is Shift+2. F4 or the edge tab opens Layers on the same chrome as the brush page. The list shows the sketch from top to bottom, with an eye, a lock, in-place rename, a right-click menu, and drag, and a canvas expands to its colors. A canvas's menu can make its palette the default. New canvas, in the toolbar before the brush, adds an empty canvas at the top and highlights it, so the next stroke lands there. An empty canvas has no box and is reached from the list.

The window check is in [testing.md](testing.md). Drawing, erasing, and the brush, eraser, and color pages still work. Shapes, text, and copy, cut, and paste stay later. Dragging a row into or out of a group is still unwired.

## Later

Shapes and text can proceed together now that this slice is committed, then the clipboard, then snapshots. Related shades stays undefined. The XP-Pen check remains the owner's.

## Still open

A row drag reorders siblings. Moving a node into or out of a group by dragging still has no drop rule, so that gesture waits. The build followed the sentences already written for the other points: a marquee selects the group; the last canvas can be deleted; handles use the near-black accent. Those are not new decisions. The select row does not fit the default 1280-point window and scrolls; whether to widen the window or tighten the toolbar is the owner's.

The opening of [systems.md](systems.md) still describes the flat mark list at `7920c85`. This slice starts from `4398ea6`.
