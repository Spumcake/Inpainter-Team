# Simple Sketch todos

The previous list is [archived](archive/2026-10-11T22-25-12Z-text/todo.md).

- [ ] Decide the open points in [the plan](plan.md): where a new text object sits in the stack, what it is named, what a drag does while Auto-size and Wrap start on, which point of the box sits on the click, the starting alignment, emphasis, and background, whether editing selects the object, and whether the first commit is one undo step. Those parts stay open until then. The caret has no render.
- [x] Text is built and committed as `ae8a9aa`. A person can click, type, commit, and undo, and a stroke and a rectangle still work.
- [x] The window checks in [testing.md](testing.md) covered placing, typing, multi-line, undo, wrap, the text page's chrome, and the eraser. Editing in place and italic were not confirmed.
- [ ] Say if any shape-drag reading in the previous plan should change. The shapes slice was built from those readings.
- [x] Tool palettes are committed as `533bfa3`: every creation tool's palette, Lock size, a Size slider that only sets the next mark, layers linked to brush colors, palettes on shapes and text, and the owner's review that followed. See [the audit](audits/2026-10-12T02-52-06Z-tool-palettes.md).
- [x] Font size and other settings now change text being edited in place: the panel shows that object instead of the text tool's page. With the select tool it already worked.
- [x] A toolbar wider than the window scrolls: the wheel turns it sideways, and dragging the row moves it. Swatches, sliders, and enabled buttons keep the pointer.
- [x] The owner's review of the toolbar and text is committed as `0c1d3b7`. See [the audit](audits/2026-10-12T18-37-26Z-toolbar-and-text-review.md).
- [ ] Try text in a live window: typing, toolbar and panel clicks while typing, Escape, the edge handles, vertical alignment, and the Border. Only automated tests and headless screenshots have checked it.
- [ ] Decide whether the text toolbar should fit 1280 points. With vertical alignment it overflows and scrolls; Padding and Radius could move to the panel, or the alignments into a menu.
- [ ] Say which pointer position moves a row into or out of a group. Sibling reorder works. The cross-parent drag waits on that rule.
- [ ] Test pen pressure on the XP-Pen when more of the app is in hand. Thinning, alignment, the eraser end, and the hover cursor are still unchecked. See [the pressure audit](audits/2026-10-10T15-48-17Z-wayland-pen-pressure.md).
