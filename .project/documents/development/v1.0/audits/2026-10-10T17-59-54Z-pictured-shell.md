# Pictured shell

**UTC:** 2026-10-10T17:59:54Z
**Scope/baseline:** App repository `vector-sketch`, branch `dev` at `bc987c4` (`Add the infinite canvas for drawing and erasing strokes.`). The shell and the earlier Wayland pen path are uncommitted. Nothing was pushed.
**Outcome:** Completed for SS-5 in the dirty worktree. Not a clean candidate.

## Goal and participants

The goal was the pictured window, so a person can try it. One task-operations worker wrote the shell and the later fixes in `src/vector-sketch/worktrees/dev`. The shell shares the session and the frame, so a second implementer was not started. Verification and code review then ran together. Verification rechecked the fixes after they landed. The fixes themselves were not reviewed again.

## Attempts and result

The window opens on an empty Untitled 1. Tabs keep their own marks, camera, and history. Tool, color, and size stay across tabs. The three icons after the swatches do nothing. Open and Save wait. Select, rectangle, ellipse, line, and text can be chosen, and their gestures wait.

Review found two defects: zoom and the maximize toggle could scroll off a narrow toolbar, and a tab changed sketches only when its name was clicked. Closing the window also dropped a dirty sketch without asking. Those three were fixed. Iconoir through `iconflow` 2.1.0 compiled against eframe 0.36.2.

## Evidence and limits

`cargo test` passed 16 tests, then `cargo test --offline` passed 16 after the fixes. Verification did not repeat that suite. The window journey is in [testing.md](../testing.md). An X11 relaunch was used because the Wayland window could not be driven. At 1280×800 the chrome matched [ui.png](../../../renders/ui/ui.png), including the single maximize toggle. At 640×700 the zoom controls and the toggle stayed visible. Tab padding switched sketches. Closing a dirty window asked once; Cancel stayed, and Discard quit. Unverified in the window: per-tab selection, and the size stops at 1 and 32.

## Next action

The three icons after the swatches still have no named action. Shapes, text, selection, and snapshots stay later and are not started. Committing this work is a separate request.
