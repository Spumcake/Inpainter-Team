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
- [color-property-ui.png](.project/renders/ui/color-property-ui.png) — one palette color's properties. This render alone defines the color page's sections, for every palette: a tool's, a canvas's, a shape's, or a text object's.
- [update/brush-property-ui.png](.project/renders/ui/update/brush-property-ui.png), [update/eraser-property-ui.png](.project/renders/ui/update/eraser-property-ui.png), and [update/color-property-ui.png](.project/renders/ui/update/color-property-ui.png) — the panel chrome the owner accepted on 2026-10-11. It applies to every properties page, including the shape, text, and Layers pages that are not built yet. A small tab on the panel's left edge shows and hides the panel, and F4 does the same. There is no close button, no Properties header, and no tool-name and description card. The panel opens on its first section.
- [selected-object-and-outliner-ui.png](.project/renders/ui/selected-object-and-outliner-ui.png) — the select tool, its context actions, and the Layers outliner.

The ink in the renders is sample drawing; a new sketch has none. The renders do not draw the sketch menu, the text caret, the save confirmation, an empty canvas, or the canvas bounding box. Where a render and this document differ, this document holds. These render details are not adopted:

- The outliner render drops the tool buttons, size, and zoom from the toolbar. Here the select tool's context actions replace only the size control.
- The renders draw the brush tool as a pencil and have no New canvas button in the toolbar. Here the brush is a paintbrush and New canvas sits just before it.
- The outliner render has a header row with New canvas. Here the outliner has no header.
- The square and circle renders omit the text tool from the toolbar.
- Brush "Pressure affects flow" and eraser "Pressure affects opacity" are not in the first version.
- The shape renders list Constrain proportions twice. It appears once.
- Older renders still draw a Properties header, a close button, and a tool-name and description card. The Layers render still draws a header and a close button. Those drawings do not override this decision, so a later page is not built from them, including the shape, text, and Layers pages that are not built yet.
- The color render's Join control and its third Cap option are not adopted. Brush ink is outlined with round joins and round or flat caps, and those two controls would need a different outline builder.
- [extra-color-properties-ui.png](.project/renders/ui/extra-color-properties-ui.png) is kept as a possible future layout for more color settings. It is not part of the first version, and nothing in this document is drawn from it.
- Rendered values, such as the shape fill at 40% or a font size of 36, are examples, not defaults. Sizes are given under Size below, and the other defaults are technical choices in [systems.md](.project/documents/development/systems.md).

## Presentation

The window is light and compact. A title bar carries a small orange pencil and the words Simple Sketch, with minimize, maximize, and close at the right. It follows the desktop's light or dark theme, so it is white in the renders and dark on a dark desktop. The rest of the window stays light. Below it, a pale bar holds a menu button, document tabs, and a new-tab button. The active tab is white and joins the white toolbar beneath it; inactive tabs are a step grayer. The rest of the window is the canvas: a near-white surface with a faint square grid that pans and zooms with the drawing. The grid is a guide.

When the toolbar is wider than the window, everything before the zoom group scrolls sideways: the mouse wheel turns it without Shift, and dragging the row moves it, with an open hand over it and a closed hand while dragging. A swatch, a slider, and an enabled button keep the pointer: no hand shows over them, and a drag that starts on one never scrolls the row, so sorting swatches and moving a slider work as before. A drag that starts on a disabled button, a divider, or a gap scrolls it. The zoom group stays at the right edge. Toolbar groups are separated by thin vertical dividers, in this order:

- The current tool's palette swatches, in palette order. Changing the tool changes the swatches. With the brush, the active color is drawn larger, with a ring in its own color and a sliver of toolbar between ring and fill. A swatch whose color would not draw is drawn lighter, and a near-white swatch has a faint edge so it does not vanish into the toolbar. A swatch that would not draw is: a brush color whose layer on the highlighted canvas is hidden or locked, or a shape's fill or a text background that is off. The eraser and select tools have no palette, so this group and the divider after it are left out.
- New sketch, Open, and Save.
- Undo and redo.
- New canvas, a plus in a circle, then the brush, eraser, select, rectangle, ellipse, line, and text tools.
- The current tool's settings for its next mark, or with the text tool for the text it is working on. For the brush, eraser, and shape tools that is the word Size, a slider, and the current value as a number, on one row. The text tool adds, after Size: Align left, Align center, Align right, and Justify as four buttons, then Align top, Align middle, and Align bottom as three more (Iconoir's align-top-box, align-horizontal-centers, and align-bottom-box), the chosen one of each set blue; Padding and Radius sliders for its box; and the list button. At 1280 points wide this row overflows and scrolls, as described above. With the select tool, its context actions take this place.
- Zoom out, zoom in, fit, and one button that toggles the maximized state, at the far end.

