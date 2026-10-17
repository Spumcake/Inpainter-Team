# Inpainter — systems

**Inputs and baseline:** The mission is the product's identity. This document covers only the first slice of version 0.1, which the owner instructed to be built on 2026-10-14. Accepted scope is the first-slice paragraph of H-1 in [tasks.md](tasks.md), the picture [shell-demo.png](../../../renders/ui/shell-demo.png), [INTERFACE.md](../../../../INTERFACE.md), and the first-slice section of [reuse.md](reuse.md) through "First slice — into the template". The plan's shorthand that the template declares nothing is the milestone label; the accepted slice is the H-1 paragraph and that reuse section: one tab, Template, and the shared grid.

Current fact: the reference is one process, `src/simple-sketch` at `887f6f4d7813f0a2cf1114f496f6b7eb33f18c4f`. It has no child process and no pipe between programs. `std::process` appears there only as `process::id` in test directories. The app checkout is `src/inpainter/worktrees/dev/`. On 2026-10-14 its committed revision is `2595cfb`, and the first slice is uncommitted on top of it.

The shell's look, sizes, colors, and drawing are the moved code in the reuse map. This document does not reimplement them. It specifies the new process boundary for this slice: how the host starts the template, which declarations the template sends, how the host shuts the template down when the window closes, and how the host notices that the process has exited. That pipe is the H-1 contract two implementers build against. It is not a product decision for a later milestone.

## Boundaries and ownership

Three workspace members, and no others, in this slice:

- `host/` — binary package `inpainter`. The window. It depends on `protocol`, `eframe`, and `iconflow`. It does not depend on `template`.
- `protocol/` — library package `protocol`. The messages both programs use, one JSON object per line. It depends on `serde` and `serde_json`.
- `apps/template/` — binary package `template`. The sub-application. It depends on `protocol`. It does not depend on `inpainter`, and it does not open a window.

No `draw/` crate and no `apps/simple-sketch/` crate exist in this slice. The host links no app crate. The template links no host crate. Neither program imports the other.

The **host** owns the window, the title `Inpainter`, the application id `io.github.spumcake.Inpainter`, the panel's open flag, and the child process. The **template** owns the declaration it writes: one tab, the shared grid, and the properties panel on the right. The **protocol** crate owns parsing and encoding. It does not spawn processes and it does not draw.

The properties panel is a shared component of the host, as the owner set on 2026-10-14. The host draws it, always the same way, only for a tab whose sub-application declares it. The sub-application declares whether a tab has the panel, which side it sits on, and what it holds. Only the right side has a reference, so only the right side is built. When the selected tab has no declared panel, the host draws no edge tab and no column, F4 does nothing, and the flag is held closed. The panel flag starts closed (`false`), as the reference session does. The edge tab and F4 toggle it. F4 is the shortcut the reuse map moves into `host/src/icons.rs`. Opening the panel narrows the contents area by the moved `panel::WIDTH` on that frame. The template is not told. The flag is not a protocol field.

A click on the menu button, the plus, or a tab's close mark, including a middle-click on the close mark, changes nothing. The controls are still drawn. This slice has no menu-row, new-tab, or close message, so the host does not open a popup, add a tab, or remove one. The host does not send those clicks to the template. The only host-to-template message is shutdown, and only when the window closes.

## Source map

The reference commit is `887f6f4`. Copy each moved file, then make only the edits the reuse map names. Relative `include_bytes!("../assets/...")` paths stay as they are: `host/src/` and `host/assets/` match the reference layout of `src/` and `assets/`.

Moved into the host, from the first-slice section of the reuse map:

- `src/canvas.rs` window pieces → `host/src/window.rs`
- `src/canvas.rs` `paint_grid`, `screen_to_document`, `document_to_screen`, `GRID_SPACING` (`24.0`), and the two lines of `Camera::centered` → `host/src/grid.rs`
- `src/tabs.rs` → `host/src/tabs.rs`
- `src/widgets.rs` → `host/src/widgets.rs`
- `src/toolbar.rs` → `host/src/toolbar.rs`
- The icon, key, and chord pieces named in the reuse map, from `src/registry.rs` and the face-name strings from `src/text.rs` → `host/src/icons.rs`
- `src/panel.rs` → `host/src/panel.rs`
- `assets/icon/` and `assets/fonts/` → `host/assets/icon/` and `host/assets/fonts/`

