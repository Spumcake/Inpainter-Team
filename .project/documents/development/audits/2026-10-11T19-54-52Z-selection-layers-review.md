# Selection and Layers review

**UTC:** 2026-10-11T19:54:52Z
**Scope/baseline:** App repository `vector-sketch`, branch `dev`. The selection and Layers slice and the changes below were committed together as `5567860` (`Add selection, transforms, and the Layers outliner.`) on top of `4398ea6`. `dev` is one commit ahead of `origin/dev` and was not pushed. Follows [the selection and Layers audit](2026-10-11T16-31-33Z-selection-layers.md).
**Outcome:** Completed. The owner reviewed the window over several rounds and asked for this commit.

## Goal and participants

The owner asked for the panel's contents and the outliner, toolbar, and selection handles to match their intent and the renders. One agent, Claude Code, changed the code and documents, tested, and committed. No Cursor roles ran. The review was self-review.

## Attempts and result

- **Panel scale.** Panel contents are drawn at seven eighths of full size, and the panel is 266 points wide. Every slider, the toolbar's included, is one size, with its rail snapped to whole pixels.
- **Outliner.**
  - The header and the canvas section are gone.
  - Canvases, groups, and colors rename in place by double-click or from a right-click menu built from registry tables. A canvas's menu offers Make palette default.
  - Rows are drawn larger than the panel's controls.
  - Every menu shares one set of rows, sized to its longest label.
- **Toolbar.**
  - New canvas, a plus in a circle, sits before the brush. Iconoir has no brush, so a brush is drawn in its style.
  - The select actions are one row of standard toolbar buttons, using the owner's choice of Iconoir icons. Rotate and Flip open menus whose rows have icons.
  - Bring to front and Send to back were added. Zoom to selection moved to Shift+2.
  - The owner turned down Lucide icons, and later the arrange and align menus and the dividers built in between.
- **Handles.**
  - Edge handles scale one axis.
  - The pointer shows each handle's action.
  - Boxes hug the transformed ink hull, with per-axis padding, instead of a rotated rectangle.
- **Rename fix.** Escape and clicking away did not close a rename, because the field took focus every frame. Its text also jumped up. Both are fixed.

INTERFACE, systems, tasks, testing, plan, and the queue record these decisions.

## Evidence and limits

`cargo test` passed 53 of 53. These tests failed without their fix:
- the rotated-box test;
- the rename test, which covers Escape and Enter.

The new cursor, reorder, rename-color, and select-actions tests passed but were not shown to fail. Clippy reports no new warnings. Headless screenshots checked the toolbar, the menus, the row menu, the rename field, and the outliner. Cursors were checked only by test.

## Next action

The select row overflows the default 1280-point window. The fix is the owner's choice, in [todo.md](../todo.md). Shapes and text are next in [plan.md](../plan.md). Pushing `dev` is a separate request.
