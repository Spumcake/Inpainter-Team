# References first

**UTC:** 2026-10-14T12:10:56Z
**Covers:** since [the mission and the v0.1 documents](2026-10-14T01-15-31Z-v0.1-documents.md).
**Repositories:** Before this entry's commits, the team repository was on `master` at `fe2ad92`, with `origin` still pointing to `simple-sketch-team`. `src/inpainter/worktrees/` is empty, and the GitHub repository `Spumcake/Inpainter` is empty. `src/simple-sketch` is clean at `887f6f4`.

This entry records a second failed attempt at the host shell. That work was deleted at the owner's request. What is said about it comes from Claude Code's inspection before the deletion.

## What we did

**The build.** The Cursor team implemented the first slice, the host shell with an empty demo, in three new crates of about 1,850 lines. The host's `shell.rs` alone was about 870. Its audit at 02:38 said the milestone ran and was checked on the display.

**The owner's review.** The owner ran it and found a shell that was not Simple Sketch's chrome:
- **A new menu bar:** named commands, New Tab, About, and Quit, with an About dialog.
- **A right-hand panel always open,** where Simple Sketch's panel opens from a tab at its edge.
- **New colors.** The strip and tab colors were not the constants in Simple Sketch's `tabs.rs` and `toolbar.rs`.
- **Rewritten chrome.** It was written from scratch instead of carried over from Simple Sketch.

**The trace.** The coder built what the v0.1 `INTERFACE.md` said. The product designer had specified the word menu, About, Quit, and the always-open panel there. It closed its presentation section with: "These placements, colors, and type are this interface's choices. The owner has not approved them." Implementation went ahead anyway.

Three things let this happen:
- **No document named the reference for the shell.** INTERFACE pointed to Simple Sketch only for the canvas, tools, and properties. The mission named the shell's areas as plain nouns. Claude Code drafted that mission and did not name a reference.
- **The pipeline told roles to build to "comparable production software."** The shell research brief studied Firefox, Krita, and Visual Studio Code, and those conventions became the shell.
- **Nothing required carrying existing code over** instead of writing it again.

**The cleanup.** At the owner's request, Claude Code deleted all of it:
- the v0.1 documents, research briefs, and audit;
- the local Inpainter repository and its four worktrees, after stopping the host and demo that were still running.

It also restored the root `AGENTS.md` and `INTERFACE.md` to their Simple Sketch versions. The mission and the diary were kept.

**The pipeline.** Claude Code changed it, keeping it project-agnostic:
- **References outrank research and invention.** Every visible element and behavior traces to a reference or a dated owner decision, or it is not built. A document that records a choice the owner has not approved blocks implementation.
- **Research is limited.** The "comparable software" standard now applies only to features no reference covers. Research findings are suggestions, never scope.
- **Interface work needs a visual reference.** The product designer writes INTERFACE only for views with an approved visual reference. Without one, it returns the view to the owner. A generated image becomes a reference only after the owner approves it.
- **A new appropriation skill.** When a reference codebase covers the work, the technical planner writes a reuse map. Each piece the scope needs gets a source, a destination, and a label: move, move and edit, or wrap. Anything new needs the owner's approval. Implementers copy each file first, then make only the mapped edits.
- **Three gates before any slice:** approved visual references, an approved reuse map, and the owner's agreed sentence.
- **Checks against the reference.** Code review checks provenance. Verification compares a screenshot with the reference, or reports itself blocked.
- **References in the mission and AGENTS.** The mission template gained a References section, and the AGENTS template a "References come first" paragraph.

**The mission.** With the owner's approval, the mission gained a References section. Simple Sketch 1.0.1 is the reference for the shell and for sketching: its renders, its archived INTERFACE, and its code. The code is carried over, not rewritten.

## What we learned

Prohibitions cannot keep up with every way an agent can invent. The rule that holds is positive and closed: only what traces to a reference or an owner decision is built. The pipeline's own completeness rule, built to comparable tools, pushed the roles toward invention whenever a reference was not named.

## Where things stand

Nothing for Inpainter is built. The pipeline changes, the mission, and the Simple Sketch archive under `.project/.simple-sketch/` are committed with this entry's session. That archive holds the renders and INTERFACE the mission points to.

## Next steps

All of these are proposed and need the owner's agreement:
- Ask the Cursor team to prepare v0.1 again, starting from the mission. Appropriation of Simple Sketch should come before systems, with the reuse map and the shell's sentence brought to the owner before any code.
- Point `origin` at `Inpainter-Team` before pushing anything.
