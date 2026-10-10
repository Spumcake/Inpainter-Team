# Simple Sketch interface

Simple Sketch is the desktop window where someone sketches and annotates with a mouse or a pressure-sensitive stylus. Product intent lives in [the pitch](.project/documents/PITCH.md). This document describes that one window: its chrome, the objects a sketch is made of, the tools that make and edit them, and the properties panel that adjusts them.

## Journey and sitemap

The app opens on **Sketch**, an infinite grid with one new document. The journey stays inside that window.

- **Sketch** is the whole app. Drawing, erasing, selecting, panning, and zooming happen here.
- **Properties** is a panel on the right side of Sketch. It opens and closes over the canvas area without leaving Sketch. With the select tool its subject is **Layers**, the outliner.
- The menu button opens **Sketch menu**, a short menu for creating, opening, saving, renaming, and closing sketches. Choosing one returns to Sketch.
- The text tool enters **Text editing** on the canvas, then returns to Sketch when the text is committed.
- Closing a sketch that has changes since its last save, or whose name was edited, opens **Keep this sketch?**. A clean new sketch skips it. Cancel stays on that sketch. Save and Discard close it and leave the person on another open sketch. A new sketch is created only after Save or Discard finishes, and only when that sketch was the last one, so the window still ends with one sketch.

## References

The renders in `.project/renders/ui/` are the accepted appearance:

- [base-ui.png](.project/renders/ui/base-ui.png) — the window, tabs, and toolbar with the panel closed.
- [brush-property-ui.png](.project/renders/ui/brush-property-ui.png) and [eraser-tool-property-ui.png](.project/renders/ui/eraser-tool-property-ui.png) — the brush and eraser properties.
- [square-tool-property-ui.png](.project/renders/ui/square-tool-property-ui.png) and [circle-tool-property-ui.png](.project/renders/ui/circle-tool-property-ui.png) — the rectangle and ellipse properties.
- [text-tool-property.png](.project/renders/ui/text-tool-property.png) — the text properties.
- [color-property-ui.png](.project/renders/ui/color-property-ui.png) and [extra-color-properties-ui.png](.project/renders/ui/extra-color-properties-ui.png) — one palette color's properties.
- [selected-object-and-outliner-ui.png](.project/renders/ui/selected-object-and-outliner-ui.png) — the select tool, its context actions, and the Layers outliner.

The ink in the renders is sample drawing; a new sketch has none. The renders do not draw the sketch menu, the text caret, the save confirmation, an empty canvas, or the canvas bounding box. Where a render and this document differ, this document holds. These render details are not adopted:

- The outliner render drops the tool buttons, size, and zoom from the toolbar. Here the select tool's context actions replace only the size control.
- The square and circle renders omit the text tool from the toolbar.
- Brush "Pressure affects flow" and eraser "Pressure affects opacity" are not in the first version.
- The shape renders list Constrain proportions twice. It appears once.
- The color renders' Join control and its Use for stroke and Use for fill toggles are not adopted. Freehand ink has no joins, and palette colors belong to canvas strokes only.
- Rendered values, such as the shape fill at 40% or a font size of 36, are examples, not defaults. Sizes are given under Size below, and the other defaults are technical choices in [systems.md](.project/documents/development/systems.md).

## Presentation

The window is light and compact. A white title bar carries a small orange pencil and the words Simple Sketch, with minimize, maximize, and close at the right. The title bar stays light on a dark desktop. Below it, a pale bar holds a menu button, document tabs, and a new-tab button. The active tab is white and joins the white toolbar beneath it; inactive tabs are a step grayer. The rest of the window is the canvas: a near-white surface with a faint square grid that pans and zooms with the drawing. The grid is a guide.

Toolbar groups are separated by thin vertical dividers, in this order:

