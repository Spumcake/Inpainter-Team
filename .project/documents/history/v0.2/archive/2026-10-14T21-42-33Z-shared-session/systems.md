# Inpainter — systems

**Inputs and baseline:** The mission is the product's identity. Accepted scope is [tasks.md](tasks.md). Sketching behavior is Simple Sketch 1.0.1. What is carried over is [reuse.md](reuse.md). The owner approved that map on 2026-10-14. This document follows it and adds no implementation the map does not carry. The messages below are the contract for the three crossings that map names.

Current fact: the reference is one program, `src/simple-sketch` at `887f6f4`. The app checkout is `src/inpainter/worktrees/dev`. On 2026-10-14 its committed revision is `a40e07c`, and the `apps/simple-sketch` scaffold is uncommitted on top of it. The workspace members are `host`, `protocol`, `apps/template`, and `apps/simple-sketch`. There is no `sketch` crate. `protocol/src/lib.rs` speaks JSON Lines, one object per line, a `type` field, and `PROTOCOL_VERSION` 1. `AppMessage` is `Hello`, `Tab`, `Contents`, `Panel`, and `End`. `HostMessage` is `Shutdown`. `ContentsKind` is `Grid`. `PanelSide` is `Right`. `Running::apply` in `host/src/process.rs` drops every line once the declaration is no longer pending. `Running::poll` records exit. `Running::exited` is that record, and the host does not draw it.

The pipe, the manifest, and the shell frame stay as version 0.1 left them. This document does not reimplement drawing, the toolbar, or the panel. Those stay the moved code named in the reuse map. The proposal is the process boundary that code does not yet have: one shared library, and the messages on this same protocol for the three crossings the map names.

## Boundaries and ownership

Proposal, awaiting the owner. Four workspace members do this work. `apps/template` stays as it is.

- `host/` — binary package `inpainter`. The window, the pen, and the live copy used for drawing. It depends on `sketch` and `protocol`. It does not depend on `simple-sketch`.
- `sketch/` — library package `sketch`. Drawing, erasing, shapes, text, ordering, the document, the session, the registry, and the schema. It depends on neither binary. `eframe` is a dependency because the moved text painting uses egui. It does not publish `canvas::run`.
- `protocol/` — library package `protocol`. Parsing and encoding. It depends on `serde` and `serde_json`. It does not depend on `sketch`, and it does not draw.
- `apps/simple-sketch/` — binary package `simple-sketch`. Arrangement, `.smx` save, and undo. It depends on `sketch` and `protocol`. It does not depend on `inpainter` or on `apps/template`. It does not open a window.

The **host** owns the window, the title Inpainter, the application id `io.github.spumcake.Inpainter`, the panel's open flag, the child process, the pen, and the live copy. Drawing, erasing, shapes, text, and ordering run on that copy, so pen samples never leave the process. Pan, zoom, and fit change that copy's camera and do not push a step. The host stores preferences and this product's recovery files. `prefs::data_dir` is `ProjectDirs::from("com.spumcake", "Spumcake", "Inpainter")`. Recovery files are `recovery/` under that directory. Only the host calls the preference functions and the recovery functions. `poll_recovery`, `publish_emergency`, and `write_emergency_recovery` run only in the host.

The **app** owns how the sketch is arranged, how it is saved, and its undo. Its `Session` holds the undo and redo stacks. `Step` is not in the snapshot and is not a message. The app calls `undo`, `redo`, `commit_step`, `add_canvas`, `group_selected`, `ungroup_selected`, and the save marks. It calls `read_smx` and `write_smx`. The refusal sentence stays "It is not a Simple Sketch file, or it is damaged."

The **protocol** crate owns the bytes on the pipe. Names of commands, tools, sections, and properties are strings. The sketch crate's `CommandId`, `Tool`, `SectionId`, and `PropId` are the authority for those strings, serialized in snake case, the same renaming this protocol already uses for `type`. The protocol crate does not copy those enums.

A sub-application can be driven with no host. The same messages are written to its standard input and read from its standard output. The host process is not running on that path.

## Source map

