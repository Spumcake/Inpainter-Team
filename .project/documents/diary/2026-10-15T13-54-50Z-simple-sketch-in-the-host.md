# Simple Sketch runs inside the host

**UTC:** 2026-10-15T13:54:50Z
**Covers:** since [the template was pushed](2026-10-14T17-20-09Z-pushed-the-template.md).
**Repositories:** The team repository is on `master` at `6c6d629`, "Record the approved version 0.2 plan," with this stretch's document changes uncommitted on top. Inpainter is on `dev` at `865ba05`, "Record the Simple Sketch scaffold in the lockfile," with slices 1 and 2 and H-2 uncommitted on top. Nothing from this stretch is committed or pushed. `src/simple-sketch` was read as the reference and not changed.

## What we did

**The version 0.2 plan, and a first attempt set aside.** The owner approved the version 0.2 plan, committed as `6c6d629`. The first attempt at it passed its checks, but it ran Simple Sketch's whole session inside the host. That meant two copies of every sketch and two undo histories, which is the fake the mission names. The intent review caught it, and the code was kept, never to merge, on the Inpainter branch `scrap/v0.2-shared-session`. `dev` was reset to `a40e07c` and Simple Sketch was scaffolded again.

**A new line, and new documents.** The owner set the line on 2026-10-14: the host shows and previews, and Simple Sketch commits and owns. `reuse.md` and `systems.md` were rewritten on that line. The owner also chose to run several sub-applications in one window in 0.2, and to keep the template empty. After the intent review passed with residual risks, the owner approved both documents, accepting that `draw` exports `apply` and `revert`. The owner also approved the two status images for a sub-application that has exited or stopped responding.

**Slice 1, done twice.** Cursor's build of slice 1 wrote a new, smaller session instead of moving Simple Sketch's, and still passed the no-host check. The owner's review found it and asked Claude Code to do the move instead. Claude Code moved the reference's `session.rs`, `registry.rs`, `store.rs`, and `prefs.rs` whole, and changed only what the split between record and document required:
- Every edit to a sketch goes through Simple Sketch's record: `commit`, `record`, `preview`, or `move_view`.
- Marking each step-returning `draw` method `#[must_use]` found 20 places where an undo step was being silently dropped, and one paste that set the selection before inserting. All were fixed.
- All 88 of the reference's session tests passed on the moved code.
- `sketch-client` became a no-host check. It drives the real `simple-sketch` program and an in-process copy of its session side by side, and compares every reply and the saved file byte for byte.

**Slice 2 and H-2.** The owner said to finish the project, and Claude Code built both. Simple Sketch now declares everything its old window drew: tabs, keys, toolbar, menu, properties, layers, the drawing surface's current tool, the text being typed, questions, and file dialogs.
- Every declared control carries the action the host sends back unread. The host reads only the drawing surface's own `canvas.*` commands and `window.maximize`.
- The reference's `canvas.rs`, `toolbar.rs`, `panel.rs`, `outliner.rs`, `tabs.rs`, `widgets.rs`, `tablet.rs`, and `dialogs.rs` moved into the host. In each, a read or call of the session became a read of the declarations or the display copy, or a message. The new `host/src/doc.rs` stands where `canvas.rs` called the session.
- The reference's close confirmation, replace question, recovery offers, file errors, and quit walk moved into Simple Sketch as `ask` messages.
- The host runs every sub-application `--app` names in one window, with a reader thread and a writer thread for each. It shows the approved notices when one exits or stops answering its one-second ping.
- The template now answers `ping` and `quit`, and the scaffolding skill describes the new messages.

**The blink.** The owner saw a new stroke or object disappear for a split second and come back. The host was dropping its preview the moment the pen lifted, before Simple Sketch's document arrived. Now:
- A finished stroke, erase trail, or shape stays on screen until the document answering it arrives.
- Committed text stays drawn as typed.
- Simple Sketch always answers a gesture with a document, so a held preview cannot linger.

## What we learned

A check that compares a program with itself cannot tell a moved session from a rewritten one. Only the owner's review of the code caught the small replacement. Marking step-returning methods `#[must_use]` turned a class of silent undo bugs into compile errors.

The desktop blocks synthetic input from tools, so hand runs used a private virtual X display. It drew the real window and accepted scripted input, and it kept test files out of the owner's data folder. One gap remains there: file dialogs go through the desktop's portal. A Save test reached the owner's real screen, and that test was stopped.

The approved sentence for the one-owner check, that the screen does not change between sending a gesture and the reply, already described the right fix for the blink. The first build had read it as "end the preview at once."

## Where things stand

The host runs Simple Sketch with its reference look. These were checked by hand on the virtual display:
- Strokes, erasing, undo, the panel, select, move, scale, rectangles, and text.
- Zoom, fit, the object menu and its Properties page, and the sketch menu.
- New tabs, closing and quitting with unsaved sketches, and Simple Sketch beside the template.
- Both failure notices.
- Marks held on screen while Simple Sketch was paused.

All 105 tests pass, and the no-host check passes. What was built, and where it departs from the plan, is under "As built" in [systems.md](../../development/v0.2/systems.md) and [reuse.md](../../development/v0.2/reuse.md). The main departure is that the panel's open state stays with Simple Sketch, as its session kept it. The window-side helpers that the moved session tests still call stay until those tests move to the host.

Not yet checked: pressure and the stylus eraser on the owner's tablet, Save and Open with a real file dialog, and the window on the owner's own Wayland desktop. The scripted checks in systems' verification seams are not written. Under the project's rules, they are written in one pass when the owner asks to commit.

## Next steps

These are the open items in [todo.md](../../development/v0.2/todo.md):
- The owner looks at H-4, H-7, and H-2 in the running window.
- Check the tablet and the file dialogs.
- When the owner asks to commit, write the tests and the verification checks in one pass, then commit both repositories.

Recording these here does not start them.
