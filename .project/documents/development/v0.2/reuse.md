# Reuse map — 0.2

**Reference:** Simple Sketch at `887f6f4d7813f0a2cf1114f496f6b7eb33f18c4f` (version 1.0.1), checked out at `src/simple-sketch` while this map was carried out. The paths below starting with `src/` are that tree's.
**Scope:** [tasks.md](tasks.md): H-2, H-4, Simple Sketch without a host, and the H-3 boundary check.
**Replaces:** the map the owner approved earlier on 2026-10-14, [archived](../../history/v0.2/archive/2026-10-14T21-42-33Z-shared-session/reuse.md). That map put Simple Sketch's whole session, registry, schema, store, and preferences in a library the host linked, so the host ran Simple Sketch itself and kept its own undo history. Its code is kept, never to merge, on the Inpainter branch `scrap/v0.2-shared-session`.

## The line

As the owner set on 2026-10-14: **the host shows and previews; the sub-application commits and owns.** The host draws the document and previews a gesture while it happens. When the gesture finishes, the host sends it to Simple Sketch once: a stroke's samples, an erase path, a shape's corners, typed text, a transform. Simple Sketch turns it into a `Step`, records it for undo, saves, and sends the whole document back. The host replaces its display copy with it. The host never allocates an id, never keeps an undo history, and never writes a sketch file. Sending only the changed `Step` instead of the whole document can come later.

The drawing surface is a shared component of the host, like the grid and the properties panel: a sub-application declares it for a tab, with its current tool's live settings for previews. Simple Sketch keeps its own files, preferences and recovery copies, in its own folder inside Inpainter's data folder.

Every entry below is copied unchanged first, then given only the edits named.

## Into the shared drawing crate `draw/`

`draw` is a library. Both the host and Simple Sketch link it. It holds only what both need in order to draw and to apply an edit. It has no session, registry, schema, store, or preferences.

- `src/ink.rs` → `draw/src/ink.rs`: move. Its tests move with it.
- `src/text.rs` → `draw/src/text.rs`: move. Layout and painting of text objects, and its tests.
- `src/defaults.rs` → `draw/src/defaults.rs`: move.
- `src/document/snapshot.rs` → `draw/src/document/snapshot.rs`: move and edit. The `.smx` value, `format_version` 1. It is also the form in which a document crosses the protocol. `draw`'s `Sketch` does not hold `id`, `name`, `generated_name`, `stored`, `dirty`, `path`, `recovery_file`, `undo`, or `redo`. The `.smx` snapshot still contains `id` and `name` at `format_version` 1. Simple Sketch's record supplies those fields when the snapshot is written and read.
- `src/document.rs` → `draw/src/document.rs`: move and edit. Nodes, palettes, `Step`, `Camera`, `Allocator`, hit testing, scene and paint lists, and the shape and stroke paths stay. Edits:
  - `Sketch` keeps the model only: `origin_canvas`, `born`, `nodes`, `selection`, `active_canvas`, and `camera`. Its `id`, `name`, `generated_name`, `stored`, `dirty`, `path`, `recovery_file`, `undo`, and `redo` move to Simple Sketch, below.
  - `commit_step`, `undo`, and `redo` are split. `draw` keeps only `apply(&Step)` and `revert(&Step)`, which change the model and keep no history. Recording stays with Simple Sketch: see the record below. Inside `draw`, nothing that edits a sketch applies a step on its own; each such method returns the `Step` it built instead of applying and dropping it.
  - Items the host or Simple Sketch call become `pub`.
- From `src/schema.rs`: `PropId`, `PropValue`, and `Inspect` → `draw/src/document.rs`: move. `PropId`, `PropValue`, and `Inspect` live in `draw` because `Step` stores them. The rest of `schema.rs` stays in `apps/simple-sketch` and uses those types from `draw`. `draw` still has no schema module of sections and pages.
- From `src/session.rs`: `OpenGesture`, `GestureKind`, and the sample handling in `add_sample` → `draw/src/gesture.rs`: move and edit. A gesture starts from the live tool settings the sub-application declares, not from session fields. It ends as a `Gesture` value to send, not as a `Step`. A shape gesture stores its kind, `ShapeKind`, in `draw`. It does not hold `registry::Tool`. The app maps that kind when it commits a `Step`.
- From `src/session.rs` and `src/canvas.rs`: the camera arithmetic, `pan_screen`, `zoom_toward`, `zoom_at_center`, `fit`, `screen_to_document`, and `document_to_screen` → `draw/src/camera.rs`: move and edit. They act on a `Camera` and a document, not on a session.

## Into the host `host/`

The rule for every host file: wherever the reference reads or calls `Session`, the host reads the selected tab's declarations or its display copy, or sends a message. Each such edit replaces that one read or call where it stands; the function's control flow, sizes, colors, and wording stay as the reference has them. A check compares every moved function with its source. The host's window, tab chrome, panel column and edge tab, menu rows, icons, fonts, and grid are already there from 0.1.