- The active canvas's palette swatches, in palette order. The first swatch is the top layer. The active color is drawn larger, with a ring in its own color and a sliver of toolbar between ring and fill. Hidden or locked colors are drawn lighter.
- New sketch, Open, and Save.
- Undo and redo.
- Brush, eraser, select, rectangle, ellipse, line, and text.
- The word Size, a slider, and the current value as a number, on one row. With the select tool, its context actions take this place.
- Zoom out, zoom in, fit, and one button that toggles the maximized state, at the far end.

Icons are simple dark line icons from Iconoir. The current tool sits on a light gray rounded square. A control with nothing to do is drawn lighter and waits. Hovering a control shows its name and shortcut.

The properties panel is a white column on the right, below the toolbar, with a hairline at its left edge. Its header has a title, a close button, and a subject card: an icon, the subject's name, and one line saying what the settings affect. Sections follow, each with a small bold heading. Controls are toggles, sliders paired with a number field, color wells, dropdowns, and segmented buttons, as in the renders. The canvas narrows while the panel is open. It does not slide under it.

## Sketch

The person lands on a new tab named Untitled 1. It holds one empty canvas object named Canvas 1, highlighted, with the default palette and black active. The brush is the current tool. The view is centered on the origin at the default zoom, and the panel is closed.

A sketch is a list of objects, top to bottom: canvases, rectangles, ellipses, lines, text, and groups of those. Objects higher in the list paint above lower ones. Each object has a name, an eye that hides it, and a lock that keeps it from being selected on the canvas or changed. Each open sketch keeps its own objects, selection, view, and undo history. The current tool, the panel's open state, and tool settings belong to the window, so they stay as they are when the person changes tabs.

Tabs show the sketch name and a close control. The new-tab button creates Untitled N, using the smallest positive number not already used by an open tab. Clicking a tab shows that sketch. Tabs stay in the order they were opened. Middle-clicking a tab is the same as its close control. When that sketch is the only one and it is still clean, the close control is lighter and waits. Closing any other last sketch opens a new sketch, so the window keeps one. A clean sketch has only its starting canvas, with no strokes, the default palette, and the generated Untitled name, and has never been saved.

One tool is current. Choosing another commits any in-progress mark. The brush and the eraser are available only while exactly one canvas is highlighted and that canvas is visible and unlocked. Otherwise their buttons are drawn lighter and wait, and their hover text says to highlight a canvas. If the highlight changes away from a single canvas while the brush or eraser is current, the current tool becomes select. The other tools are always available.

## Canvases and palettes

A canvas object holds freehand ink. It has no fixed size: its bounding box is always the bounds of its visible ink plus its padding, a canvas property. A canvas with no visible ink has no box and is reached through the outliner. While a canvas is highlighted with the brush or eraser current, its box shows as a faint dashed outline without handles. With the select tool it shows the usual selection box.

Each canvas has its own palette, an ordered list of colors. Each color is a layer of that canvas. A color owns its name, its value, its opacity, its stroke size, its dash style (solid or dashed), its cap (round or flat), and whether it is visible and locked. Strokes drawn in a color take all of those from it, so editing a color changes every stroke drawn in it. A color's strokes paint above the strokes of every color after it in the palette. Within one color, newer strokes are above older ones. A color's opacity applies to the layer as a whole, so its overlapping strokes do not darken each other.

Clicking a swatch makes that color active on the highlighted canvas. Each canvas remembers its own active color. Choosing a color does not change strokes already drawn. Right-clicking a swatch opens the properties panel on that color. The palette is sorted by dragging its color rows in the outliner, or with Move up and Move down in the color's properties. The swatch order follows at once.

A new canvas gets a copy of the default palette. The default palette is black, red, orange, yellow, green, blue, purple, pink, and light gray, in that order, each at size 3, full opacity, solid, with round caps. Black is the top layer. A canvas's properties can save its palette as the default for canvases created later. Editing a color, adding one, removing one, and sorting the palette are changes to the sketch. A canvas always keeps at least one color.

