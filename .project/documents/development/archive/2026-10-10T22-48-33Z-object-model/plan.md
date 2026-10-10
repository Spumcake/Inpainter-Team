# Simple Sketch plan

The pictured shell is committed on `dev` as `7920c85`, on top of published `bc987c4`, and is not pushed. A window check met [SS-5](tasks.md), and a later review polished it ([polish audit](audits/2026-10-10T21-01-28Z-shell-review-and-polish.md)). The previous plan is [archived](archive/2026-10-10T16-22-05Z-shell-plan/plan.md).

## Pictured shell

The window follows [ui.png](../../renders/ui/ui.png). The three icons after the swatches stay as drawn, and their actions are not named. The far right is zoom in, zoom out, and one button that toggles the maximized state. [INTERFACE.md](../../../INTERFACE.md) and [systems.md](systems.md) record that.

The shell is usable around the canvas that already draws and erases. A new sketch is empty. Each tab keeps its own marks, view, and history. Tool, color, and size stay when the tab changes. Closing the only clean blank does nothing. The shape, text, and select buttons are visible and wait. The three unnamed icons do nothing. Open and Save wait until snapshots exist. Closing the window asks once for each dirty sketch.

The window check is in [testing.md](testing.md). The record is [the shell audit](audits/2026-10-10T17-59-54Z-pictured-shell.md). Per-tab selection was not exercised in the window. The session tests cover marks, camera, and history staying with the tab.

## Later marks and saving

Shapes, then text, then selection and the in-app clipboard, then private snapshots. Clipboard stays a later slice and is not a toolbar group. Those milestones stay broad until each one is the current slice. This shell does not start them.

The three icons after the swatches still have no named action. The Wayland pen path is committed, and pressure on the XP-Pen is still the owner's check. [systems.md](systems.md) still describes the `dev` checkout as missing, and [AGENTS.md](../../../AGENTS.md) still names the empty root. Both are stale: `dev` is published.