`sync_title` does not move. The title is always `Inpainter`. `install_panic_hook` does not move. Sketch dialogs, tools, and property widgets do not move.

New in this slice:

- `protocol/Cargo.toml`, `protocol/src/lib.rs`
- `apps/template/Cargo.toml`, `apps/template/src/main.rs`, `apps/template/manifest.json`, `apps/template/AGENTS.md`
- `host/src/apps.rs`, which finds sub-applications by their manifests
- `AGENTS.md` at the repository root and the developer skill `skills/scaffolding/SKILL.md`, which copies `apps/template` to start a new sub-application
- Workspace `Cargo.toml`
- `host/Cargo.toml`, `host/src/main.rs`, `host/src/process.rs`

There is no `host/src/lib.rs`. The host is only the `inpainter` binary, so the template cannot depend on it as a library. `host/src/main.rs` declares the modules `apps`, `grid`, `icons`, `panel`, `process`, `tabs`, `toolbar`, `widgets`, and `window`.

Package manifests use edition `2024`, rust-version `1.95`, version `0.1.0`, and `publish = false`. Host dependencies, and no others: `eframe = "=0.36.2"`, `iconflow = { version = "=2.1.0", default-features = false, features = ["pack-iconoir"] }`, and `protocol` by path `../protocol`. Template dependencies: `protocol` by path `../../protocol`. Protocol dependencies: the reference's own versions, `serde = { version = "=1.0.229", features = ["derive"] }` and `serde_json = "=1.0.152"`. The app repository has a `.gitignore` holding `/target`.

The workspace `Cargo.toml` lists members `host`, `protocol`, and `apps/template` only.

## Data and contracts

The pipe is the template's standard input and standard output. The host spawns the template with stdin and stdout piped and stderr inherited. The child gets no arguments, no extra environment variables, and the host's current directory.

