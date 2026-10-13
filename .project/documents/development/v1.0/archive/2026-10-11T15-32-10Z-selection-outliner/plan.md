# Simple Sketch plan

The brush, eraser, and color panel is committed on `dev` as `4398ea6`, on top of `6210d8e`, and is not pushed. A window check met this slice, and a later review corrected and restyled it ([review audit](audits/2026-10-11T15-07-46Z-properties-panel-review.md)). The previous plan is [archived](archive/2026-10-11T01-21-03Z-properties-panel/plan.md).

## Brush, eraser, and color panel

F4 and the edge tab open and close the panel, and the canvas narrows while it is open. The panel has no Properties header and no tool-name and description card. Right-clicking a tool opens that tool's page without changing the current tool. Right-clicking a swatch opens that color. The brush page sets the preset, pressure, the curve, and smoothing, stabilization, and streamline. The next stroke keeps those settings. The eraser page has the same controls and its own size. The color page edits the color, including dashes and flat caps, and can move, duplicate, or delete it. The last color stays. That page was first built partly from the wrong render. It is now rebuilt from `color-property-ui.png`, as [INTERFACE.md](../../../../INTERFACE.md) describes, with moving done by dragging swatches. A finished color edit is one undo step. Brush and eraser settings are kept for the next time the app opens.

The window check is in [testing.md](testing.md). The record is [the panel audit](audits/2026-10-11T01-53-00Z-properties-panel.md). Flat cap, duplicate, hiding or locking a color, and settings after a restart were not checked in the window.

## Later

Selection and the Layers outliner, then shapes and text, then the clipboard and snapshots. Each stays broad until it is the current slice. Shape, text, and Layers pages wait with those tools, and they use the same panel chrome: the edge tab, and no Properties header, close button, or tool-name and description card, even where a render still draws those. Saving a palette as the default waits with the canvas's own properties.

The XP-Pen check remains the owner's.
