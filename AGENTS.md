# AGENTS.md — Simple Sketch

Simple Sketch is a desktop app for sketching and annotating with a pressure-sensitive stylus on an infinite vector surface of canvas objects, shapes, and text. It is a native Rust program using egui through eframe, with Iconoir toolbar icons. Scope and acceptance are in `.project/documents/development/tasks.md`.

**Language:** English for code, comments, documentation, logs, commit messages, and generation prompts.

## 1. Starting and resuming work

Read this file, the current milestone in `.project/documents/development/plan.md`, and the relevant items in `.project/documents/development/todo.md`. The coordinator also reads the latest three audits in `.project/documents/development/audits/` and the latest entry in `.project/documents/development/diary/`, if any. Workers read their assignment and only the context it links.

Before a change, read the related criteria in `tasks.md` and the contracts in `.project/documents/development/systems.md`. Before visual work, read `INTERFACE.md` and the renders it lists in `.project/renders/ui/`. Intent is in `.project/documents/PITCH.md`. A missing plan or todo does not authorize creating one.

## 2. Scope and autonomy

Work toward the authorized outcome. Resolve routine choices inside that scope. Surface a decision that would change the product or go past it. A question does not authorize edits. Stop when the assignment is done, or name the blocker. Continue only when the user has authorized the next task.

## 3. Orchestration

The coordinator assigns bounded work and integrates the results. Independent work can run together once shared contracts are settled, with one writer per file. Workers return the result, the checks they ran, and any blocker.

The app repository is `vector-sketch` at https://github.com/Spumcake/vector-sketch. Its bare clone is `src/vector-sketch/worktrees/.bare`. Local `dev` is checked out at `src/vector-sketch/worktrees/dev` on `7920c85`, the same commit as `origin/dev`. Concurrent writers each get a `work-<name>` worktree from that same `dev` commit. Project documents stay in this team repository, at https://github.com/Spumcake/vs-team.

The app is data driven. A tool, command, shortcut, property, or panel section is an entry in the registries and property schema that `systems.md` describes. The toolbar, panel, outliner, menus, and hover text are built from those entries. Do not hard-code a label, icon, key, or per-tool widget, and reuse an existing section before adding one.

## 4. Plans and records

The plan is the milestone order. The todo list is the current queue. The coordinator uses the plan and todo skills when authorized, and updates them when progress or a blocker changes. A settled decision goes into the document that owns it. At a milestone boundary, or when coordinated work stops blocked or abandoned, the coordinator writes one audit with the audit skill and preserves earlier audits. On request, the diary skill records everything since the previous diary entry and the next steps discussed, which do not authorize work.

## 5. Verification

The first journey is a new sketch: draw a smoothed pressure-sensitive stroke on Canvas 1, erase part of it without destroying the stroke, undo, then pan, zoom, and fit. Commands are in `tasks.md`. Check levels are in `.project/documents/development/testing.md`. Use the tests skill when writing, changing, or reviewing tests. Cover the failure that matters. Report what was checked and what was not.

A preview judges the window against `INTERFACE.md`. A unit test judges session, document, registry, and schema rules. Do not snapshot meshes or treat the reference drawing as built-in content. Commits happen only when the user asks, and only the branch-manager changes Git state. No hook or workflow is installed. Verification runs a requested acceptance journey and does not repeat the unit suite as a routine. Document-only work needs no application run.

## 6. Boundaries

Stay with the dependencies named in `systems.md`. Do not add Electron, Tauri, a web target, accounts, sync, or collaboration, and do not fork another application. Saving uses the private snapshot contract there. Do not invent a published file format.

Keep secrets out of version control. Do not push, tag, or recommend a release. No release version is agreed. The renders listed in `INTERFACE.md` are the visual reference.
