# Simple Sketch

Simple Sketch is a desktop app for sketching and annotating with a pressure-sensitive stylus. The page is a vector surface that does not end. Strokes follow the pen, smooth into lines, and stay sharp as you zoom. The eraser lifts ink while the original stroke remains whole.

## What it solves

A sketch falls apart when the page runs out, when a zoomed line turns jagged, or when a correction cuts a mark you still needed. Simple Sketch keeps one continuous surface: pan to a clear area, zoom in, and keep drawing. Lines stay crisp vectors, and smoothing turns a quick stylus mark into a continuous stroke.

Pressure changes that stroke as you draw. A light note and a heavier outline come from the same brush. Ink lives on canvas objects, and each canvas carries its own palette. A palette color is more than a hue: it sets the stroke's size and style, and it is a layer, so black ink sits above red and red above orange until you reorder them. Restyling a color restyles everything drawn in it.

Erasing records an operation against the strokes it crosses and does not destroy their geometry. The lifted ink leaves the page, and the stroke underneath remains. Undo removes the erasure, and the line shows through again.

Rectangles, ellipses, lines, and text are objects beside the canvases. Objects can be grouped, reordered, hidden, locked, moved, scaled, rotated, and flipped, and every change shares one undo history.

## How people use it

Open a sketch and draw: the first canvas is already highlighted and black is ready. The stroke tracks the stylus, widens with pressure, and settles into a smoothed line on the grid.

Pick red and annotate. The red notes sit under the black ink because black is above red in the palette. Drag red above black in the outliner, and the notes come forward. Make red thicker in its properties, and every red stroke thickens with it.

Start a second canvas for a separate idea with its own colors. Draw a filled rectangle around a passage and add a text label. Group the label with the rectangle, rotate the group, and hide it while you work on the sketch underneath.

Drag the eraser across a line. The crossed portion disappears and the rest of the stroke stays. Undo brings the hidden portion back.

Press F4, or right-click a tool, to open its properties: the brush's pressure curve and smoothing, the rectangle's fill and dash, the text's font and alignment. Zoom in on a corner, then fit the sketch to the window. A new tab opens another sketch beside ones already open, such as Untitled 1, Notes, and Sketches. The drawings in the product pictures are examples. A new sketch starts empty.

## What to learn from existing apps

Concepts treats every stroke on its infinite canvas as an editable vector, so a sketch can change without a full redraw ([Concepts](https://concepts.app/)). You move by panning and zooming, and the app is built for pressure-sensitive styli ([the infinite canvas](https://concepts.app/en/manual/infinite-canvas)). Its hard and soft masks hide a region while the underlying strokes stay editable. A separate slice tool cuts the vector path and deletes what it touches ([brushes and tools](https://concepts.app/en/manual/brushes-and-tools)).

Simple Sketch adapts the infinite vector page, the pressure-sensitive brush, and the idea that an erasure should leave the stroke recoverable. One eraser records an operation against existing strokes. Moving around the page is pan, zoom in, zoom out, and fit. Design tools contribute the object list with groups, eyes, and locks, and one inspector panel whose contents follow the tool or selection in hand.

## How it should look and work

Simple Sketch is one desktop window. The title bar shows a small orange pencil and the name Simple Sketch. Under it, a menu, a tab for each open sketch, and a new-sketch control form a light bar. The active tab is the sketch in front of you.

The next bar holds the controls as simple line icons. The highlighted canvas's palette swatches come first, with the active color marked. Then come new, open, and save, then undo and redo. The brush is highlighted when a sketch opens, followed by the eraser, selection arrow, rectangle, ellipse, line, and text. A size slider shows its value as a number. Zoom out, zoom in, fit, and a maximize toggle stay at the far end. The surface is near-white with a faint square grid. A properties panel can open on the right; for the selection arrow it holds the layers outliner.

## The first version

The first version is this desktop window. Phone layouts, shared canvases, accounts, cloud sync, export, a published file format, and printing sit outside it. An open sketch can be saved so closing it can keep the work; the storage method stays unspecified. Brush flow and eraser opacity wait for a later version. A new sketch starts with one empty canvas.
