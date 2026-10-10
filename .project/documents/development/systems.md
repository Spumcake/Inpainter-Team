# Simple Sketch — systems

**Inputs and baseline:** The coordinator accepts [INTERFACE.md](../../../INTERFACE.md) as first-version behavior. Product intent is in [.project/documents/PITCH.md](../PITCH.md). The desktop reference is [.project/renders/ui/ui.png](../../renders/ui/ui.png). The team repository has no git remote and no `ARCHITECTURE.md`. `.gitignore` ignores `/src/`. There is no application source.

Current fact: the branch-manager bare-cloned `https://github.com/Spumcake/vector-sketch` into `src/vector-sketch/worktrees/.bare`. The fetch found no branches. GitHub’s default branch name is `main`, and `origin/main` is absent because the repository has no commits. `src/vector-sketch/worktrees/dev` does not exist.

Crate versions below were read from crates.io or docs.rs on 2026-10-09, except where a line says a version was not fetched. None were resolved or compiled. No `cargo` command was run.

## Boundaries and ownership

One native process. `eframe` owns the window and the egui frame. The **session** is the single owner of sketch state.

The session owns the open sketches in tab order, which sketch is active, the toolbar tool, the current swatch, the current size, and the in-app clipboard. Tool, swatch, and size stay put when the active tab changes.

Each **sketch** owns its identity, display name, marks in paint order, selection, camera, undo stack, and redo stack. Marks, selection, camera, and history are not shared across tabs.

A **mark** owns its identity, swatch, size, transform, local geometry, and the erase paths attached to it. Samples and shape points stay in that local geometry. Move and scale change the transform. They do not rewrite those points.

Visible ink is derived. The outline polygon, the polygon difference, and the tessellation are rebuilt for drawing and hit testing. They are not stored.

The canvas maps pointers and gestures onto session commands and paints meshes. Toolbar, tab, and menu widgets send the same commands. They hold no second copy of marks, history, or cameras.

Dependency direction: `main` → canvas and chrome → session → `perfect_freehand`, `i_overlay`, and `earcutr`. The canvas also uses `src/tablet.rs` → `octotablet` for stylus input. The session does not depend on egui or the tablet.

Accepted stack: a native Rust binary, egui via eframe, Iconoir for toolbar icons. Electron, Tauri, and a fork of another drawing application are out. No accounts, sync, or collaboration.

## Source map

Planned app repository: `vector-sketch`. The bare clone and its origin URL, `https://github.com/Spumcake/vector-sketch`, already exist at `src/vector-sketch/worktrees/.bare`. `origin/main` is absent until `main` has a commit. The branch-manager adds the permanent checkout at `src/vector-sketch/worktrees/dev` on branch `dev` from `origin/main` only after that commit exists. Planned files, all still absent from that checkout:

- `src/lib.rs` — library root, so model tests do not open a window.
- `src/session.rs` — tabs, toolbar tool, swatch, size, clipboard, close and save identity.
- `src/sketch.rs` — marks, transforms, erase attachments, selection, camera, the one history.
- `src/canvas.rs` — pointer mapping and mesh painting.
- `src/main.rs` — eframe entry.
- `src/tablet.rs` — stylus input beside eframe. It turns tablet events into egui pointer events and per-sample pressure.
- Later slices add `src/toolbar.rs`, `src/tabs.rs`, and `src/store.rs`. They call the session. They do not introduce a second document model.

The team repository keeps this file. The app repository holds only the app.

## Data and contracts

Coordinates are `f64`, y down, in document space. A document point `p` appears at screen `((p - camera.origin) * camera.zoom)` in egui points. `camera.origin` is the document point at the viewport’s top-left. Zoom `1` means one document unit per egui point. Pan and zoom change only that camera. Stored size, samples, and shape points stay as they were.

Zoom is a finite value from `0.125` through `8` (12.5% through 800% of the default). The default is `1`. Zoom-in and zoom-out multiply the current zoom by `5/4` or by `4/5`, then clamp, and keep the viewport center fixed. Ctrl or Cmd with scroll uses the same factor and keeps the document point under the pointer fixed. Scrolling pans vertically. Shift-scroll pans horizontally. Space-drag and middle-button drag pan. Those gestures do not change the active tool and do not push history.

