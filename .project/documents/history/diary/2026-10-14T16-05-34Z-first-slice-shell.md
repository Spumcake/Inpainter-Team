# The first slice: the shell and an empty demo

**UTC:** 2026-10-14T16:05:34Z
**Covers:** since [references first](2026-10-14T12-10-56Z-references-first.md).
**Repositories:** Before this entry's commits, the team repository was on `master` at `882a549`, and the app repository `Inpainter` had only `2595cfb` ("Start the Inpainter repository.") on local `main` and `dev`, with the slice uncommitted in `src/inpainter/worktrees/dev`. Both GitHub repositories, `Spumcake/Inpainter-Team` and `Spumcake/Inpainter`, were empty. `src/simple-sketch` is clean at `887f6f4`.

After two failed attempts, the first slice of version 0.1 works the way the owner meant: Simple Sketch's own chrome, carried over, running as a shell around a separate sub-application.

## What we did

**Preparation.** The Cursor team prepared v0.1 again under the reference-first pipeline. It wrote the pitch, INTERFACE, tasks, plan, todo, testing, and a reuse map, but no systems document until the map was approved. Claude Code reviewed the set. The reuse map named exact files, functions, and constants to carry over from Simple Sketch, and a spot check of a dozen values against the source found them all correct. INTERFACE described only what the renders show.

The review raised three gaps, and the owner settled them on 2026-10-14:
- **Window identity.** The title is always "Inpainter", and the application id is `io.github.spumcake.Inpainter`.
- **Tabs and menu.** Each tab is a document its sub-application reports. Its label is the name of the file it manages or, with no file, the sub-application's name, so the demo's one tab reads "Demo". The hamburger menu shows the commands of the selected tab's sub-application.
- **The demo does nothing.** Its menu button, plus, and close mark do nothing: no popup, no new tab, no close.

The owner also decided:
- **The grid is a shared component of the host.** The demo declares it as a test case, and a later app, such as a node editor, could use it too.
- **Sub-applications own the document.**
- **A picture of the result.** The owner supplied a mockup of the expected result, saved as [shell-demo.png](../../../renders/ui/shell-demo.png). It is Simple Sketch's chrome with the sketching removed.

**The build.** The Cursor team specified a first protocol in systems and built the slice. The host's chrome was carried over from Simple Sketch's `tabs.rs`, `toolbar.rs`, `widgets.rs`, `panel.rs`, and `canvas.rs`. Its [audit](../v0.1/audits/2026-10-14T15-30-08Z-first-slice-shell.md) records a build, three passing tests, and a captured window that matched the picture.

**The second review.** Claude Code compared every carried-over item with its source:
- **The carry-over held.** Constants, colors, and functions such as `paint_tab`, `glyph_button`, `menu_rows`, `hairline`, `chevron_side`, and `paint_grid` are identical. The functions that changed only lost their ties to Simple Sketch's session, as the map said.
- **The boundary is in the code.** The host depends on `eframe`, `iconflow`, and `protocol`, and the demo only on `protocol`.

The review also found three problems:
- **Labels could not contain spaces.** The protocol separated words by spaces with no quoting, so a tab labeled "Untitled 1" was rejected. Simple Sketch's first tab would have broken it.
- **Closing could hang.** Closing the window waited forever for the sub-application to exit.
- **There was no `.gitignore`,** so the build folder showed as untracked.

**The properties panel.** The owner asked whether the panel belongs to the shell or the demo. It was being drawn for every app. The owner chose the model in which the host owns and draws the panel the same way everywhere, and each sub-application declares whether a tab has one, which side it sits on, and what it holds. The mission was changed to match. The panel left the always-present common UI, and the host gained shared components that sub-applications opt into: the properties panel and the grid.

**The fixes.** Claude Code changed the code:
- **JSON Lines.** The protocol is now one JSON object per line through `serde` and `serde_json`, with a `hello` carrying protocol version 1 first. The messages are `tab`, `contents`, `panel`, and `end` from the app, and `shutdown` from the host.
- **A bounded shutdown.** Closing waits up to two seconds, then stops the sub-application.
- **`.gitignore`** holds `/target`.
- **The panel is declared.** The host draws the panel only for a tab that declares it. The demo declares it on the right, so the picture still holds.

The v0.1 documents and the systems protocol section were updated to match.

## What we learned

The reference-first pipeline worked. Carrying code over, checked against the source, produced the right look where two attempts at describing it had failed. The remaining defects sat in the one part that was new, the protocol, which is where a review should look hardest. The protocol's job is to carry names and commands, so a format that cannot carry a space was a design flaw, not a detail.

## Where things stand

The workspace builds, and the three window tests pass against the demo's real declaration. A label with spaces or quotes now goes through the protocol intact, and a declaration with a missing or wrong version is refused. The two-second shutdown has not been run against a stuck process. The window has not been looked at again since the protocol and panel changes.

With this entry, both repositories are committed and pushed to GitHub for the first time. The team repository's `origin` now points to `Inpainter-Team`.

## Next steps

All of these are proposed and need the owner's agreement:
- **Simple Sketch inside the host,** the next milestone. Before any code, two things come to the owner. First, the systems contract for how the host's live copy of a sketch follows the document the sub-application owns. Second, a visual reference for a stopped or unresponsive sub-application, H-2.
- **Choosing which program to start.** The host still starts `demo` by name, and needs a way to know which sub-application to start.
- **A window icon.** The icon is still Simple Sketch's pencil until the owner chooses one.
