# Simple Sketch plan

Rectangles, ellipses, and lines are in the dirty `dev` worktree on top of `5567860`, and are not committed. A window check met the journey after a rebuild ([audit](audits/2026-10-11T21-10-38Z-shapes.md)). The previous plan is [archived](archive/2026-10-11T20-08-05Z-shapes/plan.md).

## Rectangles, ellipses, and lines

A person can drag with the rectangle, ellipse, or line tool and release to place that shape at the top of the sketch. Escape during the drag cancels it. The shape keeps the fill, stroke, dash, and corner radius it was given. Later edits to the tool leave it alone. Fill starts off, so the first shape is an outline. The Size slider sets the next stroke width. F4 opens that tool's page on the same chrome as the brush page. With the select tool, the shape can be picked by its fill or its stroke, moved, and deleted. The eraser leaves it alone. Undo removes the shape, or restores a later edit to it.

The window check is in [testing.md](testing.md). Drawing, erasing, and the brush, eraser, and color pages still work. Text stays the next slice.

## Later

Text, then the clipboard, then snapshots. Each stays broad until it is the current slice. Related shades stays undefined. The XP-Pen check remains the owner's. The select row still overflows a 1280-point window, and a row drag still has no rule for moving a node into or out of a group.

## Readings used

The documents did not fix these, and the build used one reading of each so a drag could be tried. They are not new decisions:

- The press stays put. A square or circle uses the greater of the width and height. Snap moves the press and the pointer onto the grid first, then that constraint runs. A line keeps its length and steps by 45 degrees.
- A drag that does not move adds nothing.
- Names are Rectangle N, Ellipse N, and Line N, on the same rule as Group N.
- Constrain proportions and Snap to grid stay on the tool page. A selected shape shows Fill, Stroke, and Shape style.
- Releasing the drag does not select the new shape.
- Fill starts off. Turned on, it is light red `#FCA5A5` at 40%, from systems.

The opening of [systems.md](systems.md) still describes the flat mark list at `7920c85`. This slice starts from `5567860`.