**Finding sub-applications**, as the owner set on 2026-10-14. The host knows a sub-application only through its manifest, `apps/<id>/manifest.json`, which `protocol::Manifest` and `protocol::parse_manifest` define: `id` (lowercase letters, digits, and hyphens, and the folder's name), `name`, `executable` (a file name, not a path), and `protocol`, which must be `PROTOCOL_VERSION`. Unknown fields are refused. `host/src/apps.rs` reads the folders under `apps/` beside the host executable and then, in a development build, the workspace's own `apps/` folder (`env!("CARGO_MANIFEST_DIR")/../apps`). The program is `executable` in the host executable's directory, where a workspace build places every binary. A manifest that does not parse, or whose program is missing, is reported on stderr and skipped; a repeated id is skipped. The host starts the sub-application that `--app <id>` names on its command line or, without `--app`, the first by id. An unknown id starts nothing, and the window opens on `Declaration::empty()`. The template is registered by `apps/template/manifest.json`. No launch command is recorded yet; tasks.md owns the command once the worktree exists.

Framing is JSON Lines: UTF-8, one JSON object per line, ended by `\n`. A trailing `\r` is stripped. Every object has a `type` field. Strings are JSON strings, so a label may hold spaces, quotes, or any other text. The protocol crate is the only parser and encoder, through `serde` and `serde_json`, so the two programs do not each invent a framing. `PROTOCOL_VERSION` is `1`.

App messages, written by the template to stdout (`AppMessage`):

- `{"type":"hello","protocol":1}` — always first, once. The host accepts only `PROTOCOL_VERSION`.
- `{"type":"tab","id":…,"label":…}` — one document the sub-application has open. `label` is the name of the file it manages or, when it manages none, the sub-application's name.
- `{"type":"contents","tab":…,"kind":"grid"}` — the shared component the host draws in that tab's contents area. `grid` is the only kind.
- `{"type":"panel","tab":…,"side":"right"}` — that tab uses the host's properties panel, on that side. `right` is the only side, because only the right side has a reference.
- `{"type":"end"}` — the declaration is complete.

The template writes these five lines, then flushes stdout before it reads stdin. Stdout is block-buffered when piped, so the flush is required:

```text
{"type":"hello","protocol":1}
{"type":"tab","id":"template","label":"Template"}
{"type":"contents","tab":"template","kind":"grid"}
{"type":"panel","tab":"template","side":"right"}
{"type":"end"}
```

Host messages, written by the host to the template's stdin (`HostMessage`):

- `{"type":"shutdown"}` — the window is closing. The host writes it only from `Running::shutdown`.

Types in `protocol`:

- `AppMessage::{Hello { protocol: u32 }, Tab { id, label }, Contents { tab, kind: ContentsKind }, Panel { tab, side: PanelSide }, End}` and `HostMessage::Shutdown`, each serialized with its `type` tag.
- `ContentsKind::Grid`, as `grid`. `PanelSide::Right`, as `right`.
- `TabReport { id, label }`, `ContentsReport { tab, kind }`, and `PanelReport { tab, side }` — the host's copies of accepted messages.
- `Declaration { tabs, contents, panels }` — the host is the only writer of the value it draws. `Declaration::empty()` has all three empty. The type has no tool, command, menu-row, property, or skill field. Those categories have no message in this slice, so the toolbar and the panel column stay empty.

Functions:

- `parse_app_message(line: &str) -> Result<AppMessage, ParseError>` and `parse_host_message(line: &str) -> Result<HostMessage, ParseError>`
- `encode_app_message(&AppMessage) -> String` and `encode_host_message(&HostMessage) -> String` — compact JSON plus the trailing newline
- `Declaration::accept(&mut self, message: AppMessage) -> Accept`

`Accept` is `More`, `Ready`, or `Failed`. `accept` requires `hello` with `PROTOCOL_VERSION` first; anything else first, a second `hello`, or another version returns `Failed`. It appends a tab when its id is new. It appends contents, or a panel, when `tab` is already a tab and that tab has none of that kind yet. Otherwise it returns `Failed`. `end` returns `Ready`, including when no tab was sent.

`ParseError` is `{ line: String }`, the line that failed. Callers do not draw it.

The host shows a declaration only after `Ready`. Until then, and after `Failed`, the shown declaration is `Declaration::empty()`. A `Failed` or a closed pipe before `end` discards the partial build. Messages after `Ready` are ignored.

`Running` in `host/src/process.rs` owns the child. The protocol crate does not know this type.

- `Running::start(executable: &Path) -> Running` — spawns the program `apps::choose` returned. On a spawn failure, returns the same state as `not_started`. On success, it moves the child's stdout to a reader thread (`std::thread`, `std::sync::mpsc`) and keeps stdin on the `Child` for shutdown. The thread sends parsed lines. `start` returns without waiting for `end`. The window opens either way.
- `Running::not_started() -> Running` — no process, `exited` true, declaration empty. `poll` and `shutdown` do nothing. `Running::declared(messages)`, compiled only for tests, is `not_started` showing the declaration those messages build. The moved frame tests use it with the template's messages, so they draw the template's tab, grid, and panel without spawning it.
- `declaration(&self) -> &Declaration` — empty until `Ready`, then the accepted value. It stays that value if the child later exits.
- `poll(&mut self)` — drains the channel and applies `accept`. Then, while `exited` is still false, `Child::try_wait`. `Ok(Some(_))` or `Err` sets `exited` and reaps. `Ok(None)` leaves the child running. Once `exited` is true, later polls skip `try_wait`. Lines in the same `poll` are applied before the exit is recorded, so an `end` that arrives with the exit is kept.
- `pending(&self) -> bool` — true until `Ready`, `Failed`, a closed pipe before `end`, or an exit before `end`.
- `exited(&self) -> bool` — the host's record that the process is gone, including a spawn that never started.
- `shutdown(self)` — if the child is still running, write the `shutdown` message, flush, and drop stdin so the template also sees EOF. Then wait up to `SHUTDOWN_GRACE`, two seconds, polling `Child::try_wait`. A child that has not exited by then is killed and reaped, so a stuck sub-application cannot keep the window from closing. Then join the reader thread. If it has already exited, skip the write and the wait. The frame loop keeps `Running` until the window closes. `on_exit` takes it out of an `Option` and calls `shutdown`.

The reader thread only sends `PipeEvent`. It does not write `Declaration`. `PipeEvent` is `Line(AppMessage)`, `Invalid(ParseError)`, or `Closed`. `poll` on the frame thread is the only writer of the shown declaration. `Invalid`, or `Closed` before `Ready`, discards the partial build and leaves the shown declaration empty.

The template reads stdin until `parse_host_message` returns `Shutdown` or the read returns EOF, then exits with status 0. Any other stdin line is ignored. The template does not restart, and the host does not spawn a replacement.

The first `TabReport` is the selected tab. This slice sends one. The host draws its `label` on the moved tab chip. When that tab's id has `ContentsKind::Grid`, the host fills the contents rectangle with the moved `CANVAS` color and calls the moved `paint_grid` with origin `[-width / 2.0, -height / 2.0]` and zoom `1.0`, which are the two lines of `Camera::centered`. Width and height are that rectangle's size in points. Spacing is `24.0`. The stroke is the moved `Color32::from_rgb(234, 235, 238)`. The contents area does not pan or zoom. Pointer input there does not start a stroke. When that tab has a `panel` with side `right`, the host draws the moved edge tab and, while open, the empty column. With an empty declaration, the tab strip is the menu button and the plus, the hairline runs the full width, the contents area is `CANVAS` with no grid, and there is no panel tab.

## Main flow and failures

The person launches the host. `main` calls `Running::start`, then `window::run(child: Running) -> eframe::Result<()>`. `run` is the moved `eframe::run_native` entry: title `Inpainter`, native name `Inpainter`, application id `io.github.spumcake.Inpainter`, and the moved `NativeOptions` otherwise. The creation closure builds the host app around that `Running`.

Each frame, the app calls `poll`. While `pending` is true, it requests another repaint so the declaration appears without a click. It draws the shell from `declaration()`. The steady picture, once the template's five messages have been accepted, is shell-demo.png as INTERFACE describes it: title Inpainter, menu button, one tab labeled Template, plus, empty toolbar, the grid, panel closed.

The template writes the five messages, flushes, and blocks on stdin. It has no window.

When the window closes, `on_exit` calls `shutdown`. The template exits, and the host reaps it. A sub-application still running after the two-second grace is killed, so the window always closes.

If the template process exits while the window is open, `poll` records `exited`. The host process and the window stay up. The shell stays as the last shown declaration left it: the same chrome, and the Template tab and grid when `end` had already been accepted. The panel tab and F4 still toggle the empty column. The menu button, the plus, and the close mark still do nothing. The frame loop does not branch on `exited` to add a control, a label, a dialog, or a different picture. The visible status of a stopped sub-application is H-2, and this slice does not draw it.

No registered sub-application, a spawn failure, a parse failure, or an exit before `end` opens the same window on `Declaration::empty()`. The shell is up. No status control is added. The host does not retry the spawn.

A template that stays running and never writes `end` leaves `pending` true. The shell stays on the empty declaration, with the extra repaint still requested. This slice does not add a timeout or a hung-state picture.

## Environments and integration

Local work is the app checkout at `src/inpainter/worktrees/dev/`. The launch command that was run on 2026-10-14 is recorded in [tasks.md](tasks.md). Project documents stay in the team repository. The reference tree stays at `src/simple-sketch`.

The template is a real child process. There is no mock service, no network, no account, and no fixture store. stderr from the template is the host's stderr. No secret is required.

## Verification seams

During this slice, add no new tests and design no suite. No scripted suite exists until the app is checked out. H-1 is a human look at the running host, after the owner agrees what that run shows, against the first-slice sentence in tasks.md and the shell in INTERFACE. The preview is that window, with its tab strip, toolbar, contents area, and panel tab.

The three tests the reuse map names move with `host/src/window.rs` and only with the mapped edits: `the_title_bar_is_left_to_the_desktop_theme`, `the_panel_tab_toggles_the_panel_without_drawing`, and `f4_opens_the_panel_and_narrows_the_canvas`. They construct the host window, not `SketchApp`, and they do not assert `stroke_count`. They build that window with `Running::not_started` and do not spawn the template. The other tests in the reference `canvas` module stay behind.

The host-side declaration test in the reuse map's New list, and H-5's scripted test, wait until the owner asks to commit. They are not specified here.
