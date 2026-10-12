# Simple Sketch todos

The previous list is [archived](archive/2026-10-11T22-25-12Z-text/todo.md).

- [ ] Decide the open points in [the plan](plan.md): where a new text object sits in the stack, what it is named, what a drag does while Auto-size and Wrap start on, which point of the box sits on the click, the starting alignment, emphasis, and background, whether editing selects the object, and whether the first commit is one undo step. Those parts stay open until then. The caret has no render.
- [x] Text is built and committed as `ae8a9aa`. A person can click, type, commit, and undo, and a stroke and a rectangle still work.
- [x] The window checks in [testing.md](testing.md) covered placing, typing, multi-line, undo, wrap, the text page's chrome, and the eraser. Editing in place and italic were not confirmed.
- [ ] Say if any shape-drag reading in the previous plan should change. The shapes slice was built from those readings.
- [ ] Build tool palettes after text, from INTERFACE's Palettes and Size sections and systems slice 6. A stroke drawn after moving the Size slider keeps its size, and old strokes stay.
- [ ] Fix the panel's font size not resizing a selected text object, which the owner reported in the text slice.
- [ ] Decide how the select row fits the default 1280-point window: a wider window, a tighter toolbar, or scrolling.
- [ ] Say which pointer position moves a row into or out of a group. Sibling reorder works. The cross-parent drag waits on that rule.
- [ ] Test pen pressure on the XP-Pen when more of the app is in hand. Thinning, alignment, the eraser end, and the hover cursor are still unchecked. See [the pressure audit](audits/2026-10-10T15-48-17Z-wayland-pen-pressure.md).
