# Simple Sketch todos

The previous list is [archived](archive/2026-10-13T00-06-49Z-clipboard/todo.md).

- [x] Build copy, cut, and paste on top of the objects already in the app. Session tests passed and review found no defects. See [the clipboard audit](audits/2026-10-13T00-27-06Z-clipboard.md).
- [ ] Try copy, cut, and paste again in the Simple Sketch window. The first try did nothing because the keys never reached the command. Quit any open window, run this worktree, draw a rectangle, then Ctrl+C, Ctrl+V, Ctrl+V, Ctrl+Z, Ctrl+X, Ctrl+Z, and Ctrl+V. Release the letter before Ctrl.
- [ ] Confirm or change the readings in [the plan](plan.md): an empty paste, a group copied with one of its children, a cut of a locked object, undo of a cut leaving the clipboard full, and Ctrl+C while typing. The build used those readings.
- [ ] Try the latest text in a live window: typing, toolbar and panel clicks while typing, Escape, the edge handles, vertical alignment, and the Border. Only automated tests and headless screenshots have checked that review.
- [ ] Decide whether the text toolbar should fit 1280 points. With vertical alignment it overflows and scrolls.
- [ ] Say which pointer position moves a row into or out of a group. Sibling reorder works. The cross-parent drag waits on that rule.
- [ ] Test pen pressure on the XP-Pen when more of the app is in hand. Thinning, alignment, the eraser end, and the hover cursor are still unchecked. See [the pressure audit](audits/2026-10-10T15-48-17Z-wayland-pen-pressure.md).