Text is set in Inter: regular weight for labels and values, semibold for section headings. The one accent is near-black, the color of the Size slider's knob and fill when it is not following a palette color; sliders, switches that are on, the pressure curve, and chosen segments use it, and the only blue in the window is the current tool's icon. Icons are simple dark line icons from Iconoir. Iconoir has no brush, so the brush tool's icon is a paintbrush drawn in the same line style. The current tool's icon is drawn in blue, and so is any toolbar button's icon under the pointer, a chosen setting such as the text's alignment, and the button of a toolbar menu while it is open. Toolbar and tab bar buttons have no background of their own, chosen or hovered. A control with nothing to do is drawn lighter and waits. Hovering a control shows its name and shortcut.

The properties panel is a white column on the right, below the toolbar, with a hairline at its left edge. Every properties page uses this chrome, including shape, text, and Layers. It has no Properties header, no close button, and no tool-name and description card: it opens on its first section, and each section has a small bold heading. A small white tab with a chevron sits at the panel's left edge, near its top, and joins the panel. While the panel is closed, the same tab sits at the canvas's right edge. Clicking it shows or hides the panel; its chevron points the way the panel will move. Controls are toggles, sliders paired with a number field, color wells, dropdowns, and segmented buttons, as in the renders. The canvas narrows while the panel is open. It does not slide under it. Every row sits on one grid: a muted label in a fixed column, then the control, then, for a number, its value in a field at the right edge. Values, text, dropdowns, and segmented choices sit in white fields with a faint border and rounded corners, with their text left-aligned. Sliders match the toolbar's Size slider. Sections are separated by a hairline. The panel has no scroll bars: a page longer than the panel scrolls with the wheel or by dragging its background, which shows an open hand, and a closed hand while it is dragged. Rows, fields, switches, buttons, and sliders keep the pointer: no hand shows over them, and a drag that starts on one does not scroll the page. The panel draws its contents a size smaller than the toolbar's controls, at seven eighths of their size, as the owner set on 2026-10-11, and is 266 points wide so its right margin matches its left. Every menu is drawn at full size. The outliner's rows act like buttons rather than value fields, so they are larger still: rows of about 35 points, icons near the toolbar's size, and 15-point names.

## Sketch

The person lands on a new tab named Untitled 1. It holds one empty canvas object named Canvas 1, highlighted, whose palette is a copy of the brush's. The brush is the current tool, with black active. The view is centered on the origin at the default zoom, and the panel is closed.

A sketch is a list of objects, top to bottom: canvases, rectangles, ellipses, lines, text, and groups of those. Objects higher in the list paint above lower ones. Each object has a name, an eye that hides it, and a lock that keeps it from being selected on the canvas or changed. Each open sketch keeps its own objects, selection, view, and undo history. The current tool, the panel's open state, and tool settings, including each tool's palette and the brush's active color, belong to the window, so they stay as they are when the person changes tabs.

Tabs show the sketch name and a close control. The new-tab button creates Untitled N, using the smallest positive number not already used by an open tab. Clicking a tab shows that sketch. Tabs stay in the order they were opened. Middle-clicking a tab is the same as its close control. When that sketch is the only one and it is still clean, the close control is lighter and waits. Closing any other last sketch opens a new sketch, so the window keeps one. A clean sketch has only its starting canvas, with no strokes, its palette as it was copied, and the generated Untitled name, and has never been saved.