Empty fit sets zoom to `1` and places the origin at the viewport center. A sketch whose marks are all fully hidden uses that same empty fit. Otherwise fit frames the bounds of visible ink with a margin of 24 document units on each side. When that frame would pass a zoom limit, fit uses the nearest limit and centers on those bounds.

`SketchId` and `MarkId` are non-zero `u64` values, unique in the session, and never reused, including across save and open. A new session starts with one sketch named Untitled 1. The new-tab command creates Untitled N using the smallest positive integer not already used by an open tab. Names may repeat. The pictures titled Notes and Sketches are not built-in marks.

The toolbar size is an integer from 1 to 32, default 3. A typed value is clamped into that range. The size is stored on the mark in document units. Zoom changes screen thickness only. The current swatch is one of, in order: black, red, orange, yellow, green, cyan, blue, purple, pink, light gray, gray. The default is black. The toolbar paints those swatches to match the reference; the session stores the name.

A sample is `{ x, y, pressure }`. `pressure` is `None` when the device did not report force, otherwise finite and in `0.0..=1.0`. Missing pressure is treated as `1.0`. Full pressure draws at the stored size. Lower pressure draws thinner. `simulate_pressure` is false. There is no velocity simulation and no smoothing control.

Upstream perfect-freehand, read from its TypeScript and not executed in the Rust port, uses `radius = size * (0.5 - thinning * (0.5 - pressure))`, and uses `size / 2` when thinning is 0. The Rust options must be set so the diameters below hold even if that port’s defaults differ:

- Pen: `thinning` 1 and a library size of half the stored integer, with the sample pressure above. Pressure 1 then has diameter equal to the stored size.
- Eraser: `thinning` 0 and a library size equal to the stored integer, with device pressure ignored. Every sample has diameter equal to that integer.
- `smoothing` and `streamline` are `0.5`, the upstream documented defaults, so small jitters fall away while a curve stays curved and a straight pass stays straight.

An open gesture is outside the undo stack. Switching tools commits it. `begin` fails if a gesture is already open or the size is outside 1..=32. `add_sample` fails if the gesture is not open, a coordinate is non-finite, or pressure is outside the range above. A repeated position with the same pressure is ignored. A repeated position with a new pressure replaces the last sample’s pressure only. A pen commit with fewer than two samples drops the open gesture, writes no history, and leaves committed marks unchanged. Escape cancels an open shape. Escape commits text. Cancel drops the open gesture and writes no history. Any rejected command leaves the previous session unchanged.

### Marks

Every mark has `id`, `swatch`, `size` (1..=32), a transform, local geometry, and zero or more attached erase paths in that same local space. The transform is translation plus independent x and y scale. It is the identity when the mark is committed. Rotation is not a gesture.

Local geometry:

- **Pen.** Ordered samples, including pressure. The filled ink is the pressure outline.
- **Rectangle.** The committed box. Unfilled. Shift constrains the drag to a square before commit.
- **Ellipse.** The committed box. Unfilled. Shift constrains the drag to a circle before commit.
- **Line.** The committed endpoints. Unfilled. Shift constrains the drag to 45-degree steps before commit.
- **Text.** One plain single-line string, no styling. The em size in document units is `8 * size`, so size 3 is a short annotation at zoom 1. The font is egui’s default proportional sans. Enter, Escape, or a click elsewhere commits. An empty commit discards the label. On an existing label that discard is one history step. The text tool edits an existing label in place; that replacement is one history step.

Shape and text tools are later slices. Slice 1 still defines these variants on the one mark list so those slices add commands rather than a second model. Slice 1 constructs only pen marks.

The ink of a rectangle, ellipse, or line is its outline stroked at the stored size, not the interior of the box. Text ink is the glyph shape. Pen ink is the filled pressure outline.

### Eraser

Accepted method: polygon difference, not centerline distance. Centerline distance would hide a capsule around the samples and would miss or include ink the eraser path does not. That is rejected.

