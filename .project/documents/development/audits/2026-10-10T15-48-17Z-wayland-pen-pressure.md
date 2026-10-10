# Wayland pen pressure

**UTC:** 2026-10-10T15:48:17Z
**Scope/baseline:** App repository `vector-sketch`, branch `dev` at `bc987c4` (`Add the infinite canvas for drawing and erasing strokes.`), with the tablet work uncommitted. Nothing was pushed. Follows [the review and fixes audit](2026-10-10T15-12-57Z-stroke-canvas-review-fixes.md).
**Outcome:** Completed for the code and every check possible without the tablet. Pressure on the owner's XP-Pen is unverified; the owner chose to test on the device later.

## Goal and participants

The goal was for an XP-Pen on Wayland to draw with pressure. One agent, Claude Code, did the work and checked it. No Cursor roles ran. The review was self-review.

## Attempts and result

[tablet.rs](../../../../src/vector-sketch/worktrees/dev/src/tablet.rs) connects `octotablet` to the window's display. Its feed runs in eframe's `raw_input_hook`. Once a client binds the tablet protocol, the compositor stops sending the pen as a pointer, so contact, motion, and lift are re-sent to egui as pointer events. Each touching pose becomes one sample with its own pressure. The eraser end switches to the eraser until lift. [canvas.rs](../../../../src/vector-sketch/worktrees/dev/src/canvas.rs) uses those samples for pen strokes. X11 and other platforms fall back to the mouse path.

The first Wayland launch exited with status 1. `octotablet` 0.1.0 bound tablet protocol version 2, which the compositor advertises, but its bundled definitions only know version 1. The compositor's `zwp_tablet_v2` event 5 then broke the shared connection. Upstream `master` at `1b0a14e` still has this bug. A patched copy in `vendor/octotablet` caps the bound version, and the app uses it through `[patch.crates-io]`. The patch is described in [PATCHED.md](../../../../src/vector-sketch/worktrees/dev/vendor/octotablet/PATCHED.md). Only the Wayland feature is enabled.

[systems.md](../systems.md) and the app README record the dependency and its behavior.

## Evidence and limits

`cargo test` passed 12 of 12, including three new tests of the pen-event translation. Clippy reported no warnings in new code. Under Xvfb (X11), the app ran until its timeout. On the owner's Wayland session, with `WAYLAND_DEBUG=1` and the patch applied, the app ran until its timeout. It bound `zwp_tablet_manager_v2` at version 1 and received the "XP-Pen Pen" and "XP-Pen Eraser" tablets.

The following were not verified on the device:
- that pressure arrives and thins strokes;
- that egui coordinates match the pen at fractional scaling;
- that the eraser end reports as an eraser tool;
- that the cursor stays visible while hovering, since `octotablet` sets no tablet cursor.

An idle window now repaints every 100 ms. Stylus side buttons are ignored.

## Next action

The owner tests with the XP-Pen once more of the app exists. Committing this work is a separate request. AGENTS.md still names `697f231` and says the slice is uncommitted.
