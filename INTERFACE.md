# Simple Sketch interface

Simple Sketch is the desktop window where someone sketches and annotates with a mouse or a pressure-sensitive stylus. Product intent lives in [the pitch](.project/documents/PITCH.md). This document describes that one window: how the pictured shell behaves, and the states a person moves through while drawing.

## Journey and sitemap

The app opens on **Sketch**, an infinite grid with one blank document. The journey stays inside that window.

- **Sketch** is the whole app. Drawing, erasing, selecting, panning, and zooming all happen here.
- The menu button opens **Sketch menu**, a short menu for creating, opening, saving, renaming, and closing sketches. Choosing one returns to Sketch.
- The text tool on the canvas enters **Text editing**, then returns to Sketch when the label is committed.
- Closing a sketch that has changes since its last save, or whose name was edited, opens **Keep this sketch?**, including a never-saved sketch that has no marks. A clean blank skips it: no marks, the generated Untitled name, never saved. Cancel stays on that sketch and does not add a sketch. Save and Discard close it and leave the person on another open sketch. A new blank sketch is created only after Save or Discard finishes, and only when that sketch was the last one, so the window still ends with one sketch.

## Presentation

The accepted appearance is the supplied desktop overview, [ui.png](.project/renders/ui/ui.png). It was inspected and is the reference for the shell. The render does not draw the menu, selection box, text caret, save confirmation, or empty canvas. The clipboard group is read as copy, paste, then cut.

The window is light and compact. A white title bar carries a small orange pencil and the words Simple Sketch, with minimize, maximize, and close at the right. Below that, a pale bar holds a menu button, document tabs, and a new-tab button. A second pale bar is the toolbar: color, clipboard, undo and redo, tools, stroke size, then zoom. Icons are simple dark line icons, matching the reference. The selected tool sits on a light gray rounded rectangle. The rest of the window is the canvas, white with a light square grid in even intervals. The grid pans and zooms with the drawing. It is a guide only, and marks are placed where the pointer draws them.

The ink in the reference — the handwritten title, the figure, the red note, the solids, the flowchart, and the landscape — is sample drawing. New sketches start empty.

Toolbar groups are separated by a thin vertical gap, in this order:

- Color swatches, left to right: black, red, orange, yellow, green, cyan, blue, purple, pink, light gray, gray. The current color is drawn larger, with a thin dark ring and a sliver of toolbar showing between the fill and the ring, so the mark stays visible on black. The others stay small filled circles in a fixed order.
- Copy, paste, and cut, then undo and redo.
- Pen, eraser, select, rectangle, ellipse, line, and text.
- The word Size, a slider, and the current size as a number. The pictured value is 3, which is also the default.
- Zoom in, zoom out, and fit.

Hovering a control shows its name: the color name, Copy, Paste, Cut, Undo, Redo, Pen, Eraser, Select, Rectangle, Ellipse, Line, Text, Zoom in, Zoom out, Fit, or Menu.

## Sketch

Sketch is the working surface. The person lands here when the app opens, on a blank tab named Untitled 1, with the pen selected, black current, size 3, and the view centered on the origin at the default zoom. The grid squares are clearly visible, in the spirit of the reference.

One tool is active. Choosing another tool highlights it and commits any in-progress mark. Each open sketch keeps its own marks, selection, pan, zoom, and undo history. The current tool, color, and size belong to the toolbar, so they stay as they are when the person changes tabs.

Tabs show the sketch name and a close control. The active tab is white; inactive tabs are a step grayer, as in the reference. Untitled 1, Notes, and Sketches in the reference are examples of names on open tabs. The new-tab button creates Untitled N, using the smallest positive number not already used by an open tab. Clicking a tab shows that sketch. Tabs stay in the order they were opened. Middle-clicking a tab is the same as its close control. When that sketch is the only one and it is still a clean blank, the close control is lighter and waits. Closing any other last sketch opens a new blank sketch, so the window keeps a canvas.

The pen draws a freehand vector stroke from pointer down to pointer up. When the device reports pressure, lighter pressure draws thinner and full pressure draws at the current size. A mouse, or a stylus that reports no pressure, draws at the full size. On release, the stroke is smoothed enough that small jitters fall away and the gesture remains: a curve stays curved, and a deliberate straight pass stays straight. Smoothing has no separate control.

The eraser drags a path at a constant width of the current size, whether or not the stylus reports pressure. On release it records one operation against the strokes, shape outlines, lines, and text that already existed and that the path crossed. Ink inside that path disappears. The original geometry stays. Marks drawn afterward keep their full ink. Undo drops that operation and the hidden ink returns. An erase operation stays attached to the marks it changed: moving a stroke moves its remaining ink and its hidden portions together. The person selects, recolors, and moves those marks, and uses undo to reverse the erasure. Hit testing uses visible ink. A mark whose ink is fully hidden stays in the sketch, and undo is what restores that ink. Clicks pass through it while it is hidden.