On release, the eraser builds one constant-width outline from its samples and size. It records one history step against the marks that already exist and whose current visible ink intersects that outline. Each of those marks stores a copy of the eraser path converted into its local space. Later marks are absent from that step and keep their ink. A path that crosses nothing, or that has fewer than two samples, writes no history.

Visible ink is the mark’s ink outline minus the union of its attached erase outlines, in local space, then transformed. `i_overlay` computes the difference (`OverlayRule::Difference`). `earcutr` fills the remaining rings, including holes. A failed difference or tessellation keeps the mark and its attachments, draws nothing for that mark that frame, and does not hit-test it.

Hit testing uses that visible ink. A fully hidden mark stays in the sketch. Clicks pass through it. Undo of the erase step drops the attachments it added and the hidden ink returns. Moving or scaling the mark moves its samples and its attached erase paths together, because both live in local space under the one transform.

A stylus eraser end, when the device reports one, selects the eraser until lift and then restores the previous tool.

### History

One undo stack and one redo stack per sketch. Each entry is one committed gesture:

- add a pen stroke, a shape, or a text label
- erase, as the attachments above
- move, or scale from the opposite corner of the selection bounds
- recolor
- delete the selected objects
- paste
- cut
- replace a text label’s string

Slice 1 pushes only a pen add or an erase. The later gestures are further variants of this same entry, not a second history. Pan and zoom are not entries. A new committed gesture clears that sketch’s redo stack. Undo and redo do not clear it. Undo and redo of an empty stack change nothing and return an empty-history error. Undo of a pen add removes that mark. Redo puts it back in paint order.

With a selection, clicking a swatch recolors those marks in one step and becomes the current swatch. Marks already drawn keep their size when the slider moves. Delete or Backspace removes the selected objects in one step. That removal is separate from an erase. Arrow keys nudge the selection by 1 document unit; Shift nudges by 8. Each key press is one move step.

### Clipboard, tabs, and closing

The clipboard is in-app and belongs to the session. It is a later slice, not a toolbar group in the shell. Copy is available when the selection is non-empty and copies those marks, including attachments and relative positions. Cut copies and then deletes them in one step. Paste adds duplicates on the active sketch, selects only those duplicates, and is one step. The first paste offsets by `(16, 16)` document units from the copied marks. Each further paste adds another `(16, 16)` from the copied marks. New ids are minted. A control with nothing to do waits.

Selection is per sketch: the set of mark ids last chosen by a click, shift-click, or marquee on visible ink. Clicking empty visible area clears it. Shift-click toggles one mark. A marquee selects marks whose visible ink it crosses. Shift-drag adds those marks. The box shown is the bounds of the selection’s visible ink. Dragging a selected mark moves the selection. Corner handles scale from the opposite corner. Slice 1 stores the set and leaves it empty. The select tool is a later slice.

Closing the only sketch, when it is a clean blank, does nothing. A clean blank has no marks, still carries its generated Untitled name, and has never been saved. Closing any other last sketch ends with a new blank sketch, so the window always has one. A sketch with marks since its last save, or a never-saved sketch that has marks, or a sketch whose name was edited, asks once: Save, Discard, or Cancel. Cancel leaves it open and does not add a tab. Save stores it, then closes it. Discard closes it and drops the unsaved name and marks. The replacement blank is created when that close finishes and the sketch was the last one.

Quitting asks once per such sketch, in tab order. Cancel stops the quit and leaves the sketches not yet closed still open. Sketches already saved or discarded in that pass stay closed. Rename makes the active name editable, as does double-clicking it. Clearing the name restores the previous one. A completed rename dirties the sketch and is not an undo step.

### Storage

Save and open are in the product and out of slice 1. There is no published file format and no format picker.

The later store keeps one private snapshot per `SketchId` under the platform user-data directory for Simple Sketch. The snapshot is a versioned JSON object, `format_version` 1, written with `serde_json`. That crate’s version was not fetched in this pass and was not compiled. The object holds the id, the saved name, the camera, and the ordered marks with swatch, size, transform, local geometry, and attached erase paths. It does not hold undo, redo, selection, the clipboard, or the toolbar tool, swatch, and size.

