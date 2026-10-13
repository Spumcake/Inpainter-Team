# Simple Sketch plan

Copy, cut, and paste are in the dirty `dev` worktree, on top of `0c1d3b7`. The window has not shown a paste. Snapshots stay next and are not started. The previous plan is [archived](archive/2026-10-13T00-06-49Z-clipboard/plan.md). The records are [the clipboard audit](audits/2026-10-13T00-27-06Z-clipboard.md) and [the key-path fix](audits/2026-10-13T01-07-24Z-clipboard-keys.md).

## Copy, cut, and paste

A person can select a shape, a text object, a group, or a canvas and press Ctrl+C. Ctrl+V puts a duplicate at the top of the sketch, 16 units down and to the right, and selects it. The next paste steps another 16 units. A copied canvas brings its palette, strokes, and erasures. A copied group brings its children. Ctrl+X copies and then deletes, as one undo step. Undo of a paste removes the duplicates. Copy with nothing selected does nothing. The clipboard belongs to the window, so a copy in one tab pastes in another. It is not the desktop clipboard. While text is being edited, these shortcuts do not run. There are no toolbar icons for them.

One worker built it in the session and the document. Those session tests call the commands directly. In the window, egui replaced Ctrl+C, Ctrl+X, and Ctrl+V with copy, cut, and paste events and dropped the key, so the shortcuts did nothing. The frame now runs the in-app clipboard from those events. A frame test sends the same events and checks that a selected rectangle is duplicated. That test passed. The owner has not yet seen a paste after this fix. Quit any open Simple Sketch window and run the worktree again before trying.

## Later

Snapshots, so Open and Save store a sketch and bring it back. That stays broad until it is the current slice. Related shades stays undefined. The XP-Pen check remains the owner's. The latest toolbar and text behavior still needs a live window. The text toolbar overflows 1280 points. A row drag still has no rule for moving a node into or out of a group. The shape-drag readings are still not owner decisions.

## Readings used

The documents left these open. The build used these readings. They are not owner decisions:

- Paste with an empty clipboard does nothing.
- A selected child of a selected group is copied once, inside the group.
- Copy may include a locked object. Cut deletes only objects that can be changed. When nothing can be deleted, the sketch and the clipboard stay as they are.
- Undo of a cut restores the objects and leaves the clipboard holding the copies.
- While text is being edited, the object clipboard does not run, and highlighted words are not copied.

Undo of a paste does not rewind the repeat offset. A cut of a mixed selection copies only the objects it can delete. A locked child inside an unlocked group is copied and deleted with that group.

The opening of [systems.md](systems.md) still describes the flat mark list at `7920c85`. This slice starts from `0c1d3b7`.
