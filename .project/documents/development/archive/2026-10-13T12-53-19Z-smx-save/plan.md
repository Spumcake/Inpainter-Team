# Simple Sketch plan

Saving a sketch is in the dirty `dev` worktree, on top of `c435080`. The window saved an empty sketch and listed it, and did not open that sketch again. The previous plan is [archived](archive/2026-10-13T01-32-29Z-snapshots/plan.md). The record is [the snapshots audit](audits/2026-10-13T02-16-53Z-snapshots.md).

## Save and open

A person can draw, save, quit, and open that sketch again. Objects, palettes, strokes, erasures, and the view come back. History, the selection, and the clipboard do not. Saving again updates the same sketch. Open lists saved sketches by name, focuses one that is already open, and stays light when nothing is saved. Two sketches may share a name. Closing a changed or renamed sketch asks once. Save on that question stores it and then closes it. Discard drops the unsaved changes. Tool settings stay in the preferences file they already use.

One worker built it, because save, open, and the close question share the session. Session tests round-trip a canvas, a stroke, an erasure, a shape, text, a group, and the camera, and they leave history, the selection, and the clipboard out of the file. A frame test sends the key events the window delivers for Ctrl+S. Review found two defects, and the same worker fixed them: Save commits words still being typed before it writes, and an unreadable file keeps its id so a later sketch cannot replace it. A check that stayed inside the Simple Sketch window saved Untitled 1 and opened the list. The row could not be chosen with a key, so the sketch was not opened again. The slice is ready to try. It is not treated as seen coming back in the window.

## Later

The owner's checks stay as they are: the clipboard in the window, the clipboard readings, a live try of text, whether the text toolbar should fit 1280 points, which drop moves a row into or out of a group, the shape-drag readings, and the XP-Pen. Related shades stays undefined. No further slice is agreed.

## Readings used

The documents left these open. The build used these readings. They are not owner decisions:

- An opened sketch has no selection and no highlighted canvas. The next brush stroke makes a new canvas. Click a canvas before drawing on the one that was saved.
- A snapshot that cannot be read is left out of Open and does not stop the app. Its id stays reserved. A save that cannot be written leaves the sketch dirty, with no new dialog.
- The Open list is in ascending sketch-id order.
- Each row shows the saved name only. Two rows may show the same name.

The opening of [systems.md](systems.md) still describes the flat mark list at `7920c85`. This slice starts from `c435080`.
