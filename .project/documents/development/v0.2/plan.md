# Inpainter 0.2 plan

Version 0.2 makes Simple Sketch 1.0.1 a sub-application of Inpainter, keeping the look and behavior it has today. The first attempt passed its checks but ran Simple Sketch inside the host; the intent review caught it, and it was set aside on the Inpainter branch `scrap/v0.2-shared-session`. The work restarts from [reuse.md](reuse.md) and [systems.md](systems.md), rewritten on 2026-10-14 on the line the owner chose: the host shows and previews, and Simple Sketch commits and owns. The previous plan is [archived](../../history/v0.2/archive/2026-10-14T21-42-33Z-shared-session/plan.md).

## The scaffold

Done again on the reset tree, by the scaffolding skill: `cargo run -p inpainter -- --app simple-sketch` opens the shell with one tab labeled Simple Sketch, the empty toolbar, the grid, and the panel tab. The skill's fix from its test on a throwaway app was kept.

## Simple Sketch with no host

Done on 2026-10-14, by Claude Code. Simple Sketch's real session was moved into the sub-application whole, with every edit through the record, and all 88 of its own tests pass. The no-host check passes: a separate process with no host creates a sketch, strokes, erases, changes the canvas padding, undoes, redoes, and saves through Simple Sketch's own file request, and every reply and the saved file equal an in-process session's. Shape and text gestures, the replace-file question, and recovery offers wait for the host in slice 2.

## Sketching from the host

The host draws the canvas, toolbar, menu, panel, outliner, tabs, and dialogs from the selected tab's declarations, previews gestures, and runs several sub-applications at once. Complete when the window slice sentence in [tasks.md](tasks.md) holds and the two-sub-application, one-owner, and carried-over-chrome checks in [systems.md](systems.md) pass. This is H-4 and H-7.

Built on 2026-10-15 by Claude Code and waiting for the owner's look. Run by hand on a virtual display: a stroke, an erase across it, undo, the panel, select, move, scale, a rectangle, text, zoom, fit, a new tab, closing an unsaved tab, quitting with an unsaved sketch, and Simple Sketch beside the template in one window. The scripted checks in systems are not written yet; they belong to the test pass.

## The host stays up

H-2. The owner approved the two images in INTERFACE's Sub-application status on 2026-10-14. Built on 2026-10-15 and waiting for the owner's look: pausing Simple Sketch's process shows the not-responding notice within three seconds, panning still works under it, and it clears when the process resumes; killing the process shows the exited notice.

The owner approved [reuse.md](reuse.md) and [systems.md](systems.md) on 2026-10-14, as they stood, and accepted one residual risk: `draw` exports `apply` and `revert`, which the one-owner check catches. The intent review checks each slice at its first integration, and each finished slice.
