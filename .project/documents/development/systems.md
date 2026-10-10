# Simple Sketch — systems

**Inputs and baseline:** The coordinator accepts [INTERFACE.md](../../../INTERFACE.md) as first-version behavior. Product intent is in [PITCH.md](../PITCH.md). The renders are in `.project/renders/ui/`, listed in INTERFACE.

Current fact: the app repository `vector-sketch` (https://github.com/Spumcake/vector-sketch) has local `dev` at `7920c85` (`Add the pictured shell and Wayland pen pressure.`) in `src/vector-sketch/worktrees/dev`. `origin/dev` is also `7920c85`. The crate has a flat mark list, a session with tabs, the shell, the Wayland pen path, and 19 tests. The object model below is not built. The team repository's remote is https://github.com/Spumcake/vs-team.

## Boundaries and ownership

One native process. `eframe` owns the window and the egui frame. The **session** owns all document and window state. Widgets send session commands and hold no second copy.

The **session** (the window) owns the open sketches in tab order, the active sketch, the current tool, the properties panel's open state and subject, every tool's settings, and the in-app clipboard. Tool settings are loaded from and saved to the preferences store.

Each **sketch** owns its identity, display name, object tree, selection, camera, undo stack, and redo stack. Each **canvas** in it also owns its active color. Those are not shared across tabs.

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
- `defaults.rs` — built-in data: the default palette, presets, the font list, and tool defaults.
- `panel.rs` — the generic properties panel.
- `outliner.rs` — the Layers tree.
- `prefs.rs` and `store.rs` — the preferences file and the sketch snapshots.

## Data-driven core

**Properties.** A `PropId` names every editable value. A `PropDesc` gives its label, its kind, and its default. The kinds are toggle; number (with min, max, step, unit, and a display format such as a percentage); color; choice (labels and optional icons, drawn as a dropdown or segmented buttons); curve; font family; and text. A `SectionDesc` is a titled list of `PropId`s. Sections are shared: Stroke appears for the rectangle, ellipse, and line, and Behavior for the brush and eraser. Each style component implements `Inspect` once: it lists its properties, gets a value, and sets a value. Setting validates and clamps there, and nowhere else.

**Tools.** A `ToolDef` gives the id, label, icon, shortcut, description line, availability (always, or needs a highlighted canvas), panel sections, the property the Size control binds to, context actions, and what it creates.

**Commands.** A `CommandDef` gives the id, label, icon, shortcut, an enabled rule, and the session command it runs. The commands are new sketch, open, save, rename, close, undo, redo, cut, copy, paste, delete, nudges, zoom in, zoom out, fit, zoom to selection, maximize, new canvas, group, ungroup, bring forward, send backward, the three aligns, the three rotations, the two flips, and the panel toggle.

**Panel.** The panel renderer takes a subject, resolves it to sections, and draws each property by its kind. It has no per-tool code. With several objects selected, it shows the sections they all have, and a property whose values differ reads as mixed.

**Built-in data** lives in `defaults.rs` as tables: the default palette, the presets, the font families, and every tool default. Preferences override tool settings and the default palette.

Tests check the tables themselves:
- no two commands or tools share a shortcut;
- every `PropId` in a section is readable through the `Inspect` of each subject that lists that section;
- every icon name resolves.

## Data and contracts

### Camera

Coordinates are `f64`, y down, in document space. A document point `p` appears at screen `(p - camera.origin) * camera.zoom` in egui points. `camera.origin` is the document point at the viewport's top-left. Zoom is finite, from `0.125` through `8`, default `1`. Zoom in and zoom out multiply by `5/4` or `4/5`, clamp, and keep the viewport center fixed. Ctrl or Cmd with scroll uses the same factor and keeps the point under the pointer fixed. Scroll pans vertically, and Shift-scroll pans horizontally. Space-drag and middle-drag pan. None of these change the tool or push history.

Fit frames the union of world bounds of visible objects with a 24-unit margin, or the nearest zoom limit centered on those bounds. With nothing visible, fit sets zoom `1` with the origin at the viewport center. Zoom to selection does the same for the selection's bounds. The grid is a guide with 24-unit spacing; marks are not snapped to it unless a shape tool's Snap to grid is on.

### Identity

`SketchId`, `NodeId`, `ColorId`, and `StrokeId` are non-zero `u64` values. Each is unique in the session and never reused, including across save and open. A new session has one sketch, Untitled 1. New tab creates Untitled N, using the smallest positive integer not used by an open tab. A new sketch contains one canvas, Canvas 1, with a copy of the default palette, highlighted, with its first color active. New canvas uses Canvas N, the smallest positive integer not used by a canvas in that sketch. Group N follows the same rule. Names may repeat.

### Object tree

A sketch's objects form a tree under an implicit root. Children are ordered top to bottom, and earlier children paint above later ones. Every node has `id`, `name`, `visible`, `locked`, a 2D affine `transform` (a `kurbo::Affine`), and a kind:

- **Canvas** — `padding` (0..=128, default 16) and `palette`, an ordered list of at least one color layer.
- **Rectangle** — local `min` and `max`, `corner_radius` (0..=64), `fill`, and `stroke`.
- **Ellipse** — local `min` and `max`, `fill`, and `stroke`.
- **Line** — local `start` and `end`, and `stroke`.
- **Text** — `content` (may contain line breaks), `box_width` (`None` means auto-size), and `style`.
- **Group** — `children`.

A node's world transform is its ancestors' transforms composed with its own. A node is drawn and hit only if it and all its ancestors are visible. It is picked on the canvas only if it and its ancestors are unlocked. Geometry is never rewritten by move, scale, rotate, flip, or align; those change the transform. Ungroup composes the group's transform into each child, so world positions stay.

### Canvases, palettes, and strokes

A **color layer** has `id`, `name`, `rgb`, `opacity` (0..=1), `size` (1..=32), `dash` (solid or dashed), `cap` (round or flat), `visible`, `locked`, and `strokes`, oldest first. Index 0 is the top layer. A canvas's active color is a `ColorId` in its palette. It is saved with the sketch, is not an undo step, and does not dirty the sketch.

A **stroke** has `id`, `samples`, the `dynamics` it was drawn with, and `erases`. Samples and erase paths are in canvas-local space. A stroke has no transform of its own and no color, size, or style: it takes those from its layer.

A **sample** is `{ x, y, pressure }`, where `pressure` is `None` or finite in `0.0..=1.0`. Samples are kept exactly as the device reported them.

**Dynamics** are:
- `pressure_size` (bool);
- `curve`: 2 to 5 points, with x strictly increasing from 0 to 1 and y non-decreasing within 0..=1, joined by monotone cubic interpolation;
- `smoothing`, `stabilization`, and `streamline`, each 0..=100.

The brush and the eraser each have their own dynamics in the session. A stroke stores a copy of the brush's dynamics at commit.

**Brush outline.** Effective pressure is `1.0` when pressure is `None` or `pressure_size` is off, and `curve(pressure)` otherwise. The diameter at a sample is the layer size times the effective pressure. That is `perfect_freehand` with thinning 1 and a library size of half the layer size, as today. `smoothing / 100` and `streamline / 100` go to the library. Stabilization replaces each sample position with the mean of the last `n` positions before outlining, where `n = round(stabilization / 100 × 16)`; 0 or 1 means off. Pressures and the stored samples are unchanged. A flat cap sets the library's start and end caps off; round sets them on. Dashed splits the stabilized centerline by arc length into dashes of 3× size separated by gaps of 2× size, and outlines each dash. A layer's size, dash, or cap change redraws its strokes with the same samples.

**Layer ink.** A stroke's visible ink is its outline minus the union of its erase outlines (`i_overlay`, non-zero). A layer's ink is the union of its strokes' visible ink, tessellated by `earcutr`, and painted once with the layer's rgb and opacity. Overlaps within a layer therefore do not darken. A canvas paints its visible layers from last to first under its world transform. A failed difference, union, or tessellation draws nothing for that stroke or layer that frame and keeps the data.

**Bounding box.** A canvas's box is the union of its visible layers' visible ink bounds, expanded by `padding` on every side, in canvas-local space. With no visible ink it has no box and is not hit on the canvas.

**Brush availability.** The brush and eraser are available when the selection is exactly one canvas, and that canvas and its ancestors are visible and unlocked. The brush also needs the active color visible and unlocked. When the selection stops being one canvas while either tool is current, the current tool becomes select. A brush commit with fewer than two samples writes no history.

### Eraser

The eraser has `size` (1..=32, default 8) and its own dynamics (`pressure_size` default on). Its outline uses the same rules as the brush, with its own size and dynamics. Accepted method: polygon difference, not centerline distance.

On release it records one step. The step adds a copy of its path, in canvas-local space, to every stroke on the highlighted canvas whose layer is visible and unlocked and whose current visible ink intersects the eraser outline. Strokes added later are untouched. A path with fewer than two samples, or one that crosses nothing, writes no history. Shapes and text are never erased. A stylus eraser end selects the eraser until lift and then restores the previous tool, only while the eraser is available.

### Shapes

Shape styles are components:
- `FillStyle` is `on`, `rgb`, and `opacity`.
- `StrokeStyle` is `rgb`, `width` (1..=32), `opacity`, and `dash`.

Paths are built with `kurbo`: a rounded rectangle, an ellipse, or a line. The stroke is expanded with `kurbo`'s stroker, using miter joins for sharp rectangle corners, round caps for lines, and a dash pattern of 3× and 2× the width when dashed. It is flattened to 0.05 local units, resolved with `i_overlay`, and tessellated. The fill is the flattened closed path, tessellated. A shape with fill on is hit inside its fill or on its stroke ink. Without fill, it is hit only on its stroke ink.

The shape tools' Constrain proportions and Snap to grid are tool settings only. They are not stored on objects. Shift inverts Constrain proportions during a drag.

### Text

`TextStyle` holds:
- `family`, from the shipped list;
- `size` (8..=144, default 24, document units per em);
- `bold`, `italic`, `underline`;
- `rgb`, `opacity`;
- `background`, a `FillStyle`;
- `align` (left, center, or right);
- `line_height` (0.8..=3.0, default 1.2);
- `letter_spacing` (−5..=20 units, default 0);
- `all_caps`, `wrap`.

Text is laid out with an egui `LayoutJob` at paint time, cached by content, style, width, and zoom. It is drawn as a text shape rotated by the world transform's rotation and scaled by its uniform scale. Non-uniform scale and flip move and size the text box; glyphs stay upright and unmirrored. Text is hit inside its box. The shipped families are Inter, in regular, bold, italic, and bold italic, and egui's monospace face. Text is never erased.

### Selection and transforms

Selection is a set of `NodeId`s per sketch. A canvas click picks the topmost visible, unlocked root-level node whose ink, fill, or text box is under the pointer. The outliner can pick a node at any depth. The selection box is the union of selected nodes' world bounds. Corner drags scale about the opposite corner, and Shift keeps the aspect. The rotation handle rotates about the box center, and Shift snaps to 15 degrees. Nudges are 1 unit, or 8 with Shift. Align left, center, and right move each selected node horizontally to the selection's left edge, center, or right edge. Rotate applies 90 degrees clockwise, 90 degrees counterclockwise, or 180 degrees about the box center. Flip scales by −1 on x or y about the box center. Group inserts a new group at the topmost selected node's position, with the selected nodes in their current order. Selection changes are not undo steps.

### History

One undo stack and one redo stack per sketch. Every entry is one of a small set of reversible steps, alone or in a batch:

- insert subtrees at a parent and index, and its inverse, remove;
- move nodes between parents or indexes;
- set transforms, with before and after values;
- set properties on a node or a color layer, with before and after `PropValue`s per `PropId`;
- add a stroke to a layer, and its inverse;
- add erase paths to strokes, and its inverse;
- insert a color layer, and its inverse, remove;
- move a color layer within a palette;
- replace a text object's content.

Paste, cut, delete, group, ungroup, align, and multi-object edits are batches. Renames, visibility, and lock are property steps. A new step clears redo. Undo and redo of an empty stack change nothing and return an empty-history error. Camera, selection, active color, tool settings, and the panel are not history. A finished slider drag is one step.

### Panel subject

The panel subject is one of: a tool, by `ToolId`; a palette color, by canvas and `ColorId`; or Layers. Changing the current tool sets the subject to that tool, and to Layers for select. Right-clicking a tool button sets that tool as the subject without changing the current tool. Right-clicking a swatch sets that color. Editing a tool subject changes session tool settings and saves preferences. Editing a color, or a selected object under Layers, records a set-properties step.

### Clipboard, tabs, and closing

The clipboard holds deep copies of whole subtrees. A copied canvas brings its palette, strokes, and erases. Paste inserts copies at the root top with new ids, offset by `(16, 16)` from the source, then another `(16, 16)` on each repeat, and selects them. Copy waits with nothing selected.

A clean sketch has only its starting canvas, with no strokes, the default palette unchanged, its first color active, and the generated name, and has never been saved. Closing the only sketch when it is clean does nothing. Closing any other last sketch ends with a new sketch. A sketch with changes since its last save, or with an edited name, asks once: Save, Discard, or Cancel, with the behavior in INTERFACE. Quitting asks once per such sketch, in tab order. A completed rename dirties the sketch and is not an undo step.

### Preferences and storage

The preferences store is one versioned JSON file (`format_version` 1) in the platform user-data directory for Simple Sketch. It holds the tool settings and the default palette, and is written whenever they change. A missing or unreadable file means built-in defaults, and it is never treated as an error that stops the app.

The snapshot store keeps one versioned JSON file per `SketchId` in that directory, with `format_version` 1. It holds the id, saved name, camera, the full object tree with every node's fields, palettes with their strokes, samples, dynamics, and erases, and each canvas's active color. It does not hold history, selection, the clipboard, tool settings, or the panel. Save overwrites that id and clears the dirty flag. Open behaves as INTERFACE describes. There is no published format and no format picker.

## Main flow and failures

The app opens on Untitled 1 with Canvas 1 highlighted, black active, the brush current, origin centered, zoom 1, and the panel closed. A brush drag on the highlighted canvas appends canvas-local samples and commits one stroke to the active layer on release. The frame rebuilds only changed caches: the stroke's visible ink, its layer's union, and that layer's tessellation. It then paints every visible node in tree order under its world transform.

Failure stays inside one command. Invalid input does not append a sample, and a rejected command leaves the session unchanged. The process is single-threaded.

Platform pressure: on Wayland, `tablet.rs` connects `octotablet` to the window's display. While a pen is in range, its pointer events replace any the compositor sends. Contact is a primary press and lift a release. Each touching pose is one sample with its own pressure. The eraser end switches tools as above. An idle window repaints every 100 ms so a pen coming into range is noticed. Elsewhere, including X11, stored pressure is `None`.

Canvas presses start a gesture only when egui reports the canvas itself hovered, so the menu, a popup, a dialog, or the panel over it never draws. A press while the sketch menu is open closes the menu and does nothing else.

## Environments and integration

Local preview is a native eframe window from the app worktree, desktop only, with `cargo test` and `cargo run`. eframe's default features pull winit, wgpu, Wayland, and X11; the Linux packages are in the app README. There is no account, network service, or seed sketch.

### Dependency set

- **`eframe` / `egui` 0.36.2**, MIT OR Apache-2.0, MSRV 1.95. Window, input, painting, text layout, and drag-and-drop for the outliner.
- **`perfect_freehand` 0.1.1**, MIT. Brush and eraser outlines. Its two-point pressure quirk is handled in the app.
- **`i_overlay` 9.0.1**, MIT OR Apache-2.0. Outline resolution, erase difference, layer union, and intersection tests.
- **`earcutr` 0.5.0**, ISC. Tessellation of resolved rings with holes.
- **`octotablet` 0.1.0**, MIT, Wayland feature only, as the patched copy in `vendor/octotablet` (see its `PATCHED.md`).
- **`iconflow` 2.1.0**, MIT, feature `pack-iconoir`. Compiles and runs with eframe 0.36.2.
- **`kurbo`**, Apache-2.0 OR MIT. To be added in the shapes slice for affine transforms, shape paths, stroke expansion with dashes, caps, and joins, and flattening. Version 0.13.1 was read on 2026-10-09; the version is pinned when it is first compiled. It does not replace `perfect_freehand` for pressure strokes.
- **`serde` and `serde_json`**, plus a platform user-data directory crate, for preferences and snapshots. Versions are not fetched; they are pinned in the slice that first needs them.
- **Inter** font files, SIL Open Font License 1.1, bundled for text.

No tree-view, color-picker, or inspector crate: the outliner, color controls, and curve editor are built from the schema and egui primitives.

## Verification seams

Unit tests sit on the session and document. The egui-frame harness in `canvas.rs` covers input that must not draw. They cover failures that would ship the wrong sketch:

- Brush: pressure 1, missing pressure, and pressure size off share a diameter of the layer size; lower pressure through a linear curve is thinner. A non-linear curve changes the diameter and leaves samples unchanged. Building an outline leaves samples and ids unchanged.
- Palette layers: a color earlier in the palette paints above later ones. Moving a color changes paint order and nothing else. Editing a color's size, dash, or cap changes the outline and leaves samples. Opacity overlaps within a layer do not stack.
- Availability: the brush and eraser need exactly one visible, unlocked canvas. The brush also needs a visible, unlocked active color. Losing the highlight switches to select.
- Eraser: it hides ink only inside its outline, never centerline extras. It affects only the highlighted canvas's visible, unlocked layers, and never shapes or text. Later strokes keep full ink. Undo restores. Moving the canvas moves the hidden ink.
- Canvas box: ink bounds plus padding. No visible ink means no box and no hit.
- Tree: group then ungroup keeps world positions. Transforms never rewrite geometry or samples. Hidden or locked ancestors stop picking.
- History: every step kind round-trips through undo and redo. A new step clears redo. Camera, selection, active color, tool settings, and the panel stay out of history.
- Schema: setting a value clamps to its range. Mixed values are reported for a multi-selection. The registry checks listed under Data-driven core.
- Tabs and closing: as today, with the new clean-sketch rule.
- Preferences: a missing or corrupt file falls back to defaults.
- Storage, in its slice: two snapshots can share a name, a second save overwrites one id, and opening an id that is already open does not replace it.

Do not snapshot meshes, test the sample drawings, or test label strings and icon codepoints. Appearance, smoothing, and whether an erase follows its path are judged in the window. No second test framework.

### Implementation slices

Assign these to **task-operations**. This document does not assign them. Each slice keeps the earlier journeys working.

1. **Document tree and registries.** Replace the mark list with the object tree, canvases, and palette layers, keeping erase, camera, tabs, and the tablet path. Add the tool and command registries and drive the toolbar, shortcuts, and hover text from them. A new sketch gets Canvas 1. Rename the pen to Brush, on B. Wire New sketch and Fit; Open and Save wait. Swatches come from the highlighted canvas. Size binds per tool. Add the generic history steps.
2. **Schema and panel.** Add the property schema and `Inspect` components; the panel with tool and color subjects, F4, and right-click; the preferences store; palette editing (edit, duplicate, delete, move); dashed and flat-cap ink; stabilization; and the pressure-curve control.
3. **Select, outliner, and groups.** Selection, handles, affine transforms, the context actions, and the Layers subject with the selection's sections. Outliner rows with eye, lock, rename, and drag. New canvas, group, and ungroup.
4. **Shapes** with `kurbo`: fill, stroke, dash, corner radius, constrain, and snap.
5. **Text**, with Inter bundled.
6. **Clipboard.**
7. **Snapshots**, after every object kind exists, so the schema is written once.

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
  - shape stroke black at width 3, solid, at 100% opacity;
  - shape fill off, light red `#FCA5A5` at 40% when turned on;
  - corner radius 0;
  - text Inter, 24 units, black, at 1.2 line height, auto-size, wrap on;
  - brush preset Round Brush: pressure on, linear curve, smoothing 50, stabilization 0, streamline 50;
  - eraser preset Round Eraser: the same, at size 8.
- The default palette's values are today's swatch colors for black, red, orange, yellow, green, blue, purple, pink, and light gray.
- Preferences and snapshots are versioned JSON in the platform user-data directory.