New canvas, in the outliner header and as a shortcut, adds an empty canvas named Canvas N at the top of the sketch and highlights it.

## Brush

The brush draws a freehand stroke on the highlighted canvas, in its active color, from pointer down to pointer up. It waits when the active color is hidden or locked. When the device reports pressure and Pressure affects size is on, lighter pressure draws thinner and full pressure draws at the color's size. The pressure curve reshapes the device's pressure before it is used. A mouse, a stylus that reports no pressure, or Pressure affects size off draws at the color's full size.

Smoothing, stabilization, and streamline shape the outline so small jitters fall away and the gesture remains: a curve stays curved, and a deliberate straight pass stays straight. Stabilization steadies a shaky hand by averaging recent samples. A stroke keeps the brush settings it was drawn with, so later changes to the brush do not alter it. It keeps its samples exactly as the device reported them.

## Eraser

The eraser drags a path at the eraser's size across the highlighted canvas. When Pressure affects size is on and the device reports pressure, lighter pressure erases a narrower path. On release it records one operation against the strokes on that canvas that the path crossed, in its visible, unlocked colors. Ink inside the path disappears, and the strokes stay whole underneath. Strokes drawn afterward keep their full ink. Undo drops the operation and the ink returns. Erased regions move with the canvas. If a color's size later grows, a stroke can show ink just beyond the edge of a region erased before.

The eraser touches only freehand ink on the highlighted canvas. Shapes and text are whole objects: they are deleted, not erased. The stylus eraser end, when the device has one, uses the eraser until it lifts, then restores the previous tool. It does nothing while the eraser would be unavailable.

## Select, layers, and objects

The select tool works on whole objects. Clicking an object's visible ink, or a shape's fill, selects that object. A click inside a group selects the group. Clicking empty canvas clears the selection. Shift-click adds or removes an object. Dragging on empty canvas draws a marquee that selects the objects whose visible ink it crosses, replacing the selection; Shift-drag adds to it. Locked and hidden objects are not picked on the canvas. Selecting a canvas highlights it, and the brush and eraser draw there once chosen.

A selection shows its bounding box with corner handles and a rotation handle above the top edge. Dragging inside moves the selection. Dragging a corner scales from the opposite corner; Shift keeps proportions. Dragging the rotation handle rotates around the box center; Shift steps by 15 degrees. Arrow keys nudge by 1 unit, or by 8 with Shift. Delete or Backspace removes the selected objects.

With the select tool, the size control's place holds the context actions: Bring forward and Send backward, Align left, Align center, and Align right, then Zoom to selection, Group, and Ungroup, then a Rotate menu and a Flip menu. Rotate turns the selection 90 degrees clockwise, 90 degrees counterclockwise, or 180 degrees. Flip mirrors it horizontally or vertically. Aligning lines up two or more selected objects on the leftmost edge, the shared center, or the rightmost edge. Group puts the selected objects in a new group named Group N, at the position of the topmost one. Ungroup releases a group's objects in place. Actions with nothing to do are drawn lighter.

**Layers**, the outliner, lists the sketch's objects top to bottom, as in the outliner render. Each row has an icon for its kind, its name, an eye, and a lock. Groups are folders that expand and collapse. A canvas row expands to its colors, each with a dot of its color, its name, an eye, and a lock. Clicking a row selects that object. Shift-click adds to the selection. Clicking a color row highlights its canvas and makes that color active. Double-clicking a name renames it. Dragging object rows reorders them or moves them into or out of a group. Dragging color rows reorders that canvas's palette. The selection on the canvas and in the outliner is always the same. The outliner header has New canvas.

## Shapes

The rectangle, ellipse, and line tools drag a new shape object, placed at the top of the sketch. A rectangle or ellipse uses the drag as its box, and a line uses the drag as its end points. With Constrain proportions on, or with Shift held when it is off, a rectangle is a square, an ellipse is a circle, and a line moves in 45-degree steps. With Snap to grid on, the drag's corners or end points land on grid intersections. Releasing commits the shape. Escape during the drag cancels it.

