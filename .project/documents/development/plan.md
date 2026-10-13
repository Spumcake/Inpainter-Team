# Simple Sketch plan

A `.smx` file the person chooses is in the dirty `dev` worktree, on top of `c435080`. The window opened and did not save a file. The previous plan is [archived](archive/2026-10-13T12-53-19Z-smx-save/plan.md). The record is [the smx audit](audits/2026-10-13T14-36-28Z-smx-save.md).

## A sketch file

Save asks for a folder and a name ending in `.smx`. The file is JSON: the name, the view, and the objects, colors, strokes, and erasures. History, the selection, the clipboard, and tool settings stay out. The first save is Save As. Ctrl+S then writes that path. Save As moves later saves. Save a Copy writes another file and leaves Ctrl+S where it was. Open is a file dialog, with recent files in the sketch menu. A path already open is focused. A changed sketch still asks once before it closes. Save uses the path, or Save As if it was never saved. Replacing a different existing file asks first. A `.smx` path given to the program opens.

Every 10 minutes, a dirty sketch writes a recovery copy into a folder the app owns, next to preferences. The `.smx` stays untouched. A crash tries one more emergency copy. The next launch offers any recovery a clean save did not delete. Restore opens it still unsaved and remembers the original path. The file changes only when the person saves. A kill that leaves no chance to write keeps the last timed copy.

The interface, the pitch, the systems document, and [SS-7](tasks.md) now describe this file. One worker built the path, the dialogs, and the recovery timer, because they share the session. Session tests round-trip a `.smx` and a recovery that does not write that file until the person saves. Review found five data-loss defects, and the same worker fixed them. A check that stayed inside the Simple Sketch window did not open the file dialog, so a saved drawing has not been seen coming back. The slice is ready to try.

## Later

A file-manager association waits. The owner's remaining checks are the clipboard, text, the toolbar width, the group drop, the shape-drag readings, and the XP-Pen. An opened sketch still highlights no canvas. Related shades stays undefined. The private-list window try is dropped, because this slice replaces that list.

## Readings used

The documents left these open. The build used these readings. They are not owner decisions:

- The folder picker is `rfd` 0.15.4, with the desktop portal. Save As is Ctrl+Shift+S, as the owner set on 2026-10-13; Save a Copy and the recovery offer have no shortcuts.
- One recovery file per sketch, replaced each interval. The recent menu is the last 10 paths. Save As starts in the last folder chosen in this window, otherwise the home directory.
- The tab shows the file name without `.smx`. Unsaved work shows a dot in place of the tab's close mark and an asterisk in the window title, as the owner set on 2026-10-13. A failed save or open shows a message with the file and the reason. There is no separate title to rename.
- The recovery offer is Restore and Discard, one at a time. Discard deletes that recovery file. Discard on close does the same.
- `sketch-1.json` and `sketch-2.json` stay in the user-data directory and are not imported.

This slice starts from the dirty worktree on `c435080`. [systems.md](systems.md) still opens on the flat mark list at `7920c85`. Its storage section still says it is undecided whether declining a recovery deletes the file. The interface, and the build, delete it on Discard.