Select clicks visible ink to select that mark, and clicks empty grid to clear the selection. Shift-click adds or removes a mark. Dragging on empty grid draws a marquee and selects marks whose visible ink it crosses, replacing the selection. Shift-drag adds those marks to the selection. A selection shows a simple bounding box. Dragging a selected mark moves the selection. Corner handles scale it from the opposite corner. Stroke points stay as the pen left them. Delete or Backspace removes the selected objects from the sketch. That removal is a whole-object edit, separate from the eraser.

The rectangle, ellipse, and line tools drag an unfilled outline in the current color and size. A rectangle or ellipse uses the drag as its box; a line uses the drag as its end points. Holding Shift constrains a rectangle to a square, an ellipse to a circle, and a line to 45-degree steps. Releasing the pointer commits the shape. Escape during the drag cancels it.

The size slider runs from 1 to 32 in whole steps, default 3. The number beside it shows the value and can be clicked to type a replacement, which is clamped into that range. Size is the stroke width stored with the mark, in canvas units. Zoom changes how thick it looks on screen and leaves the stored width as it was. The slider sets the width of the next pen stroke, eraser path, and shape outline, and the size of the next text label. Marks already on the canvas keep their width. With a selection, clicking a swatch recolors those marks and becomes the current color.

Copy, cut, and paste use an in-app clipboard of sketch marks. Copy is available when something is selected. Cut copies and then deletes those objects. Paste puts a duplicate on the active sketch, slightly offset from the source so it is visible, and selects the duplicate. Repeated paste keeps offsetting. When copy, cut, paste, undo, or redo has nothing to do, its icon is drawn lighter and the control waits. The reference shows them at full strength on a canvas that already has marks.

Each committed gesture is one undo step: a pen stroke, a shape, a text label, an erase, a move, a scale, a recolor, a deletion, a paste, or a cut. A new committed gesture clears redo. Undo and redo belong to the active sketch. Panning and zooming leave that history unchanged.

Zoom in and zoom out step by about one quarter of the current scale, centered on the view, and stop at the limits of 12.5% and 800%. Fit frames the bounds of visible ink with a small margin. When those bounds spread farther than the zoom limit allows, fit uses the nearest limit and centers on them. An empty sketch, and a sketch whose marks are all fully hidden, fits back to the default zoom on the origin. Holding Ctrl or Cmd and scrolling zooms toward the pointer. Scrolling pans vertically; Shift-scroll pans horizontally. Holding Space and dragging, or dragging with the middle button, pans without changing tools. The stylus eraser end, when the device has one, uses the eraser until it lifts, then restores the previous tool.

These shortcuts work when text is not being edited. Ctrl and Cmd are the same:

- P pen, E eraser, V select, R rectangle, O ellipse, L line, T text
- Ctrl+Z undo, Ctrl+Shift+Z or Ctrl+Y redo
- Ctrl+C copy, Ctrl+X cut, Ctrl+V paste
- Ctrl+S save, Ctrl+N new sketch, Ctrl+W close the active sketch
- Ctrl+0 fit, Ctrl++ zoom in, Ctrl+- zoom out
- [ and ] change size by one step
- Arrow keys nudge the selection; Shift makes the nudge larger
- Delete or Backspace deletes the selection
- Escape cancels an in-progress shape, commits text, or clears the selection

Closing the window quits after any sketches with unsaved changes have been resolved. **Keep this sketch?** appears once for each of those sketches, in tab order. Cancel stops the quit and leaves the remaining sketches open. Sketches already saved or discarded in that sequence stay closed.

## Sketch menu

The menu button at the left of the tab bar opens a small menu. It contains New sketch, Open, Save, Rename, and Close. New sketch matches the new-tab button. Save stores the active sketch under its current name so it can be opened again later. Saving again updates that same sketch. Each saved sketch keeps a stable identity, so a rename changes the name shown for that sketch once the rename is saved. There is no published file format and no format chooser, and Save and Open keep the sketch inside Simple Sketch under a stable identity. Save keeps the sketch available inside Simple Sketch. Open lists saved sketches by name. Choosing a row focuses its tab when that sketch is already open, and otherwise opens it. With nothing saved, Open waits, drawn lighter. Two sketches may share a name; each row still opens its own sketch. Rename makes the active tab’s name editable, as does double-clicking the name. Clearing the name restores the previous one. A rename counts as a change to the sketch. Close matches the tab’s close control.

## Text editing

Choosing the text tool and clicking the canvas places a label in the current color and starts editing. The label is a single line of plain sans-serif text. At the default zoom, size 3 reads as a short annotation. Enter or Escape commits it. Clicking elsewhere commits it and, if the text tool is still active, can start another label. An empty label is discarded on commit. Clicking an existing label with the text tool edits it. The select tool moves and scales a committed label. The handwritten lettering in the reference is pen ink. The text tool uses the plain label described here.

## Keep this sketch?

This confirmation appears when a sketch with changes since its last save is closed, and when the sketch name was edited, including a never-saved sketch that has no marks. It names the sketch and offers Save, Discard, and Cancel. Save stores it and then closes it. Discard closes it and drops unsaved changes. Cancel leaves it open and does not add a sketch. A new blank sketch is created only after Save or Discard finishes, and only when that sketch was the last one. The window still ends with one sketch. A clean blank skips this question: no marks, the generated Untitled name, never saved.
