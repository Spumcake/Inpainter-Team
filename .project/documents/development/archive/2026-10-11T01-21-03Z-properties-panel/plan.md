# Simple Sketch plan

The document tree is committed on `dev` as `6210d8e`, on top of `7920c85`, and is not pushed. A window check met this slice, and a later review fixed it ([review audit](audits/2026-10-11T01-07-33Z-document-tree-review.md)). The previous plan is [archived](archive/2026-10-10T23-09-01Z-document-tree/plan.md).

## Document tree and registries

A new sketch contains Canvas 1 and the nine-color palette. The brush draws on that canvas, and the eraser hides only the ink it crosses. New sketch and Fit work. Open and Save wait. Swatches and Size come from the highlighted canvas's active color, and changing that color's size restyles its strokes. Tabs, undo, and the Wayland pen path still work. The properties panel, the outliner, shapes, and text are not in this slice.

The window check is in [testing.md](testing.md). The record is [the document-tree audit](audits/2026-10-11T00-15-02Z-document-tree.md). Session tests cover layer order and a color restyle that leaves samples unchanged. Zoom stops, pan, and device pressure were not checked in this window.

## Later

The properties panel and palette editing, then selection with the Layers outliner and groups, then shapes and text, then the clipboard and snapshots. Each stays broad until it is the current slice. Snapshots wait until every object kind exists.

Dashed ink is stored and not drawn yet. With the select tool, the Size row is absent until its context actions exist. An open stroke paints in its color layer, the title bar follows the desktop theme, and the clean-sketch rule ignores the active color. Pen pressure on the XP-Pen is still unchecked.