The reference commit is `887f6f4`. Copy each moved file, then make only the edits the reuse map names. The map is the list of paths. This section names only what the messages require.

Current: `protocol/src/lib.rs`, `host/src/process.rs`, and `apps/simple-sketch/src/main.rs`. The scaffold writes `hello`, one tab `simple-sketch` labeled Simple Sketch, `contents` grid, `panel` right, and `end`, then exits on `shutdown`.

Proposal: the `sketch` crate, the host and app edits the map names, and the message variants below. `protocol/src/lib.rs` keeps `PROTOCOL_VERSION` at 1. `ContentsKind::Grid` and `PanelSide::Right` stay the only variants. No second framing is added.

## Data and contracts

The pipe stays the child's standard input and standard output. Framing stays JSON Lines: UTF-8, one JSON object per line, ended by `\n`, a trailing `\r` stripped. Every object has a `type` field. Strings are JSON strings. `encode_app_message` and `encode_host_message` stay compact JSON plus the newline. `Hello` with `protocol` 1 is still first, once. `end` still completes the declaration. The scaffold's five lines remain valid for a sub-application that declares nothing further. Simple Sketch, in this proposal, also sends the three crossings.

`Running::poll` keeps reading after `Accept::Ready`. A later line is applied. It does not return the declaration to pending, and a line the parser rejects does not clear the shell or the live copy. `Running::exited` still records that the child has left. The host does not draw that record. The reference has no status view, no wording for it, and no timer. Nothing in `src/simple-sketch` treats a live process as not responding. A process that has stopped responding is unspecified until the owner supplies a picture. This proposal does not choose a view, a wording, a color, or a placement for it.

### Document

Proposal. The app sends `document` after `end`, and again whenever the committed sketch changes. The host replaces that sketch's nodes and camera from it. The host also sends `document` after it has applied a finished stroke, erase, shape, text edit, or ordering change on its copy, and after a camera change that must be stored. Both directions use the same snapshot. `Sketch::snapshot_value` is that value: `format_version`, `id`, `name`, `camera` (`origin`, `zoom`), and `nodes`. History, selection, the clipboard, tool settings, and the panel are not in it and stay out of it. Samples are not sent. The `.smx` reader still rejects any `format_version` other than 1. A rejected snapshot leaves the previous nodes and camera in place.

```json
{"type":"document","dirty":false,"sketch":{"format_version":1,"id":1,"name":"Untitled 1","camera":{"origin":[0.0,0.0],"zoom":1.0},"nodes":[]}}
```

Fields:

- `sketch` — the object `snapshot_value` returns, unchanged.
- `dirty` — on the app's message only. The snapshot has no dirty flag, and the host draws the reference dirty dot from the live copy. The app sets this from the sketch it commits. The host copies it onto that copy and does not write it into a `.smx`.

The tab `id` is the decimal text of `sketch.id`. A document whose id has no tab is ignored. The host replaces nodes and camera only. It does not take the name, the path, the selection, or the undo stack from the snapshot. The tab label stays the `tab` message: `file_title` when the sketch has a path, otherwise the generated Untitled name.

On the host's message the fields are `sketch` and `step`.

- `step` — `true` after a finished stroke, erase, shape, text edit, ordering change, or other change the reference records as one undo step. The app builds a `Step` with the existing variants from the document it already holds and the snapshot it received, calls `commit_step`, then sends `document` back. The host replaces its nodes and camera from that reply and does not send the reply on again. `false` for pan, zoom, and fit. The app stores the camera with the document and does not push a step. It does not reply.

`Step` stays inside the app. The protocol does not add a step variant. A change that cannot be recorded with the existing variants is returned to the coordinator. It is not given a new variant in place.

### Command

Proposal. The host sends `command`. The app applies it with the same `Session` method the host uses for that command, which is also how Simple Sketch is driven with no host. A gesture's samples are not a command. Pen input stays in the host.

```json
{"type":"command","id":"new_canvas","viewport":[0.0,0.0]}
```

Fields:

- `id` — snake case name of a `CommandId`. The app calls `perform` with that id and `viewport`. Unknown names are ignored.
- `viewport` — the point `perform` takes, two numbers. Commands that do not read it still receive it, so the no-host driver calls the same method.
- `path` — present only for `open`, `save`, `save_as`, and `save_copy`, after the host's dialog has returned a path. The app calls `open_path` or `save_path`. It does not open a dialog. `save_copy` is the `copy` argument. The host does not write the `.smx`.
- `replace` — `true` or `false`, only with a save `path`. The host sets it from the replace dialog. Absent means `false`.
- `discard` — `true` only with `id` `close`, after the person chooses Discard. The app calls `discard` for the active sketch. Absent, `close` calls `request_close`.
- `tool` — snake case name of a `Tool`, and only when `id` is absent. A tool click in the host is this message. The app calls `set_tool`, then sends `toolbar` and `properties` for the page that tool shows. The host paints those rows and does not choose a page by matching `Tool::Brush` or any other tool.

The host shows the keep-this-sketch dialog from `dirty` before it sends `close`. The app does not show it. There is no window on the no-host path, so a script that sends `open` or `save_as` includes `path`.

When `save_path` needs the replace dialog, or a save or open fails, the app writes `result` and does not change the sketch.

```json
{"type":"result","id":"save_as","path":"/home/someone/Sketch.smx"}
```

```json
{"type":"result","id":"open","saving":false,"file":"Sketch","reason":"It is not a Simple Sketch file, or it is damaged."}
```

Fields: `id`, and either `path` (the file the replace dialog asks about) or `saving`, `file`, and `reason` (the failure the error dialog already shows). The host shows the dialog it already carries. The wording stays the reference wording.

After a command that changes which sketches are open, or a tab's label, the app sends `tab`, `contents`, and `panel` for each sketch that remains, then `menu`. That sequence replaces the open tabs. A tab omitted from it is closed. A new id is added. A `menu` with no `tab` ahead of it leaves the tabs as they are. Each new or remaining sketch is also one `document`.

### Menu, toolbar, and properties

Proposal. The app builds these from `registry` and `schema` and sends them before `end`, and again whenever they change. The host paints what it is sent. Labels, icons, and shortcuts stay the registry and the schema. The host looks them up. It does not keep a second table, and it does not choose the property page or the toolbar's tool row by matching a tool.

`menu` is `sketch_menu`, top to bottom.

```json
{"type":"menu","rows":[{"command":"new_sketch","enabled":true},{"recent":true},{"command":"close","enabled":false}]}
```

A row is either `command` plus `enabled`, or `recent`. `command` is a `CommandId` name. `enabled` is the app's `command_enabled` result. The undo stack is on the app, so the host cannot compute it. `recent` is the submenu labeled `RECENT_MENU_LABEL` ("Open Recent"). The recent paths stay on the host, which is the only process that calls `save_recent`. The host fills that submenu from its list and enables it from that list.

`toolbar` is the groups the registry already defines, in the order `show` already paints: file commands, undo and redo, canvas commands, the tools, the current tool's bar or the select tool's actions, then zoom, fit, and maximize. Separators, the scrolling row, and the zoom group pinned to the right stay as the moved toolbar draws them. Swatches stay on the host's live copy. They are not a message. The host paints them from that copy, in front of these groups, and leaves them out for a tool that has no palette.

```json
{"type":"toolbar","groups":[{"commands":["new_sketch","open","save"]},{"commands":["undo","redo"]},{"commands":["new_canvas"]},{"tools":[{"id":"brush","current":true},{"id":"eraser","current":false}]},{"controls":[{"size":true}]},{"commands":["zoom_out","zoom_in","fit","maximize"]}]}
```

A group is `commands` (CommandId names), `tools` (`id`, and `current` on exactly one), or `controls`. A control is the current tool's `BarControl` or `SelectItem`, using the fields those values already have:

- `{"size":true}`
- `{"divider":true}`
- `{"slider":{"prop":"text_padding","label":"Padding","min":0,"max":48}}`
- `{"choice":{"prop":"align","options":[["align-left","Align left"]]}}`
- `{"list":true}`
- `{"command":"bring_to_front"}`
- `{"menu":{"label":"Rotate","icon":"refresh","commands":["rotate_clockwise","rotate_counterclockwise","rotate_half"]}}`