Save overwrites that id and clears the dirty flag. The id was minted when the sketch was created, so the first save does not change it. Open lists one row per snapshot, labeled with the saved name. A row whose id is already open focuses that tab and does not reload over it. Any other row opens the snapshot as a tab. With no snapshots, Open waits. Two rows may show the same name and still open different ids. A rename is visible on the saved row only after the next save.

Camera is restored from the snapshot. Panning or zooming alone does not dirty the sketch. The next save stores the camera anyway.

The grid is a guide. It pans and zooms with the camera. Its spacing is 24 document units. Marks are stored where they are drawn, not snapped.

## Main flow and failures

The app opens on one blank Untitled 1, pen selected, black, size 3, origin centered, zoom 1.

Primary-button drag with the pen begins a stroke, appends document-space samples, and commits on release. Samples store `None` when no force arrived; the outline treats that as pressure 1. The frame builds the pressure outline, subtracts attached erase outlines, tessellates, and paints an egui mesh. The camera is applied only in that mapping.

The eraser uses the same begin and sample path and ignores pressure for its width. On release it attaches one local path to each mark whose visible ink the eraser outline crosses. A miss writes nothing.

Failure stays inside one command. Invalid input does not append a sample. A short pen stroke and a miss erase do not enter history. A history step adds, removes, or updates whole marks, or adds and removes whole attachments. The process is single-threaded.

Platform force is outside the session. The canvas may read `egui::Event::Touch.force` when egui supplies it. `PointerState` has no pressure field. eframe 0.36.2 resolves winit 0.30.13, which delivers a Linux tablet pen as a mouse without pressure.

On Wayland, `src/tablet.rs` connects `octotablet` to the window's display. While a pen is in range, its pointer events replace any the compositor sends. Contact becomes a primary press and lift becomes a release. Each pose drawn while touching becomes one sample with its own pressure. While its eraser end touches, the eraser tool is used, and the previous tool returns on lift. An idle window repaints every 100 ms so a pen coming into range is noticed. Elsewhere, including X11 sessions, the connection is unsupported and stored pressure is `None`, so the outline uses `1.0`.

## Environments and integration

Local preview is a native eframe window from the app worktree, desktop only. Do not add a wasm target. eframe’s default features pull winit, wgpu, Wayland, and X11. The eframe README names these Linux packages, unverified and not installed here: `libxcb-render0-dev`, `libxcb-shape0-dev`, `libxcb-xfixes0-dev`, `libxkbcommon-dev`, `libssl-dev`. There is no account, network service, or seed sketch.

`cargo run` and `cargo test` are the expected local commands and were not executed. There is no pre-commit hook and no app crate yet.

### Dependency set

- **`eframe` 0.36.2** (crates.io, 2026-09-08), **MIT OR Apache-2.0**, MSRV 1.95. It depends on **`egui` 0.36.2** under the same license. Native window, input, and mesh painting. Edition 2024, or Cargo resolver 2. Not compiled here.
- **`perfect_freehand` 0.1.1** (crates.io, 2026-01-27), **MIT**. Builds the pen and eraser outlines. It does not own sketches, erase attachments, or rendering. Fit limits: version 0.1, about 1,272 downloads and one dependent at fetch time; outlines can be concave or self-intersecting. Set the options in the contract above. The Rust `Default` was not executed.
- **`i_overlay` 9.0.1** (docs.rs), **MIT OR Apache-2.0**. Floating-point polygon difference of the ink outline minus the eraser outlines, including self-intersections and holes, which its docs state it supports. Use default features only, without the multithreading feature. Its Cargo.toml lists MSRV 1.88 and edition 2024, which sits under eframe’s 1.95. Not compiled here. This is the smallest checked library that clips ink to the eraser path. A centerline test is not a substitute.
- **`earcutr` 0.5.0** (crates.io listing, 2025-05-29), **ISC**. Fills the difference, which can be concave and can contain holes. `Shape::convex_polygon` is the wrong fill. Use the `Result`.
- **`octotablet` 0.1.0** (crates.io), **MIT**. Reads the Wayland tablet protocol for pen position, pressure, and tool type. Only the `wayland-tablet-unstable-v2` feature is enabled; Windows Ink stays off until it can be tested. The app uses a patched copy in `vendor/octotablet` through `[patch.crates-io]`. The patch caps the bound protocol version at the one compiled in. Without it, a compositor offering tablet version 2 broke the shared Wayland connection and closed the window. The patch is described in `vendor/octotablet/PATCHED.md`. Its X11 support is not implemented upstream.

