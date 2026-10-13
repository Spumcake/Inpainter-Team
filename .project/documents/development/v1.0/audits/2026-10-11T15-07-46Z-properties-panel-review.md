# Properties panel correction and restyle

**UTC:** 2026-10-11T15:07:46Z
**Scope/baseline:** App repository `vector-sketch`, branch `dev`. The properties-panel slice and the changes below were committed together as `4398ea6` (`Add the properties panel for the brush, eraser, and palette colors.`) on top of `6210d8e`. `dev` is two commits ahead of `origin/dev` and was not pushed. Follows [the properties-panel audit](2026-10-11T01-53-00Z-properties-panel.md).
**Outcome:** Completed. The owner reviewed the panel in the window, supplied a mockup and updated renders, and asked for this commit.

## Goal and participants

The owner found that the color page had been built from `extra-color-properties-ui.png` rather than `color-property-ui.png`. They asked for the documents and code to be corrected and the panel restyled to match the mockups and the toolbar. They then asked for the panel's title bar and close button to be replaced by a toggle tab. One agent, Claude Code, changed the documents and code, tested, and committed. No Cursor roles ran. The review was self-review.

## Attempts and result

The wrong render came from the agent's own earlier INTERFACE text, which listed both color renders and mixed their controls. The documents now name `color-property-ui.png` alone. The owner settled three points:
- Join and the third cap option are dropped, because brush ink cannot draw them.
- Sorting is a drag of the toolbar swatches.
- The extra render is a future reference only.

Duplicate and Delete moved into a menu.

The code changes:
- **Color page.** [schema.rs](../../../../../src/vector-sketch/worktrees/dev/src/schema.rs) defines the page as Color, Stroke, and Stroke Style, with Duplicate and Delete in a menu. The RGB/HSL/HSV sliders, the Layer section, and Move up and Move down are gone.
- **Shared controls.** [widgets.rs](../../../../../src/vector-sketch/worktrees/dev/src/widgets.rs) holds the slider, switch, number and text fields, dropdown, and segmented choice, used by the toolbar and [panel.rs](../../../../../src/vector-sketch/worktrees/dev/src/panel.rs). The blue accent is replaced by the toolbar's near-black.
- **Typeface.** egui's thin default font could not do bold. Inter is bundled in `assets/fonts` with its OFL license, at weights 400 and 600 from its variable axis.
- **Swatch sorting.** Dragging a swatch along the toolbar sorts the palette in one undo step.
- **Panel top.** The title, close button, and subject card are gone. The color menu moved to the end of the Color heading. An edge tab on a layer above the canvas toggles the panel, and its chevron was later centered on the tab's visible inside.

INTERFACE, systems, tasks, testing, plan, and todo record these decisions. INTERFACE also now lists the owner's `update/` renders as the panel reference.

## Evidence and limits

`cargo test` passed 36 of 36. The new tests check:
- the color page's sections against its render;
- a swatch drag sorting in one step while a click only picks a color;
- the tab toggling the panel without drawing.

The swatch test failed when swatches could not be dragged. The tab test still passed with the tab on the canvas's layer, because egui puts a newly shown floating layer above the canvas, so no failing case was shown for it. Clippy's four remaining warnings predate this work. Headless screenshots matched the renders and mockup, and pixel sampling put the chevron within half a point of the tab's center. The app ran on Wayland.

Not checked in a live window: the color picker's single undo step, the menu's Duplicate and Delete, and typing into number fields.

## Next action

Selection and the Layers outliner are the next milestone in [plan.md](../plan.md) and are not started. Pushing `dev` is a separate request.