One tool is current. Choosing another commits any in-progress mark. Every tool can always be chosen, and choosing a canvas or anything else never changes the current tool. The selection follows what the person is working with: a finished stroke or erase selects its canvas, and a new shape or text object is selected. Choosing the brush or eraser selects the highlighted canvas, and choosing the select tool keeps whatever was made last selected. With no visible, unlocked canvas highlighted, a brush stroke makes a new canvas, named Canvas N, at the top of the sketch, as a shape tool makes a new shape; the stroke and the new canvas are one undo step, and the new canvas is highlighted. With no canvas highlighted, the eraser erases nothing.

## Palettes

Everything that has color has a palette: an ordered list of colors, organized like the layers of a canvas. Every palette color is the same kind of thing. It has a name, a value, an opacity, a stroke size, a dash style (solid or dashed), and a cap (round or flat), and every one opens the same color page. A color with no stroke, such as a fill, uses only its value and opacity.

**Each creation tool has its own palette.** These palettes are tool settings: they belong to the window, are kept between sessions, and are not undo steps.

- The brush has nine colors to start: black, red, orange, yellow, green, blue, purple, pink, and light gray, in that order, each at size 3, full opacity, solid, with round caps. Colors can be added, duplicated, deleted, and sorted.
- The rectangle and the ellipse each have two: Outline and Fill.
- The line has one: Stroke.
- The text tool has three: Text, Background, and Border.
- The eraser and the select tool have none.

The shape and text palettes keep those fixed roles. Their colors cannot be added, deleted, or sorted, and their names do not change. A Fill or Background color that is off is not drawn; turning it on is its eye.

**Each object has its own palette, copied from its tool when it is made.** A new canvas copies the brush's palette, and those colors are the canvas's layers. A new rectangle copies the rectangle tool's Outline and Fill, and so on for the ellipse, the line, and text. After that the object's palette is its own: changing a tool's palette changes only what that tool makes next, and changing an object's palette is a change to the sketch.

**The toolbar sets up what comes next.** Everything after the tools in the toolbar, and the swatches at its start, adjusts the next mark or object the current tool makes. It never changes something already in the sketch. To change something that exists, select it and use the panel. Text is the exception, as the owner set on 2026-10-12: with the text tool, the toolbar's Size, alignment, Padding, Radius, and list change the text being edited, or the one selected text, as one undo step each (a slider's whole drag is one), and the panel shows that text. A new text keeps itself selected after it is committed. New text being typed is drawn with the tool's settings, so there the toolbar changes the tool's settings and the new text with them. Escape finishes typing; a second Escape lets go of the text, and the toolbar then sets up the next text. The swatches still open the tool's colors.

Clicking a brush swatch makes that color active. Clicking a shape or text swatch opens that color's page in the panel. Right-clicking a swatch opens a menu styled like the other menus: Duplicate and Delete for a brush color, then Properties, which opens the panel on that color's page. A shape's or text's swatch offers only Properties, since its colors are fixed roles. Duplicate adds a copy directly after the color. Delete is drawn lighter on the last color: the brush always keeps at least one. The brush's palette is sorted by dragging a swatch along the toolbar and dropping it in its new place; a drag that stays within the swatch is still a click.

## Canvases

A canvas object holds freehand ink. It has no fixed size: its bounding box always hugs its visible ink, plus a padding of 16 units. A rotated or scaled canvas's box hugs the ink as it now lies, not the corners of the unrotated box, and the padding scales with the canvas. A canvas with no visible ink has no box and is reached through the outliner. While a canvas is highlighted with the brush or eraser current, its box shows as the faint dashed outline described under Selection. With the select tool it shows the usual selection box.

A canvas's palette colors are its layers. Each layer also has its own eye and lock. Strokes take their value, opacity, dash, and cap from their layer, so editing a layer changes every stroke in it. A stroke drawn while its brush color's size was locked also follows the layer's size; a stroke drawn at a size set on the slider keeps that size. A layer's strokes paint above the strokes of every layer after it in the palette. Within one layer, newer strokes are above older ones. A layer's opacity applies to the layer as a whole, so its overlapping strokes do not darken each other.

Each layer remembers the brush color it was copied from. A brush stroke goes into the highlighted canvas's layer that came from the active brush color. If the canvas has no such layer, because that brush color was added after the canvas was made, the stroke adds one to the bottom of the canvas's palette, copied from the brush color. Editing, adding, removing, and sorting a canvas's layers are changes to the sketch, made from the outliner and the panel. A canvas always keeps at least one layer.

