# Simple Sketch plan

Simple Sketch is a native desktop canvas for pressure-sensitive vector sketching. The product, the window, and the stroke contract are written. That preparation is recorded in [the preparation audit](audits/2026-10-10T01-08-16Z-preparation.md). No application code exists yet. The bare clone of https://github.com/Spumcake/vector-sketch is in place, and that remote has no commits.

## Stroke model and infinite canvas

This comes first. The bare repository is already at `src/vector-sketch/worktrees/.bare`. The branch-manager adds `src/vector-sketch/worktrees/dev` on `dev` once `main` has a commit. Task-operations then builds the session, pen, eraser, history, and camera in that checkout. What becomes usable is one sketch where a person can draw, erase part of a stroke, undo, pan, zoom, and fit. It is complete when [SS-1](tasks.md), [SS-2](tasks.md), [SS-3](tasks.md), and the pen and erase parts of [SS-4](tasks.md) hold, including their unit tests. Shapes, text, selection, the pictured toolbar, and saving wait.

## Pictured shell

After those types have landed, one worker adds the toolbar, tabs, and menu so the window matches the desktop reference. Open and Save wait until sketches can be stored. This milestone is complete when [SS-5](tasks.md) holds on a running window. It cannot start beside another writer of the app entry point.

## Shapes, text, and selection

Shapes come next, then text, then selection and the in-app clipboard. Each one extends the same mark list and the same history. They stay in that order because they edit the same session files. This milestone is complete when [SS-6](tasks.md) holds and [SS-4](tasks.md) includes those gestures.

## Saving a sketch

Private snapshots then give Save and Open a stable identity, including every kind of mark. This milestone is complete when [SS-7](tasks.md) holds. A published file format is not part of it.

No product decision is waiting before the first milestone. `dev` waits on an initial commit on `main`, and no release version is agreed.
