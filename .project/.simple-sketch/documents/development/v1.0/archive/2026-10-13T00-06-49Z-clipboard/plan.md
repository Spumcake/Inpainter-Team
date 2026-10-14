# Simple Sketch plan

The owner's review of the toolbar and text is committed on `dev` as `0c1d3b7`, three commits ahead of `origin/dev` ([audit](audits/2026-10-12T18-37-26Z-toolbar-and-text-review.md)). Tool palettes are at `533bfa3` ([audit](audits/2026-10-12T02-52-06Z-tool-palettes.md)). Text is at `ae8a9aa`, with its window checks in [testing.md](testing.md). Rectangles, ellipses, and lines are at `94ad2c9`. The previous plan is [archived](archive/2026-10-11T22-25-12Z-text/plan.md).

## Text

A person can press T, click, and type. Enter starts a new line. Escape, or a click somewhere else, commits the text, and a click somewhere else with the text tool still current starts another. An empty commit is discarded. Dragging places a box of that width. Clicking an existing text object with the text tool edits it in place. The Size slider sets the next font size. F4 opens the text page on the same chrome as the brush page. With the select tool, the text is picked by its box and can be moved. The eraser leaves the letters alone. A committed edit is one undo step. Later edits to the tool leave that text alone.

This needs the shapes slice already on `dev`. One worker builds it, because the text object and the caret share the document and the frame. It is complete when the text sentences of [SS-6](tasks.md) hold, a committed edit undoes as in SS-4, the text page keeps the chrome in SS-9, and picking by the text box holds in SS-10. Drawing, erasing, and the shape tools still work.

## Later

**Tool palettes**, slice 6 in [systems.md](systems.md), are committed as `533bfa3`, together with the owner's review that followed: right-click menus with Properties, an object page, back rows, and the brush and eraser staying chosen. Claude Code built them, because the owner wants fixes and updates handled there rather than by the Cursor team. The model is in [INTERFACE.md](../../../../INTERFACE.md) under Palettes and Size. The same change fixes editing text in place: the panel now shows the text being edited.

**The owner's review of the toolbar and text** is committed as `0c1d3b7`, built by Claude Code over many short rounds. Text gained a box shaped by edge handles, vertical alignment, and a Border color; with the text tool the toolbar changes the text it is working on; text has no rotate or flip. Every tool outlines what it works on, and the toolbar scrolls. The rules are in [INTERFACE.md](../../../../INTERFACE.md) under Text editing, Selection, and the toolbar. None of it has been tried in a live window.

Then the clipboard, then snapshots. Each stays broad until it is the current slice. Related shades stays undefined. The XP-Pen check remains the owner's. A toolbar wider than the window now scrolls with the wheel or a drag, and a row drag still has no rule for moving a node into or out of a group. The shape-drag readings in the previous plan are still not owner decisions.

## Open decisions

These change the first text object, and they stay open:

- A new shape is placed at the top of the sketch. A new text object is not given a place in that order.
- Canvas N and Group N have naming rules. A text object does not.
- A click places auto-sized text, and a drag places a box of that width. Auto-size and Wrap both start on. The documents do not say whether that drag turns Auto-size off, or how Wrap behaves in a box that is also growing to fit.
- The click is where the text is placed. Which point of the box sits on that click is not named.
- Systems names the font, the size, the line height, auto-size, and wrap. It does not name a starting alignment, whether bold, italic, underline, or all caps start on, or whether the background fill starts off.
- Nothing says whether the text object is selected while it is being edited, or whether the first commit is one undo step or an insert followed by a content change.
- The renders do not draw the caret.

The opening of [systems.md](systems.md) still describes the flat mark list at `7920c85`. This slice starts from `94ad2c9`.
