# Inpainter — systems (0.2)

**Inputs and baseline:** The mission, including what would fake it, is the product's identity. Scope is [tasks.md](tasks.md). What carries over is [reuse.md](reuse.md), which applies the owner's decisions of 2026-10-14: the host previews and the sub-application commits; the whole document crosses after each change; the drawing surface is a shared component; Simple Sketch keeps its files in its own folder inside Inpainter's data folder. This replaces the [archived systems](../../history/v0.2/archive/2026-10-14T21-42-33Z-shared-session/systems.md), whose two copies of each sketch and two undo histories are the fake the mission names.

Current fact: Inpainter `dev` is at `a40e07c`, with the Simple Sketch scaffold and the scaffolding skill's fix uncommitted on top. Members are `host`, `protocol`, `apps/template`, and `apps/simple-sketch`.

## Boundaries and ownership

- `draw/` — library. The document model, `apply` and `revert` of a `Step`, ink, text layout, the `.smx` snapshot, defaults, the camera, and gesture previews. Depends on `eframe` (text paints with egui), `kurbo`, `i_overlay`, `earcutr`, `perfect_freehand`, `serde`, and `serde_json`. It holds no session, registry, schema, store, or preferences.
- `protocol/` — library. Messages and the manifest. Depends on `serde` and `serde_json`, not on `draw`. A document, a gesture, and a tool's live settings travel as JSON values whose shape `draw` defines.
- `host/` — the `inpainter` program. Depends on `protocol`, `draw`, `eframe`, `iconflow`, `octotablet`, `rfd`, and `directories`. It never depends on a sub-application.
- `apps/simple-sketch/` — depends on `protocol`, `draw`, `serde`, `serde_json`, and `directories`. It has no window.
- `apps/template/` — unchanged.

**Simple Sketch owns:** its open sketches and their order, ids, each sketch's undo and redo, save state, paths, and recovery; the clipboard; tools, palettes, sizes, and the active color; the panel's subject and pages; selection; recent files and preferences; every command. **The host owns:** the window and common UI; which sub-applications are running and which tab belongs to which; which tab is selected; whether the panel is open; the camera of each tab while it is shown; the gesture in progress; the display copy, which it only ever replaces. The host has no undo history, allocates no ids, and writes no sketch file.

## Data and contracts

**Several sub-applications at once**, as the owner set on 2026-10-14. The host starts every sub-application `--app <id>` names, and `--app` may be given more than once; without it, the host starts the first by id, as in 0.1. Each running sub-application has its own process, reader thread, and declarations. A tab belongs to the sub-application that reported it. The strip shows every running sub-application's tabs, in the order their programs started, then in the order each reports them. The toolbar, menu, panel, and contents are always the selected tab's sub-application's declarations: selecting a tab of another sub-application swaps all of them, and sends that sub-application `command select_tab`. The frame loop never waits on a sub-application; each is read on its own thread.

JSON Lines as in 0.1, `PROTOCOL_VERSION` 1. The messages below are added. Existing ones keep their meaning. Every declaration carries what is needed to draw it; the host never looks up anything in a sub-application's code.

**From the sub-application:**

- `commands {commands: [{id, label, icon?, keys: [{key, ctrl, shift, alt}], enabled, checked?}]}` — every command, resent when any changes. Menus and toolbars refer to these ids. Ids beginning `canvas.` (`canvas.zoom_in`, `canvas.zoom_out`, `canvas.fit`, `canvas.zoom_to_selection`) belong to the drawing surface: the host carries them out on the tab's camera and sends `camera`. The host recognizes no other command id.
- `menu {rows: [{command, label?, arg?} | {divider} | {submenu, rows}]}` — the hamburger menu of this sub-application's tabs. A recent file is a row with command `open_recent`, its file name as `label`, and its path as `arg`.
- `toolbar {groups: [...], trailing: [...]}` — group kinds: `commands` (ids), `swatches` (`id`, `rgb`, `opacity`, `active`, `quiet`), `size` (`value`, `min`, `max`, `rgb?`), `slider` (`prop`, `label`, `value`, `min`, `max`), `choice` (`prop`, `value`, options with `value`, `icon`, `label`), and `menu` (`label`, `icon`, command ids). `trailing` is pinned to the right, as the reference's zoom group is.
- `properties {back?, sections: [{title, props: [{id, label, kind, value, ...}]}]}` — kinds: `toggle`; `number` with `min`, `max`, `step`, `unit`, `format`; `choice` with options and `style` (dropdown or segmented); `color` with `rgb` and `opacity`; `curve` with points; `font` with families; `text`; `preset` with options; `palette` with rows.
- `tabs {tabs: [{id, label, dirty, close_waits}], active, new_tab?}` — replaces the tab list. `new_tab` names the declared command the plus runs for this sub-application's tabs. Without it, as for the template, the plus does nothing.
- `contents {tab, kind: "canvas"}` — the drawing surface, which includes the grid. `"grid"` alone stays for the template.
- `document {tab, sketch, selection, active_canvas, camera?}` — the whole document after any change. `camera` is sent only when a sketch is opened or created; otherwise the host keeps its own.
- `pen {tab, tool, settings}` — the current tool's live settings for previews: color, opacity, size, dynamics, and the target canvas, or that the tool cannot draw now.
- `ask {id, title, text, buttons: [{id, label, primary?}]}` — a question the host shows in the reference's dialog look. A failed save is an `ask` with one button.
- `file {id, kind: "open" | "save", name?, folder?}` — a file dialog for the host to open.
- `quit_ready` — every unsaved sketch has been resolved.
- `pong` — the answer to `ping`.