Make palette default, in a canvas row's right-click menu, copies that canvas's palette into the brush's palette, so canvases made later start with it. That canvas's layers then come from the new brush colors.

New canvas, in the toolbar just before the brush and as a shortcut, adds an empty canvas named Canvas N at the top of the sketch and highlights it.

## Brush

The brush draws a freehand stroke on the highlighted canvas, or on a new one when none is highlighted, from pointer down to pointer up, into the layer that came from the active brush color, as described under Canvases. When that layer is hidden or locked, the drag draws nothing. The stroke's size is the one the Size slider shows: the layer's size while the brush color's size is locked, or the size set on the slider. When the device reports pressure and Pressure affects size is on, lighter pressure draws thinner and full pressure draws at that size. The pressure curve reshapes the device's pressure before it is used. A mouse, a stylus that reports no pressure, or Pressure affects size off draws at the full size.

Smoothing, stabilization, and streamline shape the outline so small jitters fall away and the gesture remains: a curve stays curved, and a deliberate straight pass stays straight. Stabilization steadies a shaky hand by averaging recent samples. A stroke keeps the brush settings it was drawn with, so later changes to the brush do not alter it. It keeps its samples exactly as the device reported them.

## Eraser

The eraser drags a path at the eraser's size across the highlighted canvas. When Pressure affects size is on and the device reports pressure, lighter pressure erases a narrower path. On release it records one operation against the strokes on that canvas that the path crossed, in its visible, unlocked colors. Ink inside the path disappears, and the strokes stay whole underneath. Strokes drawn afterward keep their full ink. Undo drops the operation and the ink returns. Erased regions move with the canvas. If a layer's size later grows, a stroke that follows it can show ink just beyond the edge of a region erased before.

The eraser touches only freehand ink on the highlighted canvas. Shapes and text are whole objects: they are deleted, not erased. The stylus eraser end, when the device has one, uses the eraser until it lifts, then restores the previous tool. It does nothing while the eraser would be unavailable.

## Select, layers, and objects

The select tool works on whole objects. Right-clicking an object, or anywhere inside the box of the current selection, opens a menu with Properties, and double-clicking a text object edits it in place without leaving the select tool. Clicking an object's visible ink, or a shape's fill, selects that object. A click inside a group selects the group. Clicking empty canvas clears the selection. Shift-click adds or removes an object. Dragging on empty canvas draws a marquee that selects the objects whose visible ink it crosses, replacing the selection; Shift-drag adds to it. Locked and hidden objects are not picked on the canvas. Selecting a canvas highlights it, and the brush and eraser draw there once chosen. Selecting a shape or text, or making one, leaves the highlighted canvas as it was.

A selection shows its bounding box as in the shape renders: a solid outline, white square handles at the four corners and the middle of each edge, and a small dot on a short stem above the top edge for rotation. Dragging inside moves the selection. Dragging a corner scales from the opposite corner; Shift keeps proportions. Dragging an edge handle scales only across that edge, from the opposite edge; Shift scales the other direction by the same amount. On one selected text object the edge handles change its box instead, as the text tool's do: the left and right its width, so the words re-wrap, and the top and bottom its height. Dragging the rotation handle rotates around the box center; Shift steps by 15 degrees. The pointer shows what a spot does: a resize arrow along the handle's direction over a corner or edge handle and while dragging it, an open hand over the rotation dot and a closed hand while rotating, and the move arrows inside the selection. Arrow keys nudge by 1 unit, or by 8 with Shift. Delete or Backspace removes the selected objects.

With any other tool, what that tool is working on shows a faint dashed outline a few points outside it, without the select tool's handles: the highlighted canvas with the brush or eraser, the shape just drawn with its shape tool, and with the text tool the new text being typed, the text being edited, or the one selected text. The text tool's outline also has side handles, as described under Text editing.