Left out of the stroke and erase path:

- **`kurbo` 0.13.1**, Apache-2.0 OR MIT. No per-point pressure and no polygon difference. Shape outlines can be egui paths.
- **`ink-stroke-modeler-rs` 0.1.0**, MIT OR Apache-2.0. A second smoother, with no eraser clip.
- **`iconflow` 2.1.0**, MIT, feature `pack-iconoir` only, for the toolbar slice. Iconoir’s icons are MIT. The pack is about 0.41 MiB. Its published egui example depends on eframe 0.33, so behavior on 0.36 was not verified.

Pressure fit limit, from docs rather than a device: `Event::Touch.force` is optional (docs.rs `Event`). Discussion #8666 (2026-10-05) states `PointerState` has no pressure. egui-winit’s master copy maps winit touch force and still has a commented `TouchpadPressure` todo. winit’s published `PointerSource::TabletTool` carries force; this pass did not prove that eframe 0.36.2’s winit dependency includes that API or that egui-winit forwards an eraser end.

## Verification seams and open decisions

Unit tests sit on the session and run with the build once TASK names the command. They cover failures that would ship the wrong sketch:

- Pressure `1.0` and missing pressure share a pen diameter of the stored size. A lower pressure is thinner. An eraser outline keeps that diameter when samples carry a lower pressure. Failure: velocity replaces the stylus, or the eraser narrows with pressure.
- Building an outline leaves samples and the mark id unchanged.
- A point inside both the pen ink and the eraser outline becomes hidden, and a point that is inside a centerline capsule but outside the eraser outline stays visible. Samples and ids stay. A pen stroke committed afterward stays fully visible. Undo restores the hidden point. Translating the mark moves the hidden point with it. Failure: the eraser deletes geometry, clips later ink, or uses centerline distance.
- A fully hidden mark remains in the sketch and is not hit.
- Pan, zoom, and fit leave samples, stored size, and both history stacks unchanged. Zoom clamps to `0.125` and `8`. Empty fit is zoom `1` centered on the origin.
- Size `0` and `33` are rejected. Size `1` and `32` are kept.
- A one-sample pen commit and an erase that crosses nothing leave history unchanged.
- Two sketches do not share marks, selection, camera, or history. Changing tabs leaves tool, swatch, and size as they were.
- Closing the sole clean blank leaves that sketch. After discard, closing the last sketch that has a mark leaves one new blank.

Do not add tests for the sample drawing, icon codepoints, menu label strings, or a golden file. Do not snapshot meshes. A later human preview is the check for whether smoothing and the erase mark look right. No second test framework.

Storage tests belong to the storage slice: two snapshots can share a name, a second save overwrites one id, and open of an id that is already open does not replace its marks.

### Implementation slices

Assign these to **task-operations**. This document does not assign them.

1. **Stroke model and infinite canvas.** Prerequisite: the bare clone at `src/vector-sketch/worktrees/.bare` and its origin `https://github.com/Spumcake/vector-sketch` already exist. `src/vector-sketch/worktrees/dev` does not. The branch-manager adds that checkout on branch `dev` from `origin/main` only after `main` has a commit. Do not clone the repository again. Exclusive ownership of the new crate: `Cargo.toml`, `src/lib.rs`, `src/session.rs`, `src/sketch.rs`, `src/canvas.rs`, `src/main.rs`. Delivers pen drawing, the diameter rules, polygon-difference erase attachments, the one history with pen and erase steps, and the camera gestures and fit rules above. Before any toolbar exists, it also binds P for the pen, E for the eraser, Ctrl or Cmd+Z for undo, Ctrl or Cmd+Shift+Z and Ctrl or Cmd+Y for redo, and Ctrl or Cmd+0 for fit. The mark and history types include the later variants. Slice 1 does not construct them and does not deliver shape tools, text editing, selection behavior, clipboard, tab chrome, the menu, or save and open. Nothing else proceeds in this crate until these types have landed.