**From the host:**

- `command {id, tab?, arg?, viewport}` — a menu row, toolbar button, shortcut, the plus (sending the selected tab's sub-application's `new_tab` command), a tab's close mark, selecting a tab, or an outliner action.
- `gesture {tab, gesture}` — one finished gesture.
- `select {tab, ids}` — a click or marquee result. Simple Sketch applies it and sends `document`. The host shows a selection only as the next `document` states it, never before.
- `camera {tab, origin, zoom}` — after a pan, zoom, or fit. Not a history step.
- `property {prop, value, preview}` — a panel or toolbar edit. `preview: true` while a slider is held; the release sends `false`, which is one undo step.
- `answer {id, button}` and `file_result {id, path?}`.
- `quit` — the window's close was pressed. The window stays until `quit_ready`.
- `ping`, every second; `shutdown`, as in 0.1.

The host starts a sub-application with `--data-dir <Inpainter data folder>/apps/<id>`, creating the folder first. Without it, as in the headless test, Simple Sketch writes no preferences or recovery files.

## Main flow and failures

A stroke: the host previews it from `pen` and the samples; on release it sends `gesture`; Simple Sketch commits a `Step`, records it, and sends `document` and `tabs`; the host replaces its display copy. Undo: the host sends `command undo`; Simple Sketch reverts its last `Step` and sends `document`. Save: `command save`; with no path, Simple Sketch sends `file`, the host returns `file_result`, and Simple Sketch writes and sends `tabs`. Closing a tab with changes: Simple Sketch asks "Keep this sketch?" with `ask`. Quitting: the host sends `quit`, Simple Sketch walks its unsaved sketches with `ask`, then sends `quit_ready`, and the host shuts it down and exits.

The notices follow the two images the owner approved on 2026-10-14, in INTERFACE's Sub-application status. When the process exits, the host shows the exited notice in each of its tabs. With no `pong` for three seconds, it shows the not-responding notice, and clears it when `pong` returns. While that notice shows, panning and zooming still work, and commands and gestures wait. The common UI stays usable throughout.

Known cost: the whole document crosses after every change, including each frame of a held slider. A large sketch may lag; sending `Step`s instead is the remedy, later.

## Verification seams

These are the mission's tests. Each one fails on the fake it names.

- **Boundary.** `draw` has no module named or containing a session, registry, schema, store, or preferences, and the host depends on no sub-application crate. A check on `cargo metadata` and the module list fails otherwise.
- **Two sub-applications.** One window runs Simple Sketch and a test-only sub-application in `tests/fixture`, which has no manifest and is started by the test. The fixture declares a toolbar, menu rows, and properties whose labels Simple Sketch does not use, and none of its command ids begins with `canvas.`. Its toolbar groups come in an order Simple Sketch does not use, with no trailing group; its menu is a divider and a submenu; and its `new_tab` is a command id Simple Sketch does not declare. The host draws the fixture's groups in the fixture's order, and the plus on the fixture's tab sends the fixture's `new_tab` command. Selecting the fixture's tab shows only the fixture's labels; selecting a Simple Sketch tab shows only Simple Sketch's. Clicking the fixture's toolbar button and menu row sends their commands to the fixture's process. The host crate depends on neither. Then a stand-in registered with Simple Sketch's own id, `simple-sketch`, declares a toolbar, menu, and panel with different labels from the real Simple Sketch's: the host shows the stand-in's labels, so it cannot be drawing Simple Sketch's chrome from anything but declarations.
- **One owner.** A stand-in sub-application answers a stroke `gesture` with a document that does not contain that stroke, answers `select` with a different id, and answers `undo` with a document that is not the inverse of the last gesture. After the pen lifts, the host shows no stroke: its preview ends and only the stand-in's document is drawn. The host shows exactly what it was sent, and its screen does not change between sending and the reply. With a stand-in that never writes files, Save leaves no file. With Simple Sketch, the `.smx` file is opened for writing by Simple Sketch's process, as a trace of the two processes shows.
- **Carried-over chrome.** A check diffs each function the reuse map moves against its source at `887f6f4`. Every changed line must be one that reads the selected tab's declarations or the display copy, or sends a message, in place of reading or calling `Session`; any other changed line fails the check.
- **No host.** With no host and no window, a separate client process starts `simple-sketch`, sends `command new_sketch`, a stroke `gesture` to the new sketch, an erase `gesture` across part of it, a `property` change to a declared property, `undo`, `redo`, and `save`, and answers Simple Sketch's `file` request with a temporary path. The file holds that sketch, which did not exist before the `new_sketch` command, and it matches what Simple Sketch's session produces for the same steps.
- **Stopped and not responding.** Kill the sub-application's process: the host stays up, the exited notice shows, and a pan still moves the view. Separately, stop the process from answering `ping`: the not-responding notice shows within three seconds, a pan still moves the view, and the notice clears when the process answers again.
- **The look.** The owner's preview of H-4 against the agreed sentence and the references, and of H-2 against the two images the owner approved.

Slices, in order: Simple Sketch working with no host, which covers `draw`, the record, the protocol, and the headless test; then the host drawing from declarations, which covers the canvas, toolbar, menu, panel, tabs, dialogs, several sub-applications at once, and the two-sub-application test; then H-2. The intent review checks this plan, each slice's first integration, and each finished slice.