With the select tool, the size control's place holds the context actions, as one run of buttons with no dividers, because they all act on the selection. They are the toolbar's own buttons, at the same size and spacing as the tools: Bring to front, Send to back, Align left, Align center, Align right, Group, Ungroup, then Rotate and Flip, which show a small chevron and open a menu whose rows have an icon and a label. Bring to front is Iconoir's sort-up and Send to back its sort-down; Group is exclude and Ungroup is combine. Bring forward, Send backward, and Zoom to selection are shortcuts only. Rotate turns the selection 90 degrees clockwise, 90 degrees counterclockwise, or 180 degrees. Flip mirrors it horizontally or vertically. Text is not turned or mirrored, as the owner set on 2026-10-12: while the selection holds text, alone or inside a group, Rotate and Flip are left out of the toolbar, their shortcuts do nothing, and the selection box has no rotation dot. Aligning lines up two or more selected objects on the leftmost edge, the shared center, or the rightmost edge. Group puts the selected objects in a new group named Group N, at the position of the topmost one. Ungroup releases a group's objects in place. Actions with nothing to do are drawn lighter.

**Layers**, the outliner, lists the sketch's objects top to bottom, as in the outliner render. Each row has an icon for its kind, its name, an eye, and a lock. Groups are folders that expand and collapse, open at first. A canvas row expands to its layers, collapsed at first, each with a dot of its color, its name, an eye, and a lock. Clicking a row selects that object. Shift-click adds to the selection. Clicking a layer row highlights its canvas and opens that layer's color page in the panel. Double-clicking a canvas, group, or color name edits it in place; Enter or clicking away keeps the new name, Escape cancels, and a blank name is refused. Right-clicking a row opens a menu styled like the other menus, as wide as its longest item with the same space on either side: a canvas row offers Rename canvas, Make palette default, and Properties; a group row offers Rename group and Properties; a shape or text row offers Properties; and a layer row offers Rename, Duplicate, Delete, and Properties. Deleting a layer removes its strokes with it, and is drawn lighter on a canvas's last layer. Dragging object rows reorders them or moves them into or out of a group. Dragging color rows reorders that canvas's palette. The selection on the canvas and in the outliner is always the same. The outliner has no header row; its first row lines up with the first heading of the other pages.

## Shapes

The rectangle, ellipse, and line tools drag a new shape object, placed at the top of the sketch. A rectangle or ellipse uses the drag as its box, and a line uses the drag as its end points. With Constrain proportions on, or with Shift held when it is off, a rectangle is a square, an ellipse is a circle, and a line moves in 45-degree steps. With Snap to grid on, the drag's corners or end points land on grid intersections. Releasing commits the shape. Escape during the drag cancels it.

A shape takes the tool's settings and a copy of the tool's palette when it is drawn, and keeps its own afterward. Rectangles and ellipses have an Outline and a Fill color. Lines have a Stroke color. The outline or stroke width is that color's size; it can be solid or dashed, and a line's can have round or flat caps. Fill starts off. Rectangles have a corner radius. A selected shape's settings and palette are edited in the panel under Layers.

## Text editing

Clicking the canvas with the text tool places an auto-sized text object and starts editing. Dragging places a text box of that width. Text is typed at the caret. Enter starts a new line. Escape, or a click elsewhere on the canvas, commits it, and a click elsewhere with the text tool still current starts another. A click on the toolbar or the panel does not commit it, so a switch or setting there changes the text being typed, and typing then continues at the caret; a panel field that takes typing, such as Hex, keeps the keys until it is done. New text draws its Background while it is typed, as it will once committed. An empty text object is discarded on commit. Clicking an existing text object with the text tool edits it in place. While it is being edited, the panel shows that object rather than the text tool's page, and its settings change it.

