# Shell review and polish

**UTC:** 2026-10-10T21:01:28Z
**Scope/baseline:** App repository `vector-sketch`, branch `dev`. The shell, the Wayland pen path, and this polish were committed together as `7920c85` (`Add the pictured shell and Wayland pen pressure.`) on top of `bc987c4`. `dev` is one commit ahead of `origin/dev` and was not pushed. Follows [the pictured shell audit](2026-10-10T17-59-54Z-pictured-shell.md).
**Outcome:** Completed. The owner reviewed each round in the window and asked for this commit.

## Goal and participants

The owner asked for a look at the shell milestone, then for changes so the window matched [ui.png](../../../renders/ui/ui.png) and their own suggestions. One agent, Claude Code, reviewed, changed, tested, and committed the work. No Cursor roles ran. The review was self-review.

## Attempts and result

The review found these problems:
- The canvas started strokes from the raw pointer position, so presses on the sketch menu or the keep-this-sketch dialog could leave stray marks.
- The toolbar rows sat about 9 pt low. `ui.horizontal` inside each scroll area started short and grew downward. The zoom group was outside the scroll area, so it looked raised.
- The zoom group was not at the far end.

The fixes, in [canvas.rs](../../../../../src/vector-sketch/worktrees/dev/src/canvas.rs), [toolbar.rs](../../../../../src/vector-sketch/worktrees/dev/src/toolbar.rs), and [tabs.rs](../../../../../src/vector-sketch/worktrees/dev/src/tabs.rs):
- **Colors.** Sampled from the picture: a white active tab joined to a white toolbar, gray inactive tabs, group dividers, and a light canvas at (251,251,252).
- **Layout.** Full-height rows and fixed-size icon slots, with zoom at the far right.
- **Owner's requests.**
  - "Size" sits on the slider's row.
  - The selected swatch's ring takes its own color.
  - The grid is fainter, and its spacing is 24 units. [systems.md](../systems.md) records the spacing.
  - The title bar is set to the light theme.
- **Input.** Presses and scrolls go to the canvas only when egui reports it hovered. A press on the canvas while the menu is open closes the menu and draws nothing.

## Evidence and limits

`cargo test` passed 19 of 19. Three new tests drive whole frames with synthetic pointer input:
- a canvas drag draws;
- a drag over the dialog draws nothing;
- the press that dismisses the menu draws nothing.

The last two failed when their fix was removed. Clippy's only remaining warning in app code predates this work. Headless X11 screenshots at 1280×800 confirmed the alignment, tab styling, and colors.

Not checked: the light title bar on the owner's Wayland desktop, and the zoom order, which follows the picture (zoom out, then zoom in) while [INTERFACE.md](../../../../../INTERFACE.md) lists zoom in first.

## Next action

The owner decides on Fit, the three file icons, and 9 or 11 swatches. Pushing `dev` is a separate request.