- `src/canvas.rs` → `host/src/canvas.rs`: move and edit. Pointer and pen handling, painting (`paint`, `paint_mark`, `paint_selection`, the handles, `paint_dashed_rect`), the marquee, transform previews, and the text editor and caret stay. `end_stroke`, `finish_click`, and the transform, text, and marquee endings send a gesture or a selection instead of committing. `handle_keys` and `run_command` send a declared command. Zoom in, zoom out, fit, and zoom to selection become the drawing surface's own commands, ids beginning `canvas.`, which the host carries out on its camera and reports to Simple Sketch. Simple Sketch declares their labels, icons, and keys and places them in its toolbar like any other command. The maximize toggle is the host window's. `persist_tools` and `resolve_dialogs` do not move. `sync_title` does not move; the title is Inpainter.
- `src/tablet.rs` → `host/src/tablet.rs`: move. `vendor/octotablet` → `host/vendor/octotablet`: move, with its `PATCHED.md` and the `[patch.crates-io]` path.
- `src/toolbar.rs`, the functions 0.1 did not move (`swatches`, `swatch_dot`, `swatch_color`, `mix_white`, `command_button`, `tools`, `context_actions`, `context_menu`, `tool_bar`, `list_button`, `bar_slider`, `size_control`, `zoom`, `paint_drawn_icon`, `teardrop`, `DRAWN_ICONS`) → `host/src/toolbar.rs`: move and edit. Each group is drawn from the declared `toolbar`, in the order it is declared, with the reference's sizes. A click sends its command or property.
- `src/panel.rs`, the functions 0.1 did not move (`back_row`, `draw_section`, `split_row`, `row_id`, `draw_row`, `paint_option`, `number_row`, `preset_row`, `paint_stroke_sample`, `color_well`, `curve_editor` and its helpers) → `host/src/panel.rs`: move and edit. Each section and row is drawn from the declared `properties`, by its kind. An edit sends a property message.
- `src/widgets.rs`, the controls 0.1 did not move → `host/src/widgets.rs`: move.
- `src/outliner.rs` → `host/src/outliner.rs`: move and edit. Rows come from the display copy's node tree. Renaming, the eye and lock, and a reorder drag send commands.
- `src/tabs.rs`: `menu`, `recent_rows`, `tab_row`, and `tab_chip` with the unsaved dot → `host/src/tabs.rs`: move and edit. Menu rows come from the declared `menu`. The plus, a tab's close mark, and selecting a tab send commands. `confirm`, `replace_dialog`, `recovery_dialog`, and `error_dialog` → `host/src/ask.rs`: move and edit. Each draws the question an `ask` message names, with its title, text, and buttons, in the reference's look, and returns the answer.
- `host/src/process.rs` and `host/src/apps.rs`, from 0.1: edit. The host keeps one `Running` per started sub-application, `--app` may be repeated, and each tab records which sub-application it belongs to.
- `src/dialogs.rs` → `host/src/dialogs.rs`: move and edit. `pick_open` and `pick_save` open when Simple Sketch sends a `file` request, and return the path.

## Into the sub-application `apps/simple-sketch/`

- `src/session.rs` → `apps/simple-sketch/src/session.rs`: move and edit. Copy the whole file first; a new, smaller session written in its place is not this entry, even if it passes the no-host check. What the host now does is removed: the open gesture and its previews (`begin`, `add_sample`, `begin_shape`, `move_shape`, text placing, dragging, caret, metrics, and draft-box previews, `set_transform_preview`), the camera and view (`pan_screen`, `zoom_toward`, `zoom_at_center`, `fit`), hit testing and paint lists, and the panel's open state. `commit`, `commit_text`, `commit_text_box`, and `commit_preview` take the finished gesture from the host instead of reading an open gesture. Everything else stays: the open sketches and tabs, ids, the clipboard, tools, palettes and sizes, the panel's subject and pages and property edits, files, recovery and recent files, commands, and undo and redo.
- The fields that leave `Sketch` (`id`, `name`, `generated_name`, `stored`, `dirty`, `path`, `recovery_file`, `undo`, `redo`) → a record in `apps/simple-sketch/src/session.rs` that wraps one `draw::Sketch`. Every edit to a sketch goes through this record's `commit_step`, which applies the `Step` through `draw` and pushes it onto the undo history, clearing redo. `undo` and `redo` pop a step and call `revert` or `apply`. Nothing in Simple Sketch calls `Sketch::apply` or `Sketch::revert` except these three. After one stroke, the record's undo history holds one step, and undo removes the stroke. This record supplies `id` and `name` when the snapshot is written and read.
- From `src/tabs.rs`, the quit and close walk (`start_quit`, `advance_quit`, `stop_confirm`, `cancel_save_as`, `finish_close`) → `apps/simple-sketch/src/session.rs`: move and edit. Each question it asked becomes an `ask` message.
- `src/registry.rs` → `apps/simple-sketch/src/registry.rs`: move and edit. Tools, commands, chords, menus, and the toolbar layout stay as tables. They are written out as declarations with their labels, icons, and shortcuts, so the host needs nothing from this file.
- `src/schema.rs` → `apps/simple-sketch/src/schema.rs`: move and edit. Property descriptors, written out as declared properties with their kinds and ranges. It uses `PropId`, `PropValue`, and `Inspect` from `draw`.
- `src/store.rs` → `apps/simple-sketch/src/store.rs`: move. `src/prefs.rs` → `apps/simple-sketch/src/prefs.rs`: move and edit. The folder is the `--data-dir` the host passes.
- `src/canvas.rs` `install_panic_hook` → `apps/simple-sketch/src/main.rs`: move and edit. A panic of this process flushes its own recovery copies.
- `apps/simple-sketch/src/main.rs`, the scaffold, grows the protocol loop.