Text uses the tool's font family, font size, bold, italic, underline, alignment (left, center, right, or justified), list, line height, letter spacing, padding, corner radius, and all caps, and a copy of the tool's palette: the Text color and its opacity, the Background color, which starts off, and the Border, a line around the box (not around each letter), which also starts off. The Border has a size and a solid or dashed style on its color page, drawn at the letters' scale like the padding and centered on the box's edge, following its rounded corners. A text tool palette saved before the Border existed keeps its two colors and gains it. With Auto-size on, the box grows to fit the text. With Wrap text on, lines wrap at the box width. With Auto-size off on the text page, a click places a box 240 units wide whose lines wrap. The text tool's dashed outline is always also a box control, with no button to turn it on, as the owner set on 2026-10-12: white square handles sit at the middle of its four sides, the pointer is a resize arrow over them, and the side opposite the one dragged stays put. A left or right handle sets the box's width so the words re-wrap; an auto-sized text becomes a box of that width. A top or bottom handle sets the box's height, never shorter than its lines; the lines sit at the top, middle, or bottom of a taller box by the vertical alignment, the same while typing as once committed. It works on the new text being typed, which is not an undo step until the text is, on the text being edited, and on the one selected text, which is one undo step. Auto-size on makes the box fit its lines again, both ways. Typing continues at the caret afterward, and the words wrap where the box now ends, in the caret's field as on the canvas. A box's centered or right-aligned lines sit across its whole width inside its padding, the same while typing as once committed. A line that wrapped is placed by its words: the space it wrapped at does not count, so wrapped right-aligned lines end flush. A paragraph's last line counts every character, so a space typed at its end shows, moving the line and the caret, as it is typed. Padding is the space between the box's edge and its words, and the radius rounds the box's corners, seen when its Background is on. Both are at the letters' scale: a text scaled with the select tool, even more one way than the other, keeps an even padding on every side and round corners. Its box's width follows the scale across, and its height is always its lines plus padding. A list draws a marker before every line: a bullet, its number, or an empty checkbox, with every item's words starting at the same place; a click on the list button turns it on or off, and a right-click chooses Bulleted list, Numbered list, or Checklist, and the button's icon becomes that kind's (list, numbered-list-left, or list-select). Markers are drawn, not typed, so they do not show while the text is being edited. The font families are the ones the app ships. The handwritten lettering in the renders is brush ink.

## Properties panel

The panel opens and closes with the tab on its left edge and with F4. There is no close button. There is no Properties header and no tool-name and description card, so the page opens on its first section. This is the same for shape, text, and Layers. The panel starts closed. Right-clicking a tool button opens a menu with Properties, which opens the panel on that tool's page without changing the current tool. A swatch's Properties opens it on that color. While the panel is open and the current tool changes, the panel follows the new tool.

The panel shows one page, which decides its sections. The names in this list identify the page. They are not drawn:

