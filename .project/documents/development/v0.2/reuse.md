# Reuse map — 0.2

**Reference:** `src/simple-sketch` at `887f6f4d7813f0a2cf1114f496f6b7eb33f18c4f` (version 1.0.1).
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
- `src/document/snapshot.rs` → `draw/src/document/snapshot.rs`: move. The `.smx` value, `format_version` 1. It is also the form in which a document crosses the protocol.
- `src/document.rs` → `draw/src/document.rs`: move and edit. Nodes, palettes, `Step`, `Camera`, `Allocator`, hit testing, scene and paint lists, and the shape and stroke paths stay. Edits:
  - `Sketch` keeps the model only: `origin_canvas`, `born`, `nodes`, `selection`, `active_canvas`, and `camera`. Its `id`, `name`, `generated_name`, `stored`, `dirty`, `path`, `recovery_file`, `undo`, and `redo` move to Simple Sketch, below.
  - `commit_step`, `undo`, and `redo` become `apply(&Step)` and `revert(&Step)`, which change the model and keep no history.
  - Items the host or Simple Sketch call become `pub`.
- From `src/session.rs`: `OpenGesture`, `GestureKind`, and the sample handling in `add_sample` → `draw/src/gesture.rs`: move and edit. A gesture starts from the live tool settings the sub-application declares, not from session fields. It ends as a `Gesture` value to send, not as a `Step`.
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

- `src/session.rs` → `apps/simple-sketch/src/session.rs`: move and edit. What the host now does is removed: the open gesture and its previews (`begin`, `add_sample`, `begin_shape`, `move_shape`, text placing, dragging, caret, metrics, and draft-box previews, `set_transform_preview`), the camera and view (`pan_screen`, `zoom_toward`, `zoom_at_center`, `fit`), hit testing and paint lists, and the panel's open state. `commit`, `commit_text`, `commit_text_box`, and `commit_preview` take the finished gesture from the host instead of reading an open gesture. Everything else stays: the open sketches and tabs, ids, the clipboard, tools, palettes and sizes, the panel's subject and pages and property edits, files, recovery and recent files, commands, and undo and redo.
- The fields that leave `Sketch` (`id`, `name`, `generated_name`, `stored`, `dirty`, `path`, `recovery_file`, `undo`, `redo`) → a record in `apps/simple-sketch/src/session.rs` that wraps one `draw::Sketch`. `commit_step` pushes onto this record's history after `apply`.
- From `src/tabs.rs`, the quit and close walk (`start_quit`, `advance_quit`, `stop_confirm`, `cancel_save_as`, `finish_close`) → `apps/simple-sketch/src/session.rs`: move and edit. Each question it asked becomes an `ask` message.
- `src/registry.rs` → `apps/simple-sketch/src/registry.rs`: move and edit. Tools, commands, chords, menus, and the toolbar layout stay as tables. They are written out as declarations with their labels, icons, and shortcuts, so the host needs nothing from this file.
- `src/schema.rs` → `apps/simple-sketch/src/schema.rs`: move. Property descriptors, written out as declared properties with their kinds and ranges.
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
