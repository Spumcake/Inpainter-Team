# Properties panel

**UTC:** 2026-10-11T01:53:00Z. The session clock at the start of this recovery. The system date command could not run.
**Scope/baseline:** App repository `vector-sketch`, branch `dev` at `6210d8e` (`Replace the mark list with the canvas document tree.`). The panel is uncommitted. Nothing was pushed.
**Outcome:** Completed for the brush, eraser, and color panel in the dirty worktree. Not a clean candidate.

## Goal and participants

The goal was a panel the owner can open and edit. One task-operations worker wrote it. Cursor crashed before that pass returned. The files were still in the dev worktree, and the same role finished them. The panel, the schema, and the document share one session and one frame, so a second implementer was not started. Verification and code review then ran together. Verification rechecked F4 after the fix.

## Attempts and result

F4 opens and closes the panel, and the canvas narrows while it is open. The brush, eraser, and color pages match the plan. Related shades is absent. The first F4 press panicked on a negative panel width. Review also found that undo during a color-slider drag could commit over that history change, and that a hidden or locked color told the user to highlight a canvas. Those three were fixed.

## Evidence and limits

`cargo test` passed 30 tests, then 33 after the fixes. Verification did not repeat that suite. The window journey is in [testing.md](../testing.md). An X11 relaunch was used because the Wayland window could not be captured. Presets, a stroke that kept its shape, dashed ink and undo, moving a color, and the last color refusing delete passed. F4 then passed on the recheck. Unchecked in the window: settings after a restart, flat cap, duplicate, and hiding or locking a color. Preferences are written to the platform user-data file `simplesketch/preferences.json`.

## Next action

Related shades still has no defined behavior. Selection and the Layers outliner are the next plan milestone and are not started. Committing this work is a separate request.