- **Tool: Brush.** Preset, Dynamics (Pressure affects size), Pressure curve, and Behavior (Smoothing, Stabilization, Streamline).
- **Tool: Eraser.** Preset, Size, Dynamics (Pressure affects size), Pressure curve, and Behavior, with the same controls as the brush.
- **Tool: Rectangle**, **Tool: Ellipse**, and **Tool: Line.** Shape style (corner radius) for rectangles, then Behavior: Constrain proportions and Snap to grid, each a labelled switch on one line, in one section.
- **Tool: Text.** Text (font family, font size, bold, italic, underline), Layout (alignment, vertical alignment, list, line height, letter spacing, padding, radius), and Behavior (all caps, auto-size, wrap text).
- **Selected Color**, as in [color-property-ui.png](.project/renders/ui/color-property-ui.png), for a color in any palette. Its sections are Color (a color well, Name, Hex, and Visible), Stroke (Size, Opacity, and, for a tool's color, Lock size), and Stroke Style (Style, Solid or Dashed, as a dropdown, and Cap, Round or Flat, as segmented buttons). A color with no stroke, such as Fill, Background, or Text, shows only its well, hex, Visible, and opacity. Every color has Visible: it turns a fill or background on, and for a layer it is the same as the eye on its row. Cap is shown for brush colors, layers, and a line's Stroke. A fixed role has no Name row: its name is its role. Clicking the color well opens a color picker. The page has no menu: Duplicate and Delete are on a swatch's or a layer row's right-click menu. Locking a layer is on its row in the outliner.
- **Layers**, with the select tool. The outliner alone; an object's settings are not listed under it.
- **Object.** Properties in an object's right-click menu, on the canvas with the select tool or on its outliner row, opens its own page. A shape or text object shows its Palette section, then the same sections as its tool. A canvas shows its layers as a Palette section, each row opening that layer's color page; it is renamed in the outliner and its padding is fixed. A group has no page, so its Properties is drawn lighter. Right-clicking an object outside the selection selects it first; one inside the selection keeps the selection, and with several objects selected only the sections they share are shown, a value that differs between them reading as mixed until it is set. Right-clicking the select tool and choosing Properties returns to Layers.

An object's Palette section is the same list the outliner draws under a canvas, built from the same rows: a dot of the color, its name, and an eye, and for a canvas layer also a lock, renaming, dragging to sort, and its menu. A shape's or text's fixed roles have only the eye and a menu with Properties. Clicking a row opens that color's page. A tool's page has no Palette section: a tool's colors are its swatches.

A page opened from another page starts with a back row on the panel's first line: a left chevron and the name of the page it came from, such as the object, Layers, or the tool. Clicking it returns there. A color opened from an object's palette goes back to that object; one opened from the outliner goes back to Layers; one opened from a swatch goes back to its tool's page.

Tool settings, including tool palettes, are defaults for the next mark. They belong to the window, are kept between sessions, and are not undo steps. Settings of an object or of an object's palette color change the sketch, and each finished edit is one undo step. A slider drag counts as one edit when it is released. Color wells offer the brush's palette colors as quick choices. Choosing one copies the value.

Brush and eraser presets are named sets of dynamics and behavior. The first version ships Round Brush, with pressure on, and Monoline, with pressure off, for the brush, and Round Eraser for the eraser. When the settings match no preset, the preset reads Custom.

## Size

The Size control sets the size of the next mark the current tool makes. It never resizes anything already drawn; a selected object's sizes are edited in the panel. Its range follows the tool:

- With the brush, it sets the next stroke's size, from 1 to 32.
- With the eraser, it sets the eraser size, from 1 to 32. The default is 8.
- With a shape tool, it sets the next shape's outline or stroke width, from 1 to 32. The default is 3.
- With the text tool, it sets the font size, from 8 to 144, of the text being edited or the one selected text, and otherwise of the next text. The default is 24.

A brush color and a shape tool's Outline or Stroke color each have a size and a Lock size switch, under Stroke on their color page, on by default. While Lock size is on, the slider shows that color's size and its fill and knob take that color: a brush stroke then follows its layer's size, and a shape gets the color's size. Moving the slider turns Lock size off for that color, its fill and knob turn near-black, and the slider's value is used for the next marks in that color until Lock size is turned back on. Each color keeps its own slider value, so changing colors and back returns to it. These are tool settings and not undo steps.

The number beside the slider can be clicked to type a value, which is clamped into the range. Sizes are in canvas units. Zoom changes how thick they look on screen, not the stored value.

## History and clipboard

Each finished change to the sketch is one undo step: a stroke, an erase, a new object or canvas, a deletion, a move, a scale, a rotation, a flip, an alignment, a group or ungroup, a reorder, a renamed object, a visibility or lock change, a property edit, a palette edit, a paste, a cut, or a committed text edit. A new step clears redo. Undo and redo belong to the active sketch. Panning, zooming, selection, the active brush color, and tool settings, including tool palettes and the Size slider, leave history unchanged.

Copy, cut, and paste use an in-app clipboard of whole objects. A copied canvas brings its palette and strokes. Copy is available when something is selected. Cut copies and then deletes. Paste puts the duplicates at the top of the sketch, slightly offset from the source, and selects them. Repeated paste keeps offsetting. Copy, cut, and paste are shortcuts, not toolbar icons.

## View

Zoom in and zoom out step by about one quarter of the current scale, centered on the view, and stop at 12.5% and 800%. Fit frames the bounds of all visible objects with a small margin. When those bounds spread farther than the zoom limit allows, fit uses the nearest limit and centers on them. A sketch with nothing visible fits back to the default zoom on the origin. Zoom to selection does the same for the selection. Holding Ctrl or Cmd and scrolling zooms toward the pointer. Scrolling pans vertically; Shift-scroll pans horizontally. Holding Space and dragging, or dragging with the middle button, pans without changing tools. A press on the canvas while the sketch menu is open only closes the menu.

## Shortcuts

These work when text is not being edited. Ctrl and Cmd are the same. Each control's hover text shows its shortcut from the same list.

- B brush, E eraser, V select, R rectangle, O ellipse, L line, T text
- F4 show or hide the properties panel
- Ctrl+Shift+N new canvas
- Ctrl+G group, Ctrl+Shift+G ungroup, Ctrl+] bring forward, Ctrl+[ send backward, Ctrl+Shift+] bring to front, Ctrl+Shift+[ send to back
- Shift+2 zoom to selection
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
