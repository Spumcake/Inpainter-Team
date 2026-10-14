# Clipboard keys

**UTC:** 2026-10-13T01:07:24Z
**Scope/baseline:** App repository `vector-sketch`, dirty `dev` worktree on `0c1d3b7`. Follows [the clipboard audit](2026-10-13T00-27-06Z-clipboard.md). The owner tried the window and the shortcuts did nothing.
**Outcome:** The key path is fixed in a frame test. A paste has still not been seen in the window after the fix.

## Goal and participants

The goal was to make Ctrl+C, Ctrl+X, and Ctrl+V run the in-app clipboard. One task-operations worker found the cause and changed `src/canvas.rs`. No other worker wrote code. Review was not run again. The desktop was not driven.

## Attempts and result

egui-winit turns those key-downs into copy, cut, and paste events and drops the key. The shortcut check only looked for a key-down, which is how Ctrl+Z works, so it never saw them. Paste is omitted entirely when the operating-system clipboard is empty. The key-up still arrives.

The frame now runs the in-app command from the copy, cut, or paste event, and from the key-up when the press never came. A press and its key-up run the command once. Text from the operating-system clipboard is ignored. The commands still do nothing while text, a name, or the size field is being edited.

One gesture can still miss: if the operating-system clipboard is empty and Ctrl is released before V, neither a paste event nor a key-up with Ctrl held is emitted. Releasing V while Ctrl is held still pastes.

## Evidence and limits

`canvas::tests::ctrl_c_then_ctrl_v_duplicates_the_selected_object` draws a rectangle and sends the events the window delivers. All 21 `canvas::tests` passed. The other 81 library tests were not run. The window was not opened.

## Next action

Quit any open Simple Sketch window and run this worktree. The try is still the open item in [todo.md](../todo.md). Snapshots are not started.
