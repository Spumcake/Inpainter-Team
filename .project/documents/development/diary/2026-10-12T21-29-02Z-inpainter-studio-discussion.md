# Inpainter Studio, a discussion

**UTC:** 2026-10-12T21:29:02Z
**Covers:** since [the diagram connector entry](2026-10-12T18-57-33Z-connector-idea.md).
**Repositories:** Team `master` is at `c4ec6d4` (Record the diagram connector idea in the diary.), the same as `origin/master`, with this entry uncommitted. App `vector-sketch` `dev` is at `0c1d3b7` (Shape the text box and fix the owner's review of the toolbar.), the same as `origin/dev`, and clean.

**Status: experimental.** Everything below was talked through, not decided. The owner's statements here are working positions, not decisions. The same holds for the diagram connector idea in the previous entry; see the correction at the end.

## What we did

No code or project document changed. The owner and Claude Code talked about the app's future.

**Old Inpainter.** The owner asked about Inpainter, in `/mnt/Hot/Source/old/inpainter`. Claude Code read it: three rebuilds, from a Python backend with several web front ends, to a layered agent, CLI, Lua and Python design, to a TypeScript restart with a headless animation core. The owner explained the history:
- It started over a year ago as a personal infinite-canvas alternative to Photoshop.
- It kept growing into an AI-native video production platform, and the drawing app never arrived.
- Simple Sketch was split out to escape that. Still missing: shortcuts, saving, settings, image import, and build and install.

**A new direction, floated.** The owner proposed dropping old Inpainter and renaming Simple Sketch to Inpainter Studio. Ideas from there:
- Installable workspaces, each versioned separately, under a shared shell.
- An agent harness that discovers what any workspace offers, with skills supplied by each workspace.

They shared a mockup: an Assistant panel on the left editing a dashed pink circle, beside today's toolbar and color page.

Claude Code noted that the registries and property schema already look like an interface an agent could use. It first suggested building workspaces into one program. The owner rejected that as a monolith. They said separation of concerns is where agent-built work has gone wrong before.

**Working positions after discussion:**
- **Two kinds of program.** A shell program, plus a separate program for each workspace.
- **Shared fundamentals.** Drawing, erasing, shapes, text and ordering stay with the shell and a shared drawing library, so live pen input never crosses between programs.
- **What a workspace owns:** how drawings are arranged and used, plus saving. One infinite drawing for the canvas; drawings in frames on a timeline for an animatic.
- **Names.** The canvas workspace keeps the name Simple Sketch.
- **Undo.** Undo lives with each workspace.

**Custom views.** The owner asked how a workspace could add a new component like a timeline. Claude Code described a way to do it:
- The shell passes mouse and keyboard input for an area to the workspace.
- The workspace runs its own egui there and sends back triangles to paint.
- Simple controls (tools, menus, panel rows) stay as declared data that the shell draws.

## What we learned

- **egui 0.36.2 supports this.** Claude Code checked the local copy. With the `serde` feature, egui's input, its meshes, and its texture changes can be serialized. Its higher-level drawing commands cannot, so a workspace would convert to meshes before sending.
- **Splits like this can fail.** The xi editor's core and frontend split was later judged more complex than it was worth. Neovim's works because one side owns all the state.
- **Boundaries need structure.** Agents take the shortest path to their checks. Boundaries hold only if the code structure enforces them: separate crates and programs, plus a test that drives a workspace with no shell running.

## Where things stand

Both repositories are as stated above. None of this appears in the pitch, systems, plan, or queue.

## Next steps

All proposed, none agreed. Each is the owner's decision:
- Whether to take this direction at all, and whether and when to rename. Renaming before snapshots ship would avoid moving saved data later.
- A short experiment: a throwaway separate program drawing a scrubbable frame strip inside the current shell, to test responsiveness.
- A boundary contract beside `systems.md`, drafted only if the direction holds.
- Finishing the v1 list either way.

## Correction to the previous entry

[The connector entry](2026-10-12T18-57-33Z-connector-idea.md) calls the node editor toggle "settled by the owner" and the outlines "the owner's direction." Those were the owner's preferences in an exploratory discussion, not locked decisions. Diagram connectors as a whole remain an idea.
