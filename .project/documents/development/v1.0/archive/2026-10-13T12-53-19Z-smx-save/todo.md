# Simple Sketch todos

The previous list is [archived](archive/2026-10-13T01-32-29Z-snapshots/todo.md).

- [x] Build save and open on top of the objects already in the app. Session tests round-trip a sketch, and review's two defects were fixed. See [the snapshots audit](audits/2026-10-13T02-16-53Z-snapshots.md).
- [ ] Try that slice in the Simple Sketch window: draw, Ctrl+S, quit, run it again, Ctrl+O, and click the sketch. A check that stayed inside the window could list the sketch and could not choose the row. After it opens, click a canvas before drawing, or the next stroke makes a new canvas.
- [ ] Confirm or change the readings in [the plan](plan.md): no canvas highlighted after open, a bad snapshot left out with its id reserved, a failed save left dirty, the list in sketch-id order, and rows that show the saved name only.
- [ ] Try copy, cut, and paste in the window on `c435080`. The steps are in the [archived list](archive/2026-10-13T01-32-29Z-snapshots/todo.md). Release the letter before Ctrl.
- [ ] Confirm or change the clipboard readings in the [archived plan](archive/2026-10-13T01-32-29Z-snapshots/plan.md). The build used them.
- [ ] Try the latest text in a live window, including the edge handles, vertical alignment, and the Border.
- [ ] Decide whether the text toolbar should fit 1280 points. It overflows and scrolls.
- [ ] Say which pointer position moves a row into or out of a group. Sibling reorder works.
- [ ] Test pen pressure on the XP-Pen. See [the pressure audit](audits/2026-10-10T15-48-17Z-wayland-pen-pressure.md).
