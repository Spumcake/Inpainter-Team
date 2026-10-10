# Simple Sketch

Simple Sketch is a desktop app for sketching and annotating with a pressure-sensitive stylus. The page is a vector canvas that does not end. Strokes follow the pen, smooth into lines, and stay sharp as you zoom. The eraser lifts ink while the original stroke remains whole.

## What it solves

A sketch falls apart when the page runs out, when a zoomed line turns jagged, or when a correction cuts a mark you still needed. Simple Sketch keeps one continuous surface: pan to a clear area, zoom in, and keep drawing. Lines stay crisp vectors, and smoothing turns a quick stylus mark into a continuous stroke.

Pressure changes that stroke as you draw. A light note and a heavier outline come from the same pen. Color swatches sit on the toolbar, with the color in use marked, so black ink and a red annotation are one step apart.

Erasing records an operation against the strokes it crosses and does not destroy their geometry. The lifted ink leaves the page, and the stroke underneath remains. Undo removes the erasure, and the line shows through again.

Rectangles, ellipses, straight lines, and text labels share that canvas and the same undo history as the ink around them.

## How people use it

Choose the pen and draw. The stroke tracks the stylus, widens with pressure, and settles into a smoothed line on the grid.

Switch to red, drag a rectangle around a passage, and add a line and a short text label. Those marks land on the same canvas, in the color and size you chose.

Drag the eraser across a line. The crossed portion disappears and the rest of the stroke stays. Undo brings the hidden portion back.

Zoom in on a corner, then fit the sketch to the window. Pan onward and keep drawing. A new tab opens another sketch beside ones already open, such as Untitled 1, Notes, and Sketches. The drawings in the product picture are examples. A new sketch starts blank.

## What to learn from existing apps

Concepts treats every stroke on its infinite canvas as an editable vector, so a sketch can change without a full redraw ([Concepts](https://concepts.app/)). You move by panning and zooming, and the app is built for pressure-sensitive styli ([the infinite canvas](https://concepts.app/en/manual/infinite-canvas)). Its hard and soft masks hide a region while the underlying strokes stay editable. A separate slice tool cuts the vector path and deletes what it touches ([brushes and tools](https://concepts.app/en/manual/brushes-and-tools)).

Simple Sketch adapts the infinite vector page, the pressure-sensitive pen, and the idea that an erasure should leave the stroke recoverable. One eraser records an operation against existing strokes. Moving around the page is pan, zoom in, zoom out, and fit.

## How it should look and work

Simple Sketch is one desktop window. The title bar shows a small orange pencil and the name Simple Sketch. Under it, a menu, a tab for each open sketch, and a new-sketch control form a light bar. The active tab is the canvas in front of you.

The next bar holds the controls as simple line icons. Color swatches come first, with the current color marked, then copy, paste, cut, undo, and redo. The pen is highlighted when a sketch opens, followed by the eraser, selection arrow, rectangle, ellipse, line, and text. A size slider shows its value as a number. Zoom in, zoom out, and fit stay at the far end. The surface is white with a light square grid. The active tab, the marked color, and the highlighted tool show what the next mark will do.

## The first version

The first version is this desktop window. Phone layouts, shared canvases, accounts, cloud sync, export, a published file format, and printing sit outside it. An open sketch can be saved so closing it can keep the work; the storage method stays unspecified. A new document starts empty.