A shape takes the tool's settings when it is drawn and keeps its own afterward. Rectangles and ellipses have a fill and a stroke. Lines have a stroke. The stroke can be solid or dashed. Rectangles have a corner radius. A selected shape's settings are edited in the panel under Layers.

## Text editing

Clicking the canvas with the text tool places an auto-sized text object and starts editing. Dragging places a text box of that width. Text is typed at the caret. Enter starts a new line. Escape, or a click elsewhere, commits it, and a click elsewhere with the text tool still current starts another. An empty text object is discarded on commit. Clicking an existing text object with the text tool edits it in place.

Text uses the tool's font family, font size, bold, italic, underline, color, opacity, optional background fill, alignment, line height, letter spacing, and all caps. With Auto-size on, the box grows to fit the text. With Wrap text on, lines wrap at the box width. The font families are the ones the app ships. The handwritten lettering in the renders is brush ink.

## Properties panel

The panel opens and closes with F4 and with its close button. Right-clicking a tool button opens the panel on that tool's properties without changing the current tool. Right-clicking a swatch opens it on that color. While the panel is open and the current tool changes, the panel follows the new tool.

The panel shows one subject, named in its header:

- **Tool: Brush.** Preset, Dynamics (Pressure affects size), Pressure curve, and Behavior (Smoothing, Stabilization, Streamline).
- **Tool: Eraser.** Preset, Size, Dynamics (Pressure affects size), Pressure curve, and Behavior, with the same controls as the brush.
- **Tool: Rectangle**, **Tool: Ellipse**, and **Tool: Line.** Fill (on or off, color, opacity) for rectangles and ellipses; Stroke (color, width, opacity); Shape style (solid or dashed, and corner radius for rectangles); Constrain proportions; and Snap to grid.
- **Tool: Text.** Text (font family, font size, bold, italic, underline), Color and appearance (color, opacity, background fill and its color), Layout (alignment, line height, letter spacing), and Behavior (all caps, auto-size, wrap text).
- **Color.** Color (name, hex, a color model of RGB, HSL, or HSV with its three sliders, and opacity), Stroke (size, solid or dashed, round or flat cap), Layer (visible, locked, Move up, Move down), Related shades, and Duplicate and Delete. Duplicate adds a copy directly below. Delete removes the color and its strokes and is unavailable on a canvas's last color.
- **Layers**, with the select tool. The outliner, and under it the selection's own settings. A shape or text object shows the same sections as its tool. A canvas shows its name, its padding, and Save palette as default. With several objects selected, only the sections they share are shown, and a value that differs between them reads as mixed until it is set.

Tool settings are defaults for the next mark. They belong to the window, are kept between sessions, and are not undo steps. Settings of an object or a palette color change the sketch, and each finished edit is one undo step. A slider drag counts as one edit when it is released. Color wells offer the highlighted canvas's palette colors as quick choices. Choosing one copies the value.

Brush and eraser presets are named sets of dynamics and behavior. The first version ships Round Brush, with pressure on, and Monoline, with pressure off, for the brush, and Round Eraser for the eraser. When the settings match no preset, the preset reads Custom.

## Size

The Size control edits the size the current tool uses next, and its range follows that size:

- With the brush, it edits the active color's stroke size, from 1 to 32. That is the color's own size, so every stroke in that color redraws at the new size, and the change is one undo step.
- With the eraser, it edits the eraser size, from 1 to 32. The default is 8.
- With a shape tool, it edits that tool's stroke width, from 1 to 32. The default is 3.
- With the text tool, it edits the font size, from 8 to 144. The default is 24.

The number beside the slider can be clicked to type a value, which is clamped into the range. Sizes are in canvas units. Zoom changes how thick they look on screen, not the stored value.

