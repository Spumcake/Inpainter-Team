# Reuse map — 0.1

**Reference:** `src/simple-sketch` at `887f6f4d7813f0a2cf1114f496f6b7eb33f18c4f` (version 1.0.1, “Package Simple Sketch 1.0.1 as an AppImage and an installable tarball.”). That is the commit the mission names. It was read from the tree that is checked out. The owner instructed implementation of the first slice on 2026-10-14, so that section is the map to build. The rest of this map, from “Rest of 0.1” on, is not approved and is not part of this slice.

**Scope:** Version 0.1 as the owner set on 2026-10-14, in `.project/documents/development/v0.1/tasks.md` (H-1 through H-5). The shell’s look is the chrome in `.project/.simple-sketch/INTERFACE.md`: window, tab strip and new-tab button, menu button, toolbar, and the properties panel that opens from a tab at its edge. The first slice is only that chrome, with an empty demo that declares nothing. Sketching-only tools, palettes, objects, and the grid stay out of that slice. The rest of this version carries Simple Sketch in as a sub-application. The assistant, any second creative app, and an app drawing its own view by sending paint commands are not mapped.

Destinations are paths in the Inpainter repository (https://github.com/Spumcake/Inpainter), which is not checked out. The future team-repo checkout is `src/inpainter/worktrees/dev/`.

The reference is one crate, `simple-sketch`, one program, `src/main.rs` calling `canvas::run`. The version’s boundary is a workspace, because the host must not be able to import an app and apps must not be able to import each other:

- `host/` — binary crate `inpainter`. The shell. It may depend on `draw` and `protocol`. It must not depend on anything under `apps/`.
- `draw/` — library crate `draw`. Drawing, erasing, shapes, text, and ordering. The host links it and calls it in-process, so pen input stays in the host.
- `apps/demo/` — binary crate `demo`. The empty sub-application for the first slice.
- `apps/simple-sketch/` — binary crate `simple-sketch`. How a sketch is arranged, how it is saved, and its undo.
- `protocol/` — library crate both programs may depend on. It is New. This map does not specify its messages.

A workspace `Cargo.toml` lists those members. No app crate appears in `host`’s dependencies or in another app’s.

## First slice — into the host

The host draws the areas. The demo fills none of them. Copy each file, then make only the edits named here. Strings, colors, sizes, and behavior of what is drawn stay as they are.

### Window

`src/canvas.rs` → `host/src/window.rs`: move and edit. Move these and nothing else from that file:

- `APP_ID` (`"io.github.spumcake.SimpleSketch"`), `window_icon`, `run`, and the `NativeOptions` in `run`: title `"Simple Sketch"`, `eframe::run_native` name `"Simple Sketch"`, inner size `[1280.0, 800.0]`, minimum `[640.0, 480.0]`, `persist_window: false`, `multisampling: 4`.
- `CANVAS` (`Color32::from_rgb(251, 251, 252)`) and `SketchApp::clear_color`, which returns that color.
- The theme setup in `SketchApp::with_tablet`: `options.sync_window_theme = false`, then `ctx.set_theme(egui::Theme::Light)`.
- From `SketchApp::ui`: light visuals with `panel_fill = Color32::WHITE`, `CentralPanel` with `Frame::NONE`, `item_spacing.y = 0.0`, then the tab bar, then the toolbar, then the rectangle under them. The panel, when open, takes `panel::WIDTH` off the right and the view rectangle narrows to match. The edge tab is drawn at the view’s right edge.
- `SketchApp::sync_title`: `"Simple Sketch"`, or `"{mark}{name} — Simple Sketch"` with `*` while the active tab needs keep.

Edit: this file no longer owns a `Session`, a tablet, or the sketch pointer. The view rectangle is filled with `CANVAS` and has no tools; it shows the shared grid below only when the selected tab's sub-application declares it. As the owner set on 2026-10-14, the three `"Simple Sketch"` strings become `"Inpainter"` and `APP_ID` becomes `"io.github.spumcake.Inpainter"`. The title is always `"Inpainter"`, whatever tab is selected, so `sync_title` does not move.

`src/canvas.rs` `install_panic_hook` does not move in this slice. It flushes a sketch recovery file in the same process. It is not a sub-application status.

Assets and dependencies the window needs:

- `assets/icon/128.png` → `host/assets/icon/128.png`: move. `window_icon` includes this file. The other icon files (`32.png`, `64.png`, `256.png`, `512.png`, `io.github.spumcake.SimpleSketch.svg`) move with it, unchanged, including the SVG filename.
- Host dependencies, copied from the reference `Cargo.toml`: `eframe = "=0.36.2"`. Edition `2024`, rust-version `1.95`. Not the sketch crates (`earcutr`, `i_overlay`, `kurbo`, `octotablet`, `perfect_freehand`, `rfd`, `serde`).

`src/canvas.rs` tests that cover this chrome move with the host, and only these: `the_title_bar_is_left_to_the_desktop_theme` (a frame must not send `ViewportCommand::SetTheme`), `the_panel_tab_toggles_the_panel_without_drawing`, and `f4_opens_the_panel_and_narrows_the_canvas`. Edit: they construct the host window, not `SketchApp`, and they do not assert `stroke_count`. The other tests in that module stay with the sketch view, below.

### Tab strip

`src/tabs.rs` → `host/src/tabs.rs`: move and edit. Move the drawing:

- `INACTIVE` (`243, 244, 247`), `TAB_EDGE` (`229, 231, 235`), `TAB_HEIGHT` (`34.0`).
- `show`: bar fill `toolbar::BAR`, inner margin left 8, right 8, top 6, bottom 0, item spacing x `4.0`, height `TAB_HEIGHT`. The hairline under the bar stops at the active tab (`toolbar::hairline`); with no active tab the hairline runs the full width, which is the branch already in `show`.
- `menu`: Iconoir `"menu"` at 18 points, button `vec2(32.0, TAB_HEIGHT)`, hover text `"Menu"`, opened with `egui::Popup::menu` and drawn by `widgets::menu_rows_with`.
- The new-tab control: Iconoir `"plus"` at 16 points, button `vec2(32.0, TAB_HEIGHT)`.
- `tab_row`, `Tab`, `tab_chip`, `paint_tab`, `close_control`, `paint_close_mark`. Chip constants: `PAD_LEFT` 14, `PAD_RIGHT` 8, `CLOSE` 20, `MIN_WIDTH` 120, `MAX_LABEL` 180, label 13.5 points in `ICON`, gap 10 before the close control, corner radius 8, outline stroke 1 in `TAB_EDGE`. Close glyph `"xmark"` at 11 points; the unsaved dot has radius 4. Scroll area id `"tabs"`, scroll bar hidden. The row’s width is `available_width - 36`, at least 48. Middle-click closes. Double-click does not edit the title.
- `finish_close`.

Edit, as the owner set on 2026-10-14: the menu's rows are the commands declared by the sub-application of the selected tab. Each tab is a document its sub-application reports, labeled with the name of the file that sub-application manages or, when it manages no file, with the sub-application's own name. The demo reports one tab, `Demo`. It declares no menu rows and no new-tab command, and it has only that tab, so the menu button, the plus, and the tab's close mark are drawn as they are now, as in `.project/renders/ui/shell-demo.png`, and a click on any of them does nothing: no popup, no new tab, no close. `SketchId` is not used.

Do not move, in this slice: `Chrome`’s confirm, quit, replace, recovery, and error fields, or `note_window_close`, `confirm`, `replace_dialog`, `recovery_dialog`, `error_dialog`, `recent_rows`, `RecentPick`. Those are the sketch’s save prompts. They are mapped below, when Simple Sketch is a sub-application.

### Menu rows, shared with the panel tab

`src/widgets.rs` → `host/src/widgets.rs`: move and edit. Move what the menu, the edge tab, and the scrolling areas use:

- Colors: `INK` (`40, 42, 46`), `TEXT` (`45, 47, 51`), `MUTED` (`107, 114, 128`), `RAIL` (`212, 214, 218`), `BORDER` (`226, 229, 233`), `BORDER_HOVER` (`196, 200, 206`), `SELECTED` (`236, 238, 241`), `DISABLED` (`186, 188, 192`).
- `SEMIBOLD` (`"Inter SemiBold"`), `Scale`, `Scale::PANEL` (`0.875`), `Scale::of`, `body`, `semibold`, `BODY` (`13.5`).
- `chevron`, `chevron_side`.
- `MenuItem`, `MenuPick`, `menu_rows`, `menu_rows_with`. Constants: `MENU_MIN_W` 148, `MENU_INSET` 10, `MENU_ICON` 18, `MENU_ICON_GAP` 8, `MENU_CHEVRON` 14, `MENU_CHEVRON_ICON` `"nav-arrow-right"`, row height 28, row spacing y 2, highlight radius 4, divider 5 points in `BORDER`.
- `drag_scroll_cursor`, `quiet`.

Leave in the reference for this slice, and move them into this same host file when Simple Sketch’s declarations are drawn: `field`, `slider`, `slider_in`, `toggle`, `number_field`, `text_field`, `dropdown`, `segmented`, `dash_sample`, `cap_sample`, `NumberEdit`, `action_menu`, and `KNOB_RADIUS` 7, `FIELD_RADIUS` 6, `FIELD_PAD` 10. They draw tool and property values, which the demo does not declare.

### Tool area

`src/toolbar.rs` → `host/src/toolbar.rs`: move and edit. Move the bar and the icon painting:

- `BAR` (`249, 250, 251`), `ICON` (`45, 47, 51`), `ICON_QUIET` (`186, 188, 192`), `ICON_ACTIVE` (`37, 99, 235`), `HAIRLINE` (`236, 237, 240`), `SEPARATOR` (`222, 225, 229`).
- `ROW` 40, `BUTTON_W` 36, `ICON_POINTS` 20, `SEPARATOR_W` 13. The separator line is 24 points tall (12 above and below its center), stroke 1.
- `show`’s frame: white fill, `Margin::symmetric(12, 5)`, height `ROW`, item spacing x 0. The trailing group is laid out right to left first. The leading groups sit in a horizontal `ScrollArea` id `"toolbar"`, scroll bar off, drag scroll always, mouse wheel on, `always_scroll_the_only_direction`. `widgets::drag_scroll_cursor` on that area. `hairline` along the bottom, 1 point, inset 0.5.
- `hairline`, `separator`, `glyph_button`, `icon_button`, `icon_text`, `paint_icon`, `icon_glyph`.
- `install_fonts`, `install_face`, `inter_weight`. Weights: roman 400 and 700, italic file 400 and 700, semibold 600. Proportional family starts with `"Inter"`. Iconoir faces come from `iconflow::fonts()`.
- Face-name strings, copied from `src/text.rs` because that file does not move in this slice: `INTER` `"Inter"`, `INTER_BOLD` `"Inter Bold"`, `INTER_ITALIC` `"Inter Italic"`, `INTER_BOLD_ITALIC` `"Inter Bold Italic"`.

Fonts: `assets/fonts/Inter-Variable.ttf`, `assets/fonts/Inter-Italic-Variable.ttf`, and `assets/fonts/OFL.txt` → `host/assets/fonts/`: move. `INTER` and `INTER_ITALIC_FONT` keep including those two font files.

Dependency: `iconflow = { version = "=2.1.0", default-features = false, features = ["pack-iconoir"] }`.

From `src/registry.rs`, moved into `host/src/icons.rs` because the command tables stay behind: `IconSpec`, `icon_spec`, `TURNED_ICONS` (`"flip-vertical"` is `"flip"` turned one quarter). `Key`, `Key::label`, `Chord`, `Chord::plain`, `Chord::ctrl`, `Chord::ctrl_shift`, `Chord::ctrl_any_shift`, `Chord::shift`, `Chord::label`, and `hover`. The panel shortcut moves with them: key `F4`, label `"F4"`, command label `"Properties"`, so the edge tab’s hover stays `"Properties (F4)"`.

Edit: `show` does not call `swatches`, `file_commands`, `history_commands`, `canvas_commands`, `tools`, `context_actions`, `tool_bar`, or `zoom`. Those groups are declared commands and tools. The demo declares none, so the white bar is empty, with its hairline. The pin-to-the-right layout and the scroll stay, so a later declaration still pins its trailing group and scrolls the rest. `paint_drawn_icon` and `teardrop` do not move in this slice; they draw the brush, which is a sketching tool. `DRAWN_ICONS` stays with them.

Leave in `src/toolbar.rs` for this slice: `SWATCH_W` 28, `SWATCH_INSET` 2, `SIZE_W` 186, `BAR_SLIDER_W` 150, `CHEVRON_W` 14, `CHEVRON_ICON` `"nav-arrow-down"`, `swatches`, `swatch_dot`, `swatch_color`, `mix_white`, `command_button`, `tools`, `context_actions`, `context_menu`, `tool_bar`, `list_button`, `bar_slider`, `BarEdit`, `size_control`, `zoom`, `paint_drawn_icon`, `teardrop`.

### Properties panel

`src/panel.rs` → `host/src/panel.rs`: move and edit. Move the column and the edge tab:

- `WIDTH` 266, `PAD` 16, `TAB` `vec2(22.0, 40.0)`, `TAB_TOP` 18.
- `show`: white fill, hairline at `rect.left() + 0.5` in `widgets::BORDER`, clip to the column, vertical `ScrollArea` id `"properties-panel"`, no scroll bar, drag scroll always, mouse wheel on, `widgets::drag_scroll_cursor`. If the width is at most `2.0 * PAD`, draw nothing inside.
- `toggle_tab`: foreground `Area` id `"panel-toggle"`, one point wider than `TAB` so it covers the hairline, corner radius 10 on the left only, fill white or `widgets::SELECTED` while hovered, stroke 1 in `BORDER`, seam covered while open, `chevron_side` pointing the way the panel will move. A click toggles. A click here does not fall through to the view.

The properties panel is a shared component of the host, as the owner set on 2026-10-14. The host draws it, always the same way, only for a tab whose sub-application declares it. The sub-application declares whether a tab has the panel, which side it sits on, and what it holds. Only the right side has a reference, so only the right side is built. The host draws `toggle_tab` and the column, and F4 toggles them, only when the selected tab's sub-application declares the panel. The panel starts closed, as `Session`’s `panel_open: false`. F4 toggles it, from the shortcut moved above. Opening it narrows the view by `WIDTH` on that frame, which `SketchApp::ui` already does.

Edit: `show` does not call `session.panel_sections`, `session.shows_layers`, `outliner::show`, or `back_row`. With nothing declared, the column is an empty scrolling page. `toggle_tab` toggles host state, not `Session`.

Leave in `src/panel.rs` for this slice: `S`, `ROW`, `ROW_GAP`, `LABEL_W`, `NUMBER_W`, `GAP`, `PRESET_SAMPLE_W`, `CURVE_H`, `TOP`, `HEADING`, `FIRST_LINE`, `PLOT` (`247, 248, 250`), `PLOT_GRID` (`232, 234, 238`), `ERASER_SAMPLE` (`160, 164, 170`), `back_row`, `draw_section`, `split_row`, `draw_row`, `number_row`, `preset_row`, `paint_stroke_sample`, `color_well`, `curve_editor`, and the curve helpers. They are property pages. They move into this same host file when declarations are drawn.

### Contents area: the shared grid

`src/canvas.rs` `paint_grid`, `screen_to_document`, and `document_to_screen`, with `GRID_SPACING` (`24.0`) from `src/document.rs` and the two lines of `Camera::centered` → `host/src/grid.rs`: move and edit. Stroke 1 in `Color32::from_rgb(234, 235, 238)`. As the owner set on 2026-10-14, the grid is a shared component of the host, drawn in a tab's contents area when that tab's sub-application declares it, so a later sub-application can use it too. Edit: it is drawn as the reference draws a new sketch, origin centered in the area and zoom 1, as `Camera::centered` sets it. Panning and zooming the grid are not part of this slice. `src/document.rs` keeps its own `GRID_SPACING` for snapping.

### First slice — into the demo

The demo is New. It is named with the New list. No reference file moves into `apps/demo/`. As the owner set on 2026-10-14, the demo reports one tab, `Demo`, because it manages no file, and declares the shared grid as that tab's contents and the properties panel on the right. It declares no tools, commands, menu rows, properties, or skills, so the toolbar and the panel stay empty, and nothing else happens.

## Rest of 0.1 — Simple Sketch inside the host

As the owner set on 2026-10-14, the sub-application owns the document. The host's copy in `draw` serves live edits and follows the sub-application's document; the protocol says how. Moving is the default. An edit is only the split the two programs require: the host calls `draw` in-process for a live edit, and the sub-application is the program that saves, recovers, and undoes. Look and wording stay. The protocol that connects those calls is New and is not specified here.

### Into `draw/`

`src/ink.rs` → `draw/src/ink.rs`: move. The whole module, including `Mesh`, `raw_outline`, `eraser_outline`, `stroke_visible`, `union_meshes`, `resolved_mesh`, and the boolean and curve helpers. Its tests move with it.

`src/text.rs` → `draw/src/text.rs`: move and edit. Layout, cache, and the tests move. Edit: the four face-name constants already copied into the host stay the same strings; this file uses those names and does not include the font bytes. The host has installed the faces.

`src/document.rs` → `draw/src/document.rs`: move and edit. This is the file that implements strokes, erasures, shapes, text objects, and ordering (`Sketch::reorder_selected`, `Sketch::reorder_sibling`, `Reorder`, `add_shape`, `commit_stroke_on_new_canvas`, `resolve_drag`, `shape_path`, `stroke_path`). It moves as a whole so those operations are not rewritten. Edit: `undo`, `redo`, `commit_step`, and `Step` stay in the file, and the simple-sketch program is the caller of undo, redo, and of `snapshot`. The host is the caller of the live-edit methods. Both programs link `draw`. Neither imports the other.

`src/document/snapshot.rs` → `draw/src/document/snapshot.rs`: move. It is the `.smx` value of a `Sketch`. The simple-sketch program calls it when it saves. The wording `"It is not a Simple Sketch file, or it is damaged."` in `store.rs` stays.

`src/defaults.rs` → `draw/src/defaults.rs`: move. Built-in palettes, sizes, and preset names. Callers pass them in; they are not re-derived.

### Into the host, when the sketch view is shown

These run in the host so the pen never crosses a process. They call `draw`.

`src/tablet.rs` → `host/src/tablet.rs`: move. `PenEvent`, `PenSample`, `PenInput`, `Tablet`, `IDLE_POLL` (100 ms), and its tests. `vendor/octotablet/` → `host/vendor/octotablet/`: move, including the version cap in `src/platform/wl/mod.rs` and `PATCHED.md`. The host `Cargo.toml` copies the reference dependency and the `[patch.crates-io]` path.

`src/canvas.rs` remainder → `host/src/view.rs`: move and edit. Move pointer mapping, `paint_mark`, selection handles, the text caret, the camera, and the clipboard key path (`clipboard_frame`, `take_clipboard`, `chord_pressed`, `egui_key`, `smx_from_args`). Constants: `NOTCH_POINTS` 50, `HANDLE_SIDE` 9, `HANDLE_REACH` 8, `OUTLINE_OUTSET` 6, `ROTATE_STEM` 18, `ROTATE_DOT` 4. Edit: a sample, a shape drag, a text edit, and a reorder call `draw` in this process. A command that saves, opens, closes, undoes, or redoes is asked of the simple-sketch program. Simple Sketch declares the shared grid for its tabs, and the grid pans and zooms with the sketch's camera.

`src/toolbar.rs` remainder → `host/src/toolbar.rs`: move and edit. The functions left out of the first slice (`swatches` through `zoom`, the brush `paint_drawn_icon` and `teardrop`, `DRAWN_ICONS`). Edit: each group is drawn from the sketch app’s declarations, in the reference order, with the same sizes. `SWATCH_W` 28, `SWATCH_INSET` 2, active ring 28.5, `SIZE_W` 186, `BAR_SLIDER_W` 150.

`src/panel.rs` remainder → `host/src/panel.rs`: move and edit. Section grid and property widgets. `ROW` is `Scale::PANEL` of 30, `ROW_GAP` of 8, `LABEL_W` of 92, `NUMBER_W` of 56, `GAP` of 10, `PRESET_SAMPLE_W` of 84, `CURVE_H` of 140, `TOP` of 22, `HEADING` of 20, `FIRST_LINE` as now. Edit: section titles and values come from declarations. The widgets stay the ones in `widgets.rs`.

`src/widgets.rs` remainder → `host/src/widgets.rs`: move. The controls left out of the first slice.

`src/outliner.rs` → `host/src/outliner.rs`: move and edit. `S` is `Scale(1.25)`, `ROW` is `S.of(28.0)`. Edit: reorder drags call `draw`’s ordering. Row contents come from the sketch.

`src/tabs.rs` dialogs left out of the first slice → `host/src/tabs.rs`: move and edit. `confirm` (“Keep this sketch?”), `replace_dialog` (“Replace this file?”), `recovery_dialog` (“Recovery”), `error_dialog`, `note_window_close`, `recent_rows`. Wording, widths (min 260, error max 360), and button labels stay. Edit: Save, Discard, Restore, and the file path are asked of the simple-sketch program, which owns save and recovery. The host draws the dialogs.

`src/dialogs.rs` → `host/src/dialogs.rs`: move and edit. `pick_open` and `pick_save` stay `rfd` dialogs on the host window, filter `"Sketch"` / `["smx"]`. Edit: the host opens one when the simple-sketch program asks, and returns the path to that program. Dependency `rfd = "=0.15.4"` moves to the host.

`src/prefs.rs` → `host/src/prefs.rs`: move and edit. Tool settings belong to the window. `FILE_NAME` `"preferences.json"` and the JSON keys stay. Edit: as the owner set on 2026-10-14, `data_dir` uses `ProjectDirs::from("com.spumcake", "Spumcake", "Inpainter")`, and recovery files for this product use that same folder. The host does not read or write Simple Sketch's folder. Dependency `directories = "=6.0.0"` moves to the host. `serde` and `serde_json` (the same versions as the reference) move to `draw` for the snapshot and to the host for `prefs.rs`, which parses the preferences file itself.

`src/session.rs` live-edit and window methods → `host/src/session.rs`: move and edit. Move `begin`, `add_sample`, `commit`, `cancel`, the shape and text gesture methods, `reorder_node`, transform preview and commit, `paints`, `scene`, `pan_screen`, `zoom_toward`, `zoom_at_center`, `fit`, `hit_test`, selection, clipboard (`Clipboard`, `PASTE_STEP` 16), `panel_open` / `toggle_panel`, and `ToolSnapshot`. Edit: ink, shape, text, and ordering call `draw`. File, undo, and recovery commands are not implemented here; `perform` for those ids asks the simple-sketch program.

The canvas tests in `src/canvas.rs` other than the three named above move with `host/src/view.rs`.

### Into the Simple Sketch sub-application

`src/main.rs` → `apps/simple-sketch/src/main.rs`: move and edit. Edit: it does not call `canvas::run` and it does not open a window. The host is the only window. What this program does instead is the protocol, which is New and not specified here.

`src/registry.rs` → `apps/simple-sketch/src/registry.rs`: move and edit. Tools, commands, menus, chords, and labels stay, including `sketch_menu`, `RECENT_MENU_LABEL` `"Open Recent"`, `file_commands`, `view_commands`, `select_actions`, and the icon names (`"page"`, `"folder"`, `"floppy-disk"`, `"undo"`, `"brush"` via `DRAWN_ICONS`, and the rest of `commands()`). Edit: `Key`, `Chord`, `hover`, and `icon_spec` already moved to the host; this file keeps the tables of tools and commands and does not paint. Those tables are what this app declares. The host draws them with the toolbar, menu, and panel already moved.

`src/schema.rs` → `apps/simple-sketch/src/schema.rs`: move. `PropId`, `PropKind`, `SectionId`, the section lists (`BRUSH_SECTIONS`, `ERASER_SECTIONS`, `TEXT_SECTIONS`, and the rest), and `prop` / `section`. They are this app’s property declarations. The host draws a `PropKind` with the widgets already moved. Tests move with the file.

`src/session.rs` save, undo, and sketch lifetime → `apps/simple-sketch/src/session.rs`: move and edit. Move `new_sketch`, `activate`, `request_close`, `discard`, `save_path`, `open_path`, `open_initial`, `undo`, `redo`, recovery (`RECOVERY_INTERVAL` of 10 minutes, `poll_recovery`, `publish_emergency`), recent files, and `perform` for the file and history commands. Edit: a live edit is not applied here. Undo and redo call `Sketch::undo` and `Sketch::redo` in `draw` on the sketch this program keeps. The tests of those commands move with them.

`src/store.rs` → `apps/simple-sketch/src/store.rs`: move. `.smx` read and write, atomic replace, recovery files, `reason`, and `flush_emergency`. The panic hook’s emergency flush is this program’s, because a crash of this process is not a crash of the host.

`src/lib.rs` → each crate’s own `lib.rs`: move and edit. The reference module list is split across `host`, `draw`, and `apps/simple-sketch` as the files above. No crate re-exports another program.

## Left in the reference

Left for the first slice, and carried later by the lists above: brush, eraser, select, rectangle, ellipse, line, and text tools; palettes and swatches; canvases, shapes, text objects, and groups.

Not this version, so not mapped:

- The assistant, and any second creative app.
- A sub-application drawing its own view by running egui and sending paint commands. The reference has no such path. `std::process` appears only as `process::id` in test temp directories.
- `packaging/linux/` (desktop file, metainfo, AppImage scripts) and `release-notes/v1.0.1.md`. They install Simple Sketch as its own program. The person launches the host. They are not rewritten into a host launcher.
- `README.md`, `LICENSE`, and `Cargo.lock` of the reference.

## New, awaiting the owner

- The host/sub-application protocol, as a crate both sides can depend on. It is specified in systems and brought to the owner before the first slice is built. The reference is one process: `registry.rs` and `schema.rs` are in-memory tables the window reads directly, and `main` opens the window. There is no declaration passed between programs, and no way to drive the app with no window. H-5 needs that. This map does not define the messages, the ids, or the transport.
- `apps/demo/`, a separate program that reports one tab, `Demo`, and declares only the shared grid and the properties panel. The reference has only Simple Sketch.
- The host’s report when a sub-application stops or stops responding, drawn in the window the host already has. The reference has no child process. `install_panic_hook` writes a recovery file and then the window, which is the same process, comes down with the panic. The file-error dialog is a failed save or open, not a stopped program.
- The workspace itself: root `Cargo.toml` and the crate boundaries above. The reference is a single package.
- A host-side test that the empty demo’s declarations are empty and that the areas are still drawn. The reference’s tests construct `SketchApp`. They do not cover a second program. This waits until the owner asks to commit tests, as the delivery document says.
- One scripted test that starts the simple-sketch program with no host and drives one declared command. That is H-5. The reference has no such harness.
