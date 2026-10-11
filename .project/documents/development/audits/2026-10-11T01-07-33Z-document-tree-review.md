# Document tree review and fixes

**UTC:** 2026-10-11T01:07:33Z
**Scope/baseline:** App repository `vector-sketch`, branch `dev`. The document-tree slice and the fixes below were committed together as `6210d8e` (`Replace the mark list with the canvas document tree.`) on top of `7920c85`. `dev` is one commit ahead of `origin/dev` and was not pushed. Follows [the document-tree audit](2026-10-11T00-15-02Z-document-tree.md).
**Outcome:** Completed for the review and the three requested fixes. The owner asked for this commit.

## Goal and participants

The owner asked for a review of the document-tree slice. They reported two problems: a stroke joined its color layer only after release, and the title bar stayed light on a dark desktop. They also asked for the clean-sketch rule to be fixed and the toolbar padding reduced. One agent, Claude Code, reviewed, fixed, tested, and committed the work. No Cursor roles ran. The review was self-review.

## Attempts and result

The review found the slice faithful to [systems.md](../systems.md). Palette layers own their strokes and caches, and the registry drives the toolbar, shortcuts, and hover text. It found two problems:
- `Session::paints` appended the open stroke after every layer. The stroke also skipped the canvas transform, and a translucent color would have darkened over its own ink until release.
- Systems required a clean sketch to have its first color active, yet said the active color does not dirty a sketch. This was the agent's earlier contradiction, and it is why the earlier audit saw a color switch ask to be kept.

The fixes, in [document.rs](../../../../src/vector-sketch/worktrees/dev/src/document.rs), [session.rs](../../../../src/vector-sketch/worktrees/dev/src/session.rs), [canvas.rs](../../../../src/vector-sketch/worktrees/dev/src/canvas.rs), and [toolbar.rs](../../../../src/vector-sketch/worktrees/dev/src/toolbar.rs):
- **Live stroke.** The open stroke is mapped into canvas space and outlined as it will be committed. It paints inside its color layer, joining the layer's union when the color is translucent.
- **Title bar.** A first attempt removed only the explicit light-theme command, and the title bar stayed light. The cause was egui's `sync_window_theme`, which copied the app's light egui theme to the window. It is now off, so winit's Wayland frame reads the desktop color scheme.
- **Clean sketch.** It no longer depends on the active color. The rule in systems.md says so too.
- **Toolbar.** Vertical padding dropped from 8 to 5 points.

[INTERFACE.md](../../../../INTERFACE.md) now says the title bar follows the desktop theme.

## Evidence and limits

`cargo test` passed 25 of 25. Three new tests check:
- an open stroke paints below a higher color and matches its committed outline on a moved canvas;
- no frame sends a window theme command;
- choosing another color keeps a new sketch clean.

Each failed when its fix was removed. Clippy's four warnings are style only. The release build ran on the owner's Wayland session, where the settings portal reports a dark preference, and under Xvfb. A headless screenshot showed the tighter toolbar.

Not checked: the dark title bar on screen, which the agent could not capture. The frame likely reads the desktop setting only when the window opens.

## Next action

The properties panel is the next milestone in [plan.md](../plan.md) and is not started. Pushing `dev` is a separate request.
