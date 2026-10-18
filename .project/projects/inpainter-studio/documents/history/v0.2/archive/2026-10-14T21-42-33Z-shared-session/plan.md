# Inpainter 0.2 plan

Version 0.2 makes Simple Sketch 1.0.1 a sub-application of Inpainter. It keeps the look and behavior it has today. The scaffold is in the app tree, uncommitted, on top of `a40e07c`. The owner approved [reuse.md](reuse.md) on 2026-10-14.

## The scaffold

Done. `cargo run -p inpainter -- --app simple-sketch` opens the shell with one tab labeled Simple Sketch, the empty toolbar, the grid, and the panel tab, and nothing else happens. The owner agreed that sentence on 2026-10-14. The scaffolding skill was tested on a throwaway app first, corrected, and the throwaway app was removed.

## Sketching from the host

The owner chose this as the run after the scaffold, on 2026-10-14. On that day an X11 window of the rebuilt host showed Inpainter, one Untitled 1 tab, Simple Sketch's toolbar, the grid, and the panel tab. A mouse stroke on Canvas 1, a partial erase, undo, pan, zoom, and fit all showed up. Pressure and the Wayland window were not in that look. This is the window part of [H-4](tasks.md). Simple Sketch without a host, and the [H-3](tasks.md) boundary check, are not done.

## The host stays up

[H-2](tasks.md). When Simple Sketch exits or stops responding, the window stays open, the host shows that status, and the common UI still works. No picture of that status exists. The owner asked on 2026-10-14 for generated renders to approve. The status is not built until the owner approves one.