2. **Toolbar, tabs, and menu**, after slice 1, one writer for `main.rs`. New files: `src/toolbar.rs`, `src/tabs.rs`. Iconoir via `iconflow` feature `pack-iconoir`. Mounts the pictured shell: swatches, the three icons drawn after the swatches, undo, redo, the tool buttons, size, zoom in, zoom out, and one button that toggles the maximized state. Those three icons have no actions in this slice, because the owner did not name them. The shell does not mount copy, paste, or cut. The far right is zoom in, zoom out, and that maximize toggle, which replaces the maximize controls in the picture. The menu items are New sketch, Open, Save, Rename, and Close. Open and Save call the store once that slice exists; until then those two controls wait.

3. **Shapes, then text, then select and clipboard**, in that order. Each edits `sketch.rs` and the canvas, so they do not run beside another writer of those files.

4. **Private snapshots**, after selection and clipboard, so the schema is written once every mark kind exists. Exclusive new file: `src/store.rs`.

### Accepted from the interface

- Missing pressure draws at full size: constant pressure `1.0`. The eraser width is the current size even when pressure is present.
- Size is an integer from 1 to 32, default 3, in document units. Zoom changes screen thickness only.
- Each sketch keeps its own camera, marks, selection, and undo history. Tool, swatch, and size stay on the toolbar across tabs.
- Default zoom is 1. Limits are `0.125` and `8`. A zoom step is one quarter of the current scale (`5/4`). Empty fit returns to zoom 1 with the origin centered. Fit frames visible ink with a small margin, or the nearest limit. A sketch whose marks are all fully hidden uses that same empty fit.
- Pan is Space-drag, middle-button drag, vertical scroll, and Shift-scroll. Ctrl or Cmd with scroll zooms toward the pointer. A stylus eraser end erases until lift, then restores the previous tool.
- An erase is one operation against existing marks the path crosses. Ink inside that path disappears. The rest remains. Samples and ids stay. Later marks keep their ink. The attachment moves with the mark. Undo restores the hidden ink. Hit testing uses visible ink. A fully hidden mark remains. The clip is polygon difference.
- Slice 1’s history steps are one committed pen stroke or one erase. The same history holds the later gesture steps. Pan and zoom are not history.
- A sole clean blank’s close waits. Closing any other last sketch leaves a new blank. The app does not quit and does not refuse forever. Unsaved changes ask once per sketch.
- In-app clipboard behavior remains for a later slice: copy, paste, and cut. Paste offsets the duplicate and selects it. The shell toolbar does not mount those actions.
- The three icons after the swatches are drawn as in the picture and have no actions in the shell slice. The owner did not name them.
- The far right of the toolbar is zoom in, zoom out, and one button that toggles the maximized state. That toggle replaces the maximize controls in the picture. It is the owner's decision.
- Menu items are New sketch, Open, Save, Rename, and Close.
- Shapes are unfilled outlines. Shift constrains them. Text is one plain single-line sans-serif label. Selection is a bounding box that moves, scales, marquees, and deletes whole objects.
- The repository name is `vector-sketch`.
- Save and open use a stable sketch id and allow repeated names. The person does not choose a format.

### Technical choices in this contract

These are not separate interface questions. They fix numbers the interface left as “small”, “slightly”, or “larger”:

- Fit margin is 24 document units per side.
- Paste steps by `(16, 16)` document units from the copied marks.
- A nudge is 1 document unit, or 8 with Shift.
- Text em size is `8 * size` document units.
- Grid spacing is 24 document units.
- The private snapshot is versioned JSON, one file per `SketchId`, in the platform user-data directory for Simple Sketch.

Nothing in the first-version behavior still needs a user decision before slice 1. The bare clone at `src/vector-sketch/worktrees/.bare` and its origin `https://github.com/Spumcake/vector-sketch` already exist. `dev` is added at `src/vector-sketch/worktrees/dev` from `origin/main` only after `main` has a commit.
