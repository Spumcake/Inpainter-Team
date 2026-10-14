# Inpainter 0.2 plan

Version 0.2 makes Simple Sketch 1.0.1 a sub-application of Inpainter, keeping the look and behavior it has today. The first attempt passed its checks but ran Simple Sketch inside the host; the intent review caught it, and it was set aside on the Inpainter branch `scrap/v0.2-shared-session`. The work restarts from [reuse.md](reuse.md) and [systems.md](systems.md), rewritten on 2026-10-14 on the line the owner chose: the host shows and previews, and Simple Sketch commits and owns. The previous plan is [archived](../../history/v0.2/archive/2026-10-14T21-42-33Z-shared-session/plan.md).

## The scaffold

Done again on the reset tree, by the scaffolding skill: `cargo run -p inpainter -- --app simple-sketch` opens the shell with one tab labeled Simple Sketch, the empty toolbar, the grid, and the panel tab. The skill's fix from its test on a throwaway app was kept.

## Simple Sketch with no host

The shared `draw` crate, Simple Sketch's session and per-sketch record, and the protocol messages in systems. Complete when a test with no host sends a stroke, undoes, redoes, and saves a sketch that reads back, and the boundary check holds.

## Sketching from the host

The host draws the canvas, toolbar, menu, panel, outliner, tabs, and dialogs from the selected tab's declarations, previews gestures, and runs several sub-applications at once. Complete when the window slice sentence in [tasks.md](tasks.md) holds and the two-sub-application, one-owner, and carried-over-chrome checks in [systems.md](systems.md) pass. This is H-4 and H-7.

## The host stays up

H-2. The owner approved the two images in INTERFACE's Sub-application status on 2026-10-14.

The owner approved [reuse.md](reuse.md) and [systems.md](systems.md) on 2026-10-14, as they stood, and accepted one residual risk: `draw` exports `apply` and `revert`, which the one-owner check catches. The intent review checks each slice at its first integration, and each finished slice.