## History and clipboard

Each finished change to the sketch is one undo step: a stroke, an erase, a new object or canvas, a deletion, a move, a scale, a rotation, a flip, an alignment, a group or ungroup, a reorder, a renamed object, a visibility or lock change, a property edit, a palette edit, a paste, a cut, or a committed text edit. A new step clears redo. Undo and redo belong to the active sketch. Panning, zooming, selection, the active color, and tool settings leave history unchanged.

Copy, cut, and paste use an in-app clipboard of whole objects. A copied canvas brings its palette and strokes. Copy is available when something is selected. Cut copies and then deletes. Paste puts the duplicates at the top of the sketch, slightly offset from the source, and selects them. Repeated paste keeps offsetting. Copy, cut, and paste are shortcuts, not toolbar icons.

## View

Zoom in and zoom out step by about one quarter of the current scale, centered on the view, and stop at 12.5% and 800%. Fit frames the bounds of all visible objects with a small margin. When those bounds spread farther than the zoom limit allows, fit uses the nearest limit and centers on them. A sketch with nothing visible fits back to the default zoom on the origin. Zoom to selection does the same for the selection. Holding Ctrl or Cmd and scrolling zooms toward the pointer. Scrolling pans vertically; Shift-scroll pans horizontally. Holding Space and dragging, or dragging with the middle button, pans without changing tools. A press on the canvas while the sketch menu is open only closes the menu.

## Shortcuts

These work when text is not being edited. Ctrl and Cmd are the same. Each control's hover text shows its shortcut from the same list.

- B brush, E eraser, V select, R rectangle, O ellipse, L line, T text
- F4 show or hide the properties panel
- Ctrl+Shift+N new canvas
- Ctrl+G group, Ctrl+Shift+G ungroup, Ctrl+] bring forward, Ctrl+[ send backward
- Ctrl+Z undo, Ctrl+Shift+Z or Ctrl+Y redo
- Ctrl+C copy, Ctrl+X cut, Ctrl+V paste
- Ctrl+N new sketch, Ctrl+O open, Ctrl+S save, Ctrl+W close the active sketch
- Ctrl+0 fit, Ctrl++ zoom in, Ctrl+- zoom out
- [ and ] change the current size by one step
- Arrow keys nudge the selection; Shift makes the nudge larger
- Delete or Backspace deletes the selection
- Escape cancels an in-progress shape, commits text, or, with the select tool, clears the selection

Closing the window quits after any sketches with unsaved changes have been resolved. **Keep this sketch?** appears once for each of those sketches, in tab order. Cancel stops the quit and leaves the remaining sketches open. Sketches already saved or discarded in that sequence stay closed.

## Sketch menu

The menu button at the left of the tab bar opens a small menu: New sketch, Open, Save, Rename, and Close. New sketch matches the new-tab button and the New sketch icon. Save stores the active sketch under its current name, and saving again updates that same sketch. Each saved sketch keeps a stable identity, so a rename changes the name shown for that sketch once the rename is saved. There is no published file format and no format chooser. Open lists saved sketches by name. Choosing a row focuses its tab when that sketch is already open, and otherwise opens it. With nothing saved, Open waits, drawn lighter. Two sketches may share a name; each row still opens its own sketch. Rename makes the active tab's name editable, as does double-clicking the name. Clearing the name restores the previous one. A rename counts as a change to the sketch. Close matches the tab's close control. The toolbar's Open and Save icons match their menu items.

## Keep this sketch?

This confirmation appears when a sketch with changes since its last save is closed, and when the sketch name was edited. It names the sketch and offers Save, Discard, and Cancel. Save stores it and then closes it. Discard closes it and drops unsaved changes. Cancel leaves it open and does not add a sketch. A new sketch is created only after Save or Discard finishes, and only when that sketch was the last one. A clean sketch skips this question.