`current` is how the host draws the current tool. It does not compare the click to `Tool`. A tool setting edited from a control stays on the host's session and is not an undo step. A color or object edit is applied on the host's copy and then sent as `document` with `step` true.

`properties` is the page to paint now: the section lists the schema already has, in order, and the prop ids inside each section.

```json
{"type":"properties","sections":[{"id":"preset","props":["preset"]},{"id":"dynamics","props":["pressure_size"]},{"id":"pressure_curve","props":["curve"]},{"id":"behavior","props":["smoothing","stabilization","streamline"]}]}
```

`id` is a `SectionId` name. Each `props` entry is a `PropId` name. The host calls `section` and `prop` for the title, the label, and the kind, and reads the value from its live copy. It replaces the rows it paints when another `properties` arrives. The page has no Properties header, no close button, and no tool-name card. It is drawn only when `shows_panel` is true, only on the right.

`Declaration::accept` treats `menu`, `toolbar`, and `properties` like a panel: after `hello`, before `end`, at most one of each, and each returns `More`. A second of the same kind before `end`, or a `document` or `result` before `end`, returns `Failed`. After `Ready`, another `menu`, `toolbar`, or `properties` replaces that one. The shown declaration stays the tabs, contents, and panels already accepted.

## Main flow and failures

Proposal for the sketch path. The person launches the host. The host starts `simple-sketch` from its manifest, as it does now. The app writes `hello`, one `tab` per open sketch, `contents` grid and `panel` right for each, then `menu`, `toolbar`, and `properties` for the session's starting tool, then `end`, then one `document` per sketch. The host draws that declaration. The grid uses the live copy's camera once the document has been applied. Until the first accepted document, the grid stays the centered grid the scaffold already draws.

A stroke, erase, shape, text edit, or ordering change is applied on the host's copy. The host sends `document` with `step` true. The app records the step and sends `document` back. The host replaces nodes and camera. Pan, zoom, and fit send `document` with `step` false and do not become a step.

A command the person chooses, or a script on the no-host path, is `command`. The app applies the method above and then sends any tab sequence, documents, and rows that changed. `shutdown` is unchanged: the window is closing, and the app exits.

If the app exits, `poll` sets `exited`. The window stays open on the last declaration and the last live copy. No status is drawn. A spawn failure, a failed declaration, or an exit before `end` stays the empty declaration version 0.1 already shows. The host does not retry the spawn. A document the reader rejects leaves the previous nodes and camera. An unknown command id changes nothing.

## Environments and integration

Local work is the app checkout at `src/inpainter/worktrees/dev`. The launch command and the shutdown check that were run on 2026-10-14 are recorded in [tasks.md](tasks.md). Project documents stay in the team repository. The reference tree stays at `src/simple-sketch`.

Simple Sketch is a real child process. There is no mock service, no network, and no account. stderr is the host's stderr. No secret is required. The no-host check is the same binary, with no host process, fed `command` on stdin.

## Verification seams

During a slice, add no new tests. The existing scripted suite runs once when the change is complete. Tests for approved behavior are written in one pass when the owner asks to commit.

The boundaries worth that later pass, each catching one failure, are: a `document` whose `format_version` is not 1 leaves the previous nodes and camera; the host's replace copies nodes and camera and leaves the undo stack untouched; a `command` the app applies and the same `Session` method called in one process leave the sketch equal; `menu`, `toolbar`, and `properties` round-trip, and a second `properties` replaces the page rather than appending; the host package and the `simple-sketch` package do not depend on each other. That last check is the map's boundary check. It fails if either package depends on the other. Both may depend on `sketch` and on `protocol`.

H-4 is a human look at the running host, against the sentence the owner agrees for that slice and against the Simple Sketch reference. The preview is that window, with its tab strip, toolbar, contents, and panel. A headless read of the protocol does not replace it. H-2 waits until the owner supplies a picture of the status. This proposal does not specify that picture.
