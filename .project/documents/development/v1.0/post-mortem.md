# Post-mortem

Version 1.0 of Simple Sketch closed on 2026-10-13 at 16:06 UTC. The owner closed it after confirming that the app works with an XP-Pen tablet. Version 1.0 is the full realization of [the pitch](PITCH.md).

## How long it took

The earliest record is the preparation audit at 2026-10-10T01:08:16Z. This version closed at 2026-10-13T16:06:53Z. The span is three days, fourteen hours, and fifty-eight minutes.

The app repository's first commit is an empty history at 2026-10-10T01:29:48Z. The first drawing code is 2026-10-10T15:28:10Z. The first team commit is 2026-10-10T15:32:29Z, "Add the Simple Sketch team documents."

## What was built

Simple Sketch is one desktop window for sketching with a pressure-sensitive stylus on a vector surface that does not end. The work is the list in [tasks.md](tasks.md). In order, the slices were a stroke canvas, the pictured shell and Wayland pen pressure, the document tree, the properties panel, selection and layers, shapes, text, tool palettes, the toolbar and text review, copy cut and paste of whole objects, and saving a sketch as a `.smx` file the person chooses, with a local recovery copy. Saving first landed as a private list of JSON files, then the owner replaced that with the `.smx` file. Copy, cut, and paste of whole objects needed a second pass because the window turned those keys into clipboard events and the first path never saw them. The save system is app commit `b11a89a`.

The owner set the panel chrome, Save As as Ctrl+Shift+S, the unsaved mark on the tab and in the window title, and the message shown when a save or open fails. Phone layouts, shared canvases, accounts, cloud sync, export to other formats, printing, brush flow, and eraser opacity stayed outside this version, as the pitch says.

## What the records do not show

A recorded window check did not open a saved `.smx` again, did not try the clipboard keys in the window, and did not try the latest text. Those items stay on the todo list. The owner closed the version with those checks unrecorded.
