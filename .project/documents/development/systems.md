# Simple Sketch — systems

**Inputs and baseline:** The coordinator accepts [INTERFACE.md](../../../INTERFACE.md) as first-version behavior. Product intent is in [PITCH.md](../PITCH.md). The renders are in `.project/renders/ui/`, listed in INTERFACE.

Current fact: the app repository `vector-sketch` (https://github.com/Spumcake/vector-sketch) has local `dev` at `7920c85` (`Add the pictured shell and Wayland pen pressure.`) in `src/vector-sketch/worktrees/dev`. `origin/dev` is also `7920c85`. The crate has a flat mark list, a session with tabs, the shell, the Wayland pen path, and 19 tests. The object model below is not built. The team repository's remote is https://github.com/Spumcake/vs-team.

## Boundaries and ownership

One native process. `eframe` owns the window and the egui frame. The **session** owns all document and window state. Widgets send session commands and hold no second copy.

The **session** (the window) owns the open sketches in tab order, the active sketch, the current tool, the properties panel's open state and subject, every tool's settings, and the in-app clipboard. A tool's settings include its palette, and the brush's settings include its active color. Tool settings are loaded from and saved to the preferences store.

Each **sketch** owns its identity, display name, object tree, selection, camera, undo stack, and redo stack. Those are not shared across tabs. A canvas does not own an active color; the brush does.

**Registries and the property schema** are static data. They describe every tool, command, property, and panel section once. The toolbar, shortcuts, hover text, menus, context menus, panel, and outliner are built from them. Adding a tool or a property means adding a registry or schema entry, not a widget.

Derived ink is never stored. That covers outlines, layer unions, tessellation, bounds, and text layout. It is cached and rebuilt when its inputs change.

Dependency direction: `main` → canvas, chrome, panel, and outliner → session → document → `perfect_freehand`, `i_overlay`, `earcutr`, and `kurbo`. The canvas also uses `tablet` → `octotablet`. The session and document do not depend on egui. Text layout is the exception: it is measured through egui at paint time, and the document stores only text and style.

Accepted stack: a native Rust binary, egui via eframe, and Iconoir icons through `iconflow`. Electron, Tauri, a web target, and a fork of another drawing application are out. No accounts, sync, or collaboration.

## Source map

Built today in `src/vector-sketch/worktrees/dev`: `lib.rs`, `main.rs`, `session.rs` (tabs, tool, gestures), `sketch.rs` (marks, transforms, erase attachments, camera, history, derived-ink cache), `canvas.rs` (pointer mapping and painting, with egui-frame tests), `tablet.rs`, `tabs.rs`, `toolbar.rs`, and `vendor/octotablet`.

Planned, replacing the mark list rather than adding a second model:

- `document.rs` — the object tree, canvases and palettes, strokes, shapes, text, groups, transforms, and the history steps.
- `ink.rs` — outlines, erase differences, layer unions, shape strokes and fills, tessellation, and the caches. Most of today's derived-ink code in `sketch.rs` moves here.
- `schema.rs` — property ids, descriptors, sections, and the `Inspect` implementations of each style component.
- `registry.rs` — tool and command tables, and shortcut dispatch.
- `defaults.rs` — built-in data: each tool's starting palette, presets, the font list, and tool defaults.
- `panel.rs` — the generic properties panel.
- `widgets.rs` — the controls the toolbar and panel share: slider, switch, number and text fields, dropdown, and segmented choice, in one palette. Panel controls take `Scale::PANEL` (0.875), and the panel scales its own spacing by it. The outliner's rows are drawn at a quarter above full size, with 15-point names. The slider is one size everywhere: the toolbar's Size slider is drawn at the panel's scale, and its rail is snapped to whole device pixels so every slider's line is the same thickness. `menu_rows` draws the rows of every menu at full size: the sketch menu, the select tool's action menus, a section's menu, and the outliner's row menus. A menu is as wide as its longest label plus equal insets, with a minimum width.
- `outliner.rs` — the Layers tree.
- `prefs.rs` and `store.rs` — the preferences file, `.smx` sketches, and recovery.

## Data-driven core

**Properties.** A `PropId` names every editable value. A `PropDesc` gives its label, its kind, and its default. The kinds are toggle; number (with min, max, step, unit, and a display format such as a percentage); color; choice (labels and optional icons, drawn as a dropdown or segmented buttons); curve; font family; text; and palette (the owner's colors as rows). A `SectionDesc` is a titled list of `PropId`s. Sections are shared: Palette appears for every tool with a palette and for shape and text objects, and Behavior for the brush and eraser. Each style component implements `Inspect` once: it lists its properties, gets a value, and sets a value. Setting validates and clamps there, and nowhere else.

**Right-click menus** are registry tables of `MenuActionDef` (action and label), drawn by one shared `action_menu`: a tool button has Properties; a brush swatch has Duplicate, Delete, and Properties; a shape's or text's swatch has Properties; an object with the select tool, and a shape's or text's row, has Properties; the canvas row has Rename canvas, Make palette default, and Properties; a group row has Rename group and Properties; a layer row has Rename, Duplicate, Delete, and Properties.

**Tools.** A `ToolDef` gives the id, label, icon, shortcut, panel sections, the property the Size control binds to, its toolbar controls, and what it creates. The toolbar controls are a table of `BarControl`: Size, a divider, a labelled slider, an icon choice over an index setting, and the list button. Each reads and writes the current tool's setting through the same `Inspect` as its panel page, and the toolbar draws them with no per-tool code. With the text tool, the session's active text, the text being edited or the one selected text, takes those reads and writes instead, through the node's `Inspect`, recorded as set-properties steps; new text being typed has no active text.

**Commands.** A `CommandDef` gives the id, label, icon, shortcut, an enabled rule, and the session command it runs. The commands are new sketch, open, save, save as, save a copy, clear recent, rename, close, undo, redo, cut, copy, paste, delete, nudges, zoom in, zoom out, fit, zoom to selection, maximize, new canvas, group, ungroup, bring forward, send backward, bring to front, send to back, the three aligns, the three rotations, the two flips, and the panel toggle. Save As is Ctrl+Shift+S. No shortcut is assigned for Save a Copy or the recovery offer. Open, Save, Save As, and Save a Copy are always enabled. The toolbar's file commands stay new sketch, open, and save. The sketch menu is a registry table of rows: commands, and the Open Recent submenu after Open. The shared menu rows draw a submenu row with a chevron and a divider above a row that asks for one. The recent list checks a file again only when its size or modification time changes.

**Panel.** The panel renderer takes a subject, resolves it to sections, and draws each property by its kind. It has no per-tool code. With several objects selected, it shows the sections they all have, and a property whose values differ reads as mixed.

**Built-in data** lives in `defaults.rs` as tables: each tool's starting palette, the presets, the font families, and every tool default. Preferences override tool settings, including tool palettes.

Tests check the tables themselves:
- no two commands or tools share a shortcut;
- every `PropId` in a section is readable through the `Inspect` of each subject that lists that section;
- every icon name resolves in Iconoir or is listed in `DRAWN_ICONS`. The brush icon is drawn by the toolbar, because Iconoir has none. A name ending in `-solid` is the icon's filled style, as Iconoir's site names it, and a name in `TURNED_ICONS` is another icon turned by quarter turns: Flip vertical is Iconoir's flip turned once.
- the select tool's context actions are one undivided list, `select_actions`, of buttons and menus; every arranging command is in it, and zoom to selection is not but has a shortcut.

## Data and contracts

### Camera

Coordinates are `f64`, y down, in document space. A document point `p` appears at screen `(p - camera.origin) * camera.zoom` in egui points. `camera.origin` is the document point at the viewport's top-left. Zoom is finite, from `0.125` through `8`, default `1`. Zoom in and zoom out multiply by `5/4` or `4/5`, clamp, and keep the viewport center fixed. Ctrl or Cmd with scroll uses the same factor and keeps the point under the pointer fixed. Scroll pans vertically, and Shift-scroll pans horizontally. Space-drag and middle-drag pan. None of these change the tool or push history.

Fit frames the union of world bounds of visible objects with a 24-unit margin, or the nearest zoom limit centered on those bounds. With nothing visible, fit sets zoom `1` with the origin at the viewport center. Zoom to selection does the same for the selection's bounds. The grid is a guide with 24-unit spacing; marks are not snapped to it unless a shape tool's Snap to grid is on.

### Identity

`SketchId`, `NodeId`, `ColorId`, and `StrokeId` are non-zero `u64` values. Each is unique in the session and never reused, including across save and open. A new session has one sketch, Untitled 1. New tab creates Untitled N, using the smallest positive integer not used by an open tab. A new sketch contains one canvas, Canvas 1, highlighted, with a copy of the brush's palette. Tool palette colors are stored in preferences with their `ColorId`s, and the session's allocator starts above every id it has loaded, including every id in an opened snapshot, every id in a recovery file's name, and every id in a leftover `sketch-<id>.json` name. When a snapshot just opened or restored carries a sketch, node, palette-color, or stroke id the session already holds, that sketch receives a new id in its place, so a copy can be open beside its original. A layer's `source` stays as stored; it refers to a brush color. New canvas uses Canvas N, the smallest positive integer not used by a canvas in that sketch. Group N follows the same rule. Names may repeat.

### Object tree

A sketch's objects form a tree under an implicit root. Children are ordered top to bottom, and earlier children paint above later ones. Every node has `id`, `name`, `visible`, `locked`, a 2D affine `transform` (a `kurbo::Affine`), and a kind:

- **Canvas** — `padding` (0..=128, default 16, not edited in the UI) and `palette`, an ordered list of at least one color layer.
- **Rectangle** — local `min` and `max`, `corner_radius` (0..=64), and a palette of Outline and Fill.
- **Ellipse** — local `min` and `max`, and a palette of Outline and Fill.
- **Line** — local `start` and `end`, and a palette of Stroke.
- **Text** — `content` (may contain line breaks), `box_width` (`None` means auto-size), `box_height` (`None` fits the lines; a set height is never drawn shorter than the lines, which sit in it by `style.valign`: top, middle, or bottom), `style`, and a palette of Text, Background, and Border. Padding, radius, and the Border's width are drawn at the text's glyph scale, the square root of its transform's determinant.
- **Group** — `children`.

A node's world transform is its ancestors' transforms composed with its own. A node is drawn and hit only if it and all its ancestors are visible. It is picked on the canvas only if it and its ancestors are unlocked. Geometry is never rewritten by move, scale, rotate, flip, or align; those change the transform. Ungroup composes the group's transform into each child, so world positions stay.

### Palettes, canvases, and strokes

A **palette color** has `id`, `name`, `rgb`, `opacity` (0..=1), `size` (1..=32), `dash` (solid or dashed), `cap` (round or flat), and `visible`. Every palette uses this one type, and the color page edits it through one `Inspect`. A role without a stroke (Fill, Background, Text) uses only `rgb`, `opacity`, and `visible`; the page hides the rest. `cap` applies to brush colors, canvas layers, and a line's Stroke.

**Palettes by owner:**
- Brush tool: an ordered list of at least one color, starting with the nine in `defaults.rs`. Add, duplicate, delete, and move are tool-setting changes.
- Rectangle and ellipse tools and objects: `[Outline, Fill]`. Line: `[Stroke]`. Text: `[Text, Background, Border]`. These roles are fixed: no add, delete, move, or rename. `visible` on Fill, Background, or Border is whether it is drawn. Border has `size` and `dash` but no cap, and its tool color has no `lock_size`, as the text tool's Size slider is the font's. Stored text tool preferences with `[Text, Background]` gain a Border; any other mismatch takes the starting palette.
- Canvas: its layers, below.

A tool palette color also has `lock_size` (default on) and `next_size` (1..=32, the slider's value while unlocked). These are tool settings. They exist on brush colors and on a shape tool's Outline or Stroke.

A new object copies its tool's palette with new `ColorId`s. A canvas copies the brush's palette; a rectangle, ellipse, line, or text object copies its tool's palette. The copy is the object's own from then on.

A **color layer** is a canvas palette color plus `locked`, `source` (the brush `ColorId` it was copied from, or `None`), and `strokes`, oldest first. Index 0 is the top layer.

The brush's **active color** is a brush `ColorId`, a tool setting. A brush stroke goes into the highlighted canvas's layer whose `source` is the active color. If none has it, the commit is a batch that inserts a layer copied from that brush color at the bottom of the canvas's palette, with `source` set, and then adds the stroke. Make palette default replaces the brush's palette with copies of the canvas's layers, under new brush ids, and sets each of that canvas's layers' `source` to its copy. A duplicated layer has `source` `None`.

A **stroke** has `id`, `samples`, the `dynamics` it was drawn with, `size` (`None` follows its layer's size; otherwise fixed, 1..=32), and `erases`. A stroke drawn while its brush color's `lock_size` is on stores `None`; otherwise it stores the slider's `next_size`. Samples and erase paths are in canvas-local space. A stroke has no transform of its own and no color or style: it takes those from its layer.

A **sample** is `{ x, y, pressure }`, where `pressure` is `None` or finite in `0.0..=1.0`. Samples are kept exactly as the device reported them.

**Dynamics** are:
- `pressure_size` (bool);
- `curve`: 2 to 5 points, with x strictly increasing from 0 to 1 and y non-decreasing within 0..=1, joined by monotone cubic interpolation;
- `smoothing`, `stabilization`, and `streamline`, each 0..=100.

The brush and the eraser each have their own dynamics in the session. A stroke stores a copy of the brush's dynamics at commit.

**Brush outline.** A stroke's size is its own `size`, or its layer's when that is `None`. Effective pressure is `1.0` when pressure is `None` or `pressure_size` is off, and `curve(pressure)` otherwise. The diameter at a sample is the stroke's size times the effective pressure. That is `perfect_freehand` with thinning 1 and a library size of half the stroke's size, as today. `smoothing / 100` and `streamline / 100` go to the library. Stabilization replaces each sample position with the mean of the last `n` positions before outlining, where `n = round(stabilization / 100 × 16)`; 0 or 1 means off. Pressures and the stored samples are unchanged. A flat cap sets the library's start and end caps off; round sets them on. Dashed splits the stabilized centerline by arc length into dashes of 3× size separated by gaps of 2× size, and outlines each dash. A layer's dash or cap change redraws its strokes with the same samples. A layer's size change redraws the strokes whose `size` is `None`.

**Layer ink.** A stroke's visible ink is its outline minus the union of its erase outlines (`i_overlay`, non-zero). A layer's ink is the union of its strokes' visible ink, tessellated by `earcutr`, and painted once with the layer's rgb and opacity. Overlaps within a layer therefore do not darken. A canvas paints its visible layers from last to first under its world transform. A failed difference, union, or tessellation draws nothing for that stroke or layer that frame and keeps the data.

**Bounding box.** Each layer's derived mesh keeps the convex hull of its visible ink. A canvas's world box is the bounds of those hull points after the canvas's transform, so a rotation does not inflate it, expanded by `padding` along each world axis scaled by the transform's stretch on that axis. Selection, hit testing, and fit use this box. With no visible ink it has no box and is not hit on the canvas.

**Brush and eraser.** Every tool can always be chosen; no selection change switches tools. Each sketch keeps an active canvas: selecting exactly one canvas, adding one, or a stroke that makes one sets it, and other selections leave it. The highlighted canvas is the active canvas while it and its ancestors are visible and unlocked. A committed stroke or erase selects its canvas; a new shape or text is selected; choosing the brush or eraser selects the highlighted canvas. With one, the brush draws into its layer whose `source` is the active brush color, and draws nothing while that layer is hidden or locked. With none, a brush commit is a batch that inserts a new canvas copied from the brush palette at the root top, named Canvas N, and adds the stroke to its layer for the active color, then highlights that canvas. With none, the eraser draws and records nothing. A brush commit with fewer than two samples writes no history.

### Eraser

The eraser has `size` (1..=32, default 8) and its own dynamics (`pressure_size` default on). Its outline uses the same rules as the brush, with its own size and dynamics. Accepted method: polygon difference, not centerline distance.

On release it records one step. The step adds a copy of its path, in canvas-local space, to every stroke on the highlighted canvas whose layer is visible and unlocked and whose current visible ink intersects the eraser outline. Strokes added later are untouched. A path with fewer than two samples, or one that crosses nothing, writes no history. Shapes and text are never erased. A stylus eraser end selects the eraser until lift and then restores the previous tool.

### Shapes

A rectangle or ellipse's Outline color gives the stroke's `rgb`, `opacity`, width (its `size`), and `dash`, and its Fill gives the fill's `rgb` and `opacity` when `visible`. A line's Stroke color gives the same, plus `cap`. A new shape copies its tool's palette; the Outline or Stroke `size` is the tool color's `size` while `lock_size` is on, or its `next_size` otherwise.

Paths are built with `kurbo`: a rounded rectangle, an ellipse, or a line. The stroke is expanded with `kurbo`'s stroker, using miter joins for sharp rectangle corners, the Stroke color's cap for lines, and a dash pattern of 3× and 2× the width when dashed. It is flattened to 0.05 local units, resolved with `i_overlay`, and tessellated. The fill is the flattened closed path, tessellated. A shape whose Fill is visible is hit inside its fill or on its stroke ink. Otherwise it is hit only on its stroke ink.

The shape tools' Constrain proportions and Snap to grid are tool settings only. They are not stored on objects. Shift inverts Constrain proportions during a drag.

### Text

`TextStyle` holds:
- `family`, from the shipped list;
- `size` (8..=144, default 24, document units per em);
- `bold`, `italic`, `underline`;
- `align` (left, center, right, or justify);
- `list` (none, bullets, numbers, or checks) and `list_kind`, the kind the list button turns back on;
- `padding` and `radius` (0..=48 units each), the box's inner space and corner radius;
- `line_height` (0.8..=3.0, default 1.2);
- `letter_spacing` (−5..=20 units, default 0);
- `all_caps`, `wrap`.

The glyph color and opacity come from the text's Text color, and the box fill from its Background color when `visible`. Text is laid out with an egui `LayoutJob` at paint time, cached by content, style, palette, width, and zoom. It is drawn as a text shape rotated by the world transform's rotation. Its glyphs are scaled by the mean of the transform's horizontal and vertical scale, the square root of its determinant's size, so an uneven scale, such as a corner drag without Shift, scales them between the two and never snaps them back to full size. Lines wrap at the box's drawn width, and the box is measured back through the horizontal and vertical scales, so it always covers the glyphs. Flips keep glyphs upright and unmirrored. The in-place editor uses the same scale as the canvas. Text is hit inside its box. The shipped families are Inter, in regular, bold, italic, and bold italic, and egui's monospace face. Text is never erased.

### Selection and transforms

Selection is a set of `NodeId`s per sketch. A canvas click picks the topmost visible, unlocked root-level node whose ink, fill, or text box is under the pointer. The outliner can pick a node at any depth. The selection box is the union of selected nodes' world bounds. Corner drags scale about the opposite corner, and Shift keeps the aspect. Edge-midpoint drags scale one axis about the opposite edge's midpoint, and Shift applies that scale to both axes. The rotation handle rotates about the box center, and Shift snaps to 15 degrees. A selection holding a text object, directly or inside a group, has no rotation handle, rotate, or flip; the select bar leaves those commands out rather than drawing them disabled. Nudges are 1 unit, or 8 with Shift. Align left, center, and right move each selected node horizontally to the selection's left edge, center, or right edge. Rotate applies 90 degrees clockwise, 90 degrees counterclockwise, or 180 degrees about the box center. Flip scales by −1 on x or y about the box center. Group inserts a new group at the topmost selected node's position, with the selected nodes in their current order. Selection changes are not undo steps.

### History

One undo stack and one redo stack per sketch. Every entry is one of a small set of reversible steps, alone or in a batch:

- insert subtrees at a parent and index, and its inverse, remove;
- move nodes between parents or indexes;
- set transforms, with before and after values;
- set properties on a node or on one of its palette colors, with before and after `PropValue`s per `PropId`;
- add a stroke to a layer, and its inverse;
- add erase paths to strokes, and its inverse;
- insert a color layer, and its inverse, remove;
- move a color layer within a palette;
- replace a text object's content;
- set a text object's box, its local origin, width, and height, from an edge handle.

Paste, cut, delete, group, ungroup, align, and multi-object edits are batches. Renames, visibility, and lock are property steps. A new step clears redo. Undo and redo of an empty stack change nothing and return an empty-history error. Camera, selection, the brush's active color, tool settings and tool palettes, and the panel are not history. A finished slider drag is one step.

### Panel subject

The panel subject is one of: a tool, by `ToolId`; a palette color, by its owner (a tool, or a node) and `ColorId`; Layers, the outliner alone; or Objects, the selection's own sections. An object's Properties sets Objects, selecting that object first unless it is already selected; a right-click inside the selection's box keeps the selection; a group has no sections, so its Properties waits. Objects with nothing to show falls back to the current tool's subject. Changing the current tool sets the subject to that tool, and to Layers for select. A tool menu's Properties sets that tool as the subject, opens the panel, and leaves the current tool. A swatch menu's Properties, or a click on a shape or text swatch, sets that tool color. Clicking a layer row in the outliner, or its menu's Properties, sets that layer. Editing a tool subject or a tool's color changes session tool settings and saves preferences. Editing a node's color, or a selected object under Layers, records a set-properties step.

A selected shape or text object lists a Palette section first, and a selected canvas lists its layers as one. The section is drawn by the outliner's own palette rows, with the capabilities each color's role allows. A color page remembers the page it was opened from and shows a back row to it. It is a schema section of kind palette: rows of the object's colors, each opening that color. A tool's page has none, as a tool's colors are its swatches. The panel drops any row its subject cannot read and any section left empty, so one color page serves every role. The color page has no menu. While the text tool has an active text, the text tool's subject resolves to Objects for that text, so its settings change it. A double-click on text with the select tool starts the same edit. An edit ends on Escape, a tool change, or a primary press on the canvas outside the caret and the text tool's side handles; a press on the toolbar, the panel, or an open menu leaves it open, so the panel's page does not switch under that press. New text being typed is a session draft with an origin, an optional box width, and its words; it reads the text tool's style and palette live, and the frame lays it out and draws its Background under the caret. The text tool's side handles, always shown, set the box width of the draft without history, or of a text object as one set-box step.

The color subject's sections follow `color-property-ui.png` only: Color (well, name, hex, visible), Stroke (size, opacity, and `lock_size` for a tool color that has it), and Stroke Style (dash, cap), with the rows a role does not use left out. It has no RGB, HSL, or HSV sliders, related shades, usage toggles, join, or layer controls. Duplicate and delete run from a brush swatch's or a layer row's menu. Deleting a layer removes it and its strokes as one step. Delete is disabled on a palette's last color. A brush swatch dragged along the toolbar past another swatch moves that brush color on release, as a tool setting. A press that does not become a drag is a click.

**Size control.** With the brush, the slider shows the active brush color's `size` while its `lock_size` is on, drawn in that color, and its `next_size` otherwise, drawn near-black. Moving it sets `next_size` and turns `lock_size` off for that color. With a shape tool it does the same for the tool's Outline or Stroke color. With the eraser it sets the eraser size, and with text the tool's font size. It never edits the sketch.

### Clipboard, tabs, and closing

The clipboard holds deep copies of whole subtrees. A copied canvas brings its palette, strokes, and erases. Paste inserts copies at the root top with new ids, offset by `(16, 16)` from the source, then another `(16, 16)` on each repeat, and selects them. Copy waits with nothing selected.

A clean sketch has only its starting canvas, with no strokes, its palette as copied, and the generated name, and has never been saved. Closing the only sketch when it is clean does nothing. Closing any other last sketch ends with a new sketch. A sketch with changes since its last save, or with an edited name, asks once: Save, Discard, or Cancel, with the behavior in INTERFACE. Save in that question writes the sketch's file, or chooses a path when the sketch has none, and closes the sketch only after the write succeeds. Quitting asks once per such sketch, in tab order. A completed rename dirties the sketch and is not an undo step.

### Preferences and storage

The preferences store is one versioned JSON file (`format_version` 1) in the platform user-data directory for Simple Sketch. A file from before tool palettes still loads: its `palette` becomes the brush's, and a shape's or text's old color fields fill that tool's palette. It holds the tool settings, including each tool's palette with its `ColorId`s, `lock_size`, and `next_size`, and the brush's active color, and is written whenever they change. That write keeps `recent`. `recent` is the last 10 sketch paths, most recent first. A path is recorded when a sketch is successfully opened or written there, including Save a Copy, and a path already listed moves to the front. The sketch menu lists those paths in that order. A missing or unreadable preferences file means built-in defaults and an empty recent list, and it is never treated as an error that stops the app. A recent path whose file is missing or unreadable is left out of that list and does not stop the app.

A `.smx` file is the snapshot JSON (`format_version` 1): the id, saved name, camera, and the full object tree with every node's fields, palettes with their layers' `source` ids, strokes, stroke sizes, samples, dynamics, and erases. It does not hold history, selection, the clipboard, tool settings, the panel, or the path. The path the person chose is the sketch's file, kept by the session. The first Save and Save As ask for a path. After that write succeeds, the path is the sketch's file and the dirty flag clears. A later Save overwrites that path and clears the dirty flag. Save a Copy writes another path and leaves the sketch's file and dirty flag as they were. Cancelling the dialog leaves the sketch as it was. A write goes to a temporary file beside the path, which is flushed to the disk and renamed over it, then the folder is flushed. A failed write leaves the previous file as it was, leaves the sketch dirty, and the session keeps a file error, the file's name and a sentence of why, that the window shows once. A failed open keeps one too. After a save dialog returns a path that already exists and is not this sketch's file, the window asks once before writing; refusing leaves the sketch unchanged. A chosen name that does not end in `.smx` gets that suffix. The open and save dialogs filter to `.smx`. Open, Save As, and Save a Copy start in the last folder a file dialog in this window returned, and otherwise in the home directory. A dialog that returns a path updates that folder. The window remembers it until it closes. The suggested name is the sketch's name with `.smx`. When the home directory cannot be resolved, the dialog opens without a start folder.

Opening a path already held by a tab focuses that tab and does not load a second copy. The session compares the absolute path it stored. Otherwise the snapshot opens as its own tab, with the dirty flag clear and with no selection. An unreadable `.smx` does not stop the app. A `.smx` path given on the command line opens the same way. The frame still takes pointer buttons from the window. The native dialog does not take that path.

Every 10 minutes that a sketch stays dirty, counted from the change that made it dirty or from its previous recovery write in this run, the session writes one recovery file for it. The folder is `recovery`, next to the preferences file, in the same user-data directory. The file is `recovery/<id>.json`, with the sketch id in decimal. Its body is the snapshot JSON plus `path` when the sketch has a file. A `.smx` write does not include `path`. The recovery write does not modify the `.smx`. It replaces the previous recovery file only after the new bytes are complete, and a later interval overwrites that same file. A panic writes that same file once more for each dirty sketch. A kill that never reaches that hook leaves the last timed file. A successful Save or Save As deletes that sketch's recovery file. Save a Copy leaves it in place. On launch, each recovery file that remains is offered. Restoring one opens it with the dirty flag set and with no selection, and sets the sketch's file to the stored `path` when there is one. Restore does not write the `.smx`. A recovery file that cannot be read, or whose snapshot id disagrees with its name, is skipped and does not stop the app. The allocator still starts above that file's id. Whether Discard, or declining the offer, deletes that recovery file is not decided.

Existing `sketch-<id>.json` files in the user-data directory are not imported and not deleted. The running app uses that user-data directory for preferences, recovery, and those leftover names. A test passes a directory it created and writes `.smx` files only under a directory it created. Tests do not write the real user-data directory or the person's files, and they do not open the native dialog.

## Main flow and failures

The app opens on Untitled 1 with Canvas 1 highlighted, black active, the brush current, origin centered, zoom 1, and the panel closed. A brush drag on the highlighted canvas appends canvas-local samples and commits one stroke on release, to the layer that came from the active brush color. The frame rebuilds only changed caches: the stroke's visible ink, its layer's union, and that layer's tessellation. It then paints every visible node in tree order under its world transform.

Failure stays inside one command. Invalid input does not append a sample, and a rejected command leaves the session unchanged. The process is single-threaded.

Platform pressure: on Wayland, `tablet.rs` connects `octotablet` to the window's display. While a pen is in range, its pointer events replace any the compositor sends. Contact is a primary press and lift a release. Each touching pose is one sample with its own pressure. The eraser end switches tools as above. An idle window repaints every 100 ms so a pen coming into range is noticed. Elsewhere, including X11, stored pressure is `None`.

Canvas presses start a gesture only when egui reports the canvas itself hovered, so the menu, a popup, a dialog, or the panel over it never draws. A press while the sketch menu is open closes the menu and does nothing else.

## Environments and integration

Local preview is a native eframe window from the app worktree, desktop only, with `cargo test` and `cargo run`. eframe's default features pull winit, wgpu, Wayland, and X11; the Linux packages are in the app README. There is no account, network service, or seed sketch. The file dialog uses the desktop portal at runtime, so libdbus and a GTK, GNOME, or KDE portal backend need to be present.

### Dependency set

- **`eframe` / `egui` 0.36.2**, MIT OR Apache-2.0, MSRV 1.95. Window, input, painting, text layout, and drag-and-drop for the outliner.
- **`perfect_freehand` 0.1.1**, MIT. Brush and eraser outlines. Its two-point pressure quirk is handled in the app.
- **`i_overlay` 9.0.1**, MIT OR Apache-2.0. Outline resolution, erase difference, layer union, and intersection tests.
- **`earcutr` 0.5.0**, ISC. Tessellation of resolved rings with holes.
- **`octotablet` 0.1.0**, MIT, Wayland feature only, as the patched copy in `vendor/octotablet` (see its `PATCHED.md`).
- **`iconflow` 2.1.0**, MIT, feature `pack-iconoir`. Compiles and runs with eframe 0.36.2.
- **`kurbo`**, Apache-2.0 OR MIT. To be added in the shapes slice for affine transforms, shape paths, stroke expansion with dashes, caps, and joins, and flattening. Version 0.13.1 was read on 2026-10-09; the version is pinned when it is first compiled. It does not replace `perfect_freehand` for pressure strokes.
- **`serde` and `serde_json`**, plus a platform user-data directory crate, for preferences, recent paths, recovery, and the `.smx` body. Versions are not fetched; they are pinned in the slice that first needs them.
- **`rfd`**, MIT. Native open (`pick_file`) and save (`save_file`) dialogs from the eframe 0.36 / egui 0.36.2 window, on Wayland and X11. The version is not fetched here; it is pinned when the crate is first compiled. Default features stay on, so the Linux backend is the XDG desktop portal. The portal opens the desktop's dialog, can parent it to this window, and leaves pointer buttons with the frame. The `gtk3` feature stays off, because an in-process GTK dialog would take those buttons and needs GTK headers this stack does not build against. An egui file-browser widget is not a native dialog. A crate that only launches zenity or kdialog has no dialog when that tool is missing; rfd uses zenity only when libdbus is missing.
- **Inter** 4.001, SIL Open Font License 1.1, as one variable font in the app's `assets/fonts` with its license. It is the typeface for all interface text: weight 400 for body text and 600 for headings, chosen through the font's `wght` axis. Text objects will use it too.

No tree-view, color-picker, or inspector crate: the outliner, color controls, and curve editor are built from the schema and egui primitives.

## Verification seams

Unit tests sit on the session and document. The egui-frame harness in `canvas.rs` covers input that must not draw. They cover failures that would ship the wrong sketch:

- Brush: pressure 1, missing pressure, and pressure size off share a diameter of the stroke's size; lower pressure through a linear curve is thinner. A non-linear curve changes the diameter and leaves samples unchanged. Building an outline leaves samples and ids unchanged.
- Palette layers: a color earlier in the palette paints above later ones. Moving a color changes paint order and nothing else. Editing a layer's dash or cap changes the outline and leaves samples. Editing a layer's size redraws only strokes that follow it. Opacity overlaps within a layer do not stack.
- Tool palettes: a new canvas, shape, or text object copies its tool's palette, and later tool edits leave it alone. A stroke goes into the layer whose `source` is the active brush color, and a canvas without one gains it in the same undo step. Moving the Size slider sets `next_size`, turns `lock_size` off, and records no history; a stroke drawn then keeps that size when its layer's size changes. Make palette default relinks that canvas's layers.
- Availability: the brush and eraser need exactly one visible, unlocked canvas. The brush also needs the layer it would draw into to be visible and unlocked when it exists. Losing the highlight switches to select.
- Eraser: it hides ink only inside its outline, never centerline extras. It affects only the highlighted canvas's visible, unlocked layers, and never shapes or text. Later strokes keep full ink. Undo restores. Moving the canvas moves the hidden ink.
- Canvas box: transformed ink hull bounds plus padding. No visible ink means no box and no hit. A 45-degree rotation of round ink does not widen the box.
- Tree: group then ungroup keeps world positions. Transforms never rewrite geometry or samples. Hidden or locked ancestors stop picking.
- History: every step kind round-trips through undo and redo. A new step clears redo. Camera, selection, the active brush color, tool settings and palettes, and the panel stay out of history.
- Schema: setting a value clamps to its range. Mixed values are reported for a multi-selection. The registry checks listed under Data-driven core.
- Tabs and closing: as today, with the new clean-sketch rule.
- Preferences: a missing or corrupt file falls back to defaults, with an empty recent list.
- Storage, in its slice: a `.smx` round-trips the id, name, camera, and object tree, and omits history, selection, the clipboard, tool settings, and the panel. Save writes that path and clears the dirty flag; a failed write leaves the flag set and keeps the previous file. Save a Copy leaves the sketch's file unchanged. Two files may share a display name and open as separate tabs. Opening a path that is already open focuses its tab and does not load a second copy. A second file whose sketch, node, palette, or stroke ids are already live does not alias them, and a layer's source stays as stored. A recovery write leaves the `.smx` unchanged; a successful Save or Save As removes that recovery file; restore opens with the dirty flag set, keeps the stored path, and does not write the `.smx`. An unreadable `.smx` or recovery file, and a missing recent path, are skipped. A leftover `sketch-<id>.json` is neither imported nor deleted. The test passes a directory it created and does not open the native dialog. The 10-minute schedule is not waited out; the test calls the recovery write.

Do not snapshot meshes, test the sample drawings, or test label strings and icon codepoints. Appearance, smoothing, and whether an erase follows its path are judged in the window. No second test framework.

### Implementation slices

Assign these to **task-operations**. This document does not assign them. Each slice keeps the earlier journeys working.

1. **Document tree and registries.** Replace the mark list with the object tree, canvases, and palette layers, keeping erase, camera, tabs, and the tablet path. Add the tool and command registries and drive the toolbar, shortcuts, and hover text from them. A new sketch gets Canvas 1. Rename the pen to Brush, on B. Wire New sketch and Fit; Open and Save wait. Swatches come from the highlighted canvas. Size binds per tool. Add the generic history steps.
2. **Schema and panel.** Add the property schema and `Inspect` components; the panel with tool and color pages, F4, and right-click; the preferences store; palette editing (edit, duplicate and delete from the color page's menu, and move by dragging swatches); dashed and flat-cap ink; stabilization; and the pressure-curve control. The panel chrome is the one in `INTERFACE.md`: the edge tab, no Properties header, no close button, and no tool-name and description card.
3. **Select, outliner, and groups.** Selection, handles, affine transforms, the context actions, and the Layers page with the selection's sections. Outliner rows with eye, lock, double-click rename, right-click row menus, and drag. New canvas in the toolbar, group, and ungroup. The Layers page uses that same chrome, even though its render still draws a header and a close button.
4. **Shapes** with `kurbo`: fill, stroke, dash, corner radius, constrain, and snap. Their pages use the same panel chrome, even where their renders still draw a Properties header, a close button, or a tool-name card.
5. **Text**, with Inter bundled. Its page uses that same chrome.
6. **Tool palettes.** Give every creation tool its palette and the brush its active color as tool settings; copy palettes into new objects; move shape and text colors into their palettes; add the Palette section, stroke sizes, `source` links, Lock size, and the Size control that never edits the sketch. The owner set this model on 2026-10-11, after shapes and text were built.
7. **Clipboard.**
8. **Sketch files.** The snapshot JSON is the `.smx` body. Replace the private per-id list with the path, the native dialogs, recovery, and recent paths. Leave `sketch-<id>.json` files where they are.

Slices 1 to 3 edit the same core files and run one at a time. Slices 4 and 5 can run beside each other once slice 3 lands, with one writer for `document.rs`.

### Technical choices in this contract

- Fit margin is 24 units; grid spacing is 24 units.
- Paste offset is `(16, 16)` units; nudges are 1 or 8 units.
- Canvas padding defaults to 16 units, from 0 to 128.
- Dashes are 3× and gaps 2× the stroke size or width.
- Stabilization averages up to 16 samples.
- Rotation snaps to 15 degrees with Shift.
- Shape outlines are flattened to 0.05 local units.
- Defaults:
  - eraser size 8;
  - shape Outline or Stroke black at size 3, solid, at 100% opacity, with round caps for the line;
  - shape Fill off, light red `#FCA5A5` at 40% when turned on;
  - text Text color black at 100%; Background off, white at 100% when turned on;
  - corner radius 0;
  - text Inter, 24 units, black, at 1.2 line height, auto-size, wrap on;
  - brush preset Round Brush: pressure on, linear curve, smoothing 50, stabilization 0, streamline 50;
  - eraser preset Round Eraser: the same, at size 8.
- The brush's starting palette is today's swatch colors for black, red, orange, yellow, green, blue, purple, pink, and light gray, each at size 3, full opacity, solid, round caps, with Lock size on.
- A `.smx` file is the versioned snapshot JSON at a path the person chooses. Preferences, recent paths, and the recovery folder stay in the platform user-data directory.