## Left in the reference

`packaging/`, `release-notes/`, the reference `README.md`, `lib.rs`'s `canvas::run`, `SketchApp` as a window, and `sync_title`.

## New, awaiting the owner

- The `draw` crate's `Gesture` value and `apply` and `revert`, cut from code that committed and recorded in one step.
- The protocol messages in [systems.md](systems.md). The first attempt's declarations carried only ids and relied on the host reading Simple Sketch's registry. These carry their labels, icons, shortcuts, and kinds.
- The host's handling of `ask` and `file`, and its notices for H-2, from the two images in INTERFACE's Sub-application status, which the owner approved on 2026-10-14.
- Running several sub-applications at once, as the owner set on 2026-10-14: one process, reader thread, and set of declarations each, and the selected tab choosing whose declarations are drawn.
- A test-only sub-application in `tests/fixture`, with no manifest, for the two-sub-application check in systems, and the stand-in sub-applications for the one-owner check.

## As built, slice 1 (2026-10-14)

Simple Sketch's `session.rs`, `registry.rs`, `store.rs`, and `prefs.rs` were copied whole and edited only where the record split requires it: 169 of the session's 7,313 lines differ from `887f6f4`, every one routing an edit through the record, typing a sketch as the record, pointing a path at `draw`, or restoring paste's select-after-insert order. The record has the reference's three kinds of save: `commit` (apply and record), `record` (already applied), and `preview` (no history), plus `move_view`, the reference's `note_view`. Every `draw` method that returns a step is `#[must_use]`, which found 20 places where a step was being dropped. All 88 of Simple Sketch's own session, registry, schema, and preferences tests pass on the moved code.

One departure from the entry above: the window-side parts of the session (gesture state and previews, the camera's pan and zoom, hit testing, paint lists, the panel's open state) were not removed yet. The host does not do them until slice 2, and the moved tests exercise them. A finished gesture from the host is replayed through the session's own `begin`, `add_sample`, and `commit`, so the commit is exactly the one Simple Sketch 1.0.1 made. They are removed in slice 2, as the host takes each over.

`draw` gained, beyond what was already listed: `set_camera`, `Node::id`, the test-only helpers behind a `test-support` feature, and paste no longer selecting before insertion.

## As built, slice 2 and H-2 (2026-10-15)

The host's `canvas.rs`, `toolbar.rs`, `panel.rs`, `outliner.rs`, `tabs.rs`, `widgets.rs`, `tablet.rs`, and `dialogs.rs` are the reference's, moved, with each read or call of the session replaced as the rule above says. The session's place in `canvas.rs` is taken by `host/src/doc.rs`: each of its methods stands where `canvas.rs` called the session, and reads the display copy or the declared tool, previews on the display copy, or sends a message. The reference's live stroke, eraser trail, and shape previews are there, moved from the session. `confirm`, `replace_dialog`, `recovery_dialog`, and `error_dialog` became `host/src/ask.rs`, which draws any `ask` in their look. The octotablet patch moved to `host/vendor/octotablet`.

In Simple Sketch, the window's `Chrome` (the close confirmation, replace question, recovery offers, file errors, and the quit walk) moved to `apps/simple-sketch/src/ask.rs`, each dialog now an `ask`. What `toolbar.rs`, `tabs.rs`, `panel.rs`, `outliner.rs`, and `canvas.rs` read from the session is written out by `apps/simple-sketch/src/session/declare.rs`, a child of the session that reads it as those files did. The registry's tables stay tables; they are declared from there.

Departures:

- The session's gesture path (`begin`, `add_sample`, `begin_shape`, `move_shape`, `begin_text`, `move_text`, `end_text`, `commit`) stays, as the commit path a finished gesture is replayed through, so the commit is exactly Simple Sketch 1.0.1's. The previews and window-only parts nothing calls any more were removed: the caret and draft-box previews, the text drag, the transform wait (the host holds shortcuts while a selection is dragged), and the platform data folder. The window-side helpers the moved tests still call (paints, hit testing, the camera, the selection's handles) stay until those tests move to the host in the test pass.
- `BoxSide` and `dragged_box` moved from the session to `draw`, since the host previews a text box's drag with them.
- The panel's open state stays with Simple Sketch, as its session kept it; see the As built section of systems.
