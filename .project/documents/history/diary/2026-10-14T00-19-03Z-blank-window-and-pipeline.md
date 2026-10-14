# The blank window and the pipeline

**UTC:** 2026-10-14T00:19:03Z
**Covers:** since [Inpainter begins](2026-10-13T20-50-01Z-inpainter-begins.md).
**Repositories:** The team repository is on `master` at `fe2ad92` (Organize version 1.0 documents together and close the version with a post-mortem.). Its `origin` still points to `simple-sketch-team`. Uncommitted: the pipeline changes below, the Simple Sketch documents archived under `.project/.simple-sketch/`, and `.project/documents/history/`. `src/inpainter/worktrees/` is empty, and the GitHub repository `Spumcake/Inpainter` is empty. `src/simple-sketch` is clean at `887f6f4` (v1.0.1).

This entry records a failed first slice, how it was traced, and the pipeline changes that followed. The documents and code it describes were deleted at the owner's request. What is said about them comes from Claude Code's inspection before the deletion.

## What we did

**Preparation and the first slice.** The owner asked the Cursor team, working under the development rule, to do three things:
- Say how it would pursue the founding entry's goals.
- Treat version 0.1 as only the separation of host and sub-applications, with the chat assistant out of scope.
- Prepare 0.1 for implementation, then plan it.

The team wrote a research brief, a v0.1 pitch, tasks, systems, a plan, and a todo list. It rewrote the root `AGENTS.md` and `INTERFACE.md`. The team got stuck on how much of Simple Sketch the first slice needed. So the owner told it to build a demo sub-application instead, so the host shell could be tested on its own.

The slice it delivered was two programs in an `Inpainter` checkout, about 1,050 lines in all:
- `inpainter`, a host whose window was filled with plain white.
- `demo`, a child process that completed a handshake, declared nothing, and drew nothing.

Verification could not find the window on the Wayland session and left the window check to the owner. Nothing was committed or pushed.

**The owner's review.** The owner ran it and saw a blank window. There was no tab strip, no menu, and nothing else of the shell. The owner had said many times that the host shell draws common UI whatever sub-application is loaded, and that this is what makes it a shell. Claude Code's first explanation also missed this: it treated the missing tabs as one defect among several.

**The trace.** Claude Code compared file times with what each document said (times in UTC):
- 22:01: the v0.1 pitch called Inpainter "the Linux desktop window you open to sketch." The modular framework was gone.
- 22:14: `AGENTS.md`, the file every agent reads first, said "Inpainter is a Linux desktop window for sketching" and "The host builds its chrome from the declaration the sketch sends."
- 22:20 and 22:33: two plans were archived. The second came from the owner's question about rebuilding Simple Sketch, which led to the demo.
- 22:52 to 22:57: the host and demo code were written.
- 23:09: criterion D-1 was written: "One window opens, and the host has started one separate demo process." The audit's checks were the title, "the empty surface," and Ctrl+W. Neither mentioned tabs.
- 23:21: `systems.md` and `INTERFACE.md` gained the rule that the host owns the tab strip and tool area whenever the window is up.
- 23:29: six pipeline files gained clauses saying a blank window is never a valid trial. The records do not show who added either of these last two changes.

## What we learned

The demo switch did not cause the failure; it exposed it. Before any code existed, the documents already said the host's chrome comes from the sketch. A child with no declarations therefore produced no UI, as documented.

The pipeline allowed this in five ways:
- **The founding vision lived only in a diary entry.** Diary entries do not authorize work, and only the coordinator reads the latest one.
- **The pitch was per version.** With the assistant cut from 0.1, nothing in it belonged to the host.
- **The coordinator could rewrite the identity paragraph in `AGENTS.md` without asking the owner.**
- **The rule about the host's tabs reached a contract that the criteria and checks never cited.**
- **There was no step where the owner agreed to what a slice would show before it was built.**

Claude Code's founding entry contributed. It said "The host draws simple UI from those declarations" and listed only declared UI and custom views, never the host's own common UI.

## Where things stand

At the owner's request, everything made for Inpainter after the founding entry was deleted:
- the v0.1 documents and their archive;
- the Inpainter checkouts and local repository;
- the Inpainter versions of `AGENTS.md` and `INTERFACE.md`, which were restored to their committed Simple Sketch versions.

The six "blank window" clauses in the pipeline were kept. So was the agent's copy of the Simple Sketch `INTERFACE.md` in the archive.

The pipeline now divides project documents by authority, and stays project-agnostic:
- **Mission.** `concept/MISSION.md` is owned by the owner and outranks everything else. Every agent reads it, and every assignment links it. A new **mission** skill drafts it or proposes a change, and saves only what the owner agrees to.
- **Development.** `development/<version>/` holds the version in progress. Its pitch says what that version delivers toward the mission and cannot redefine the product. Leaving something out of a version defers it rather than removing it.
- **History.** `history/` holds the diary as one stream, and each version's audits, archive, and post-mortem. It is only added to and is never an instruction.
- **The slice sentence.** Before building anything the owner will try, the coordinator writes what the owner will see and do, checks it against the mission, and waits for agreement. Verification judges the slice against it.

The founding entry was moved into `history/diary/`, and its link to the Simple Sketch post-mortem was adjusted for the new location.

## Next steps

All of these are proposed and need the owner's agreement:
- Draft `MISSION.md` from the founding entry with the mission skill. It should state the shell's common UI explicitly: the window, the tab strip with a new-tab control, the menu, the tool area, the panel area, and the sub-application's status, present whatever app is loaded.
- Build the shell slice to an agreed sentence: a window with a tab strip showing one Demo tab, a new-tab control, a menu, and an empty tool area, with the demo running as a separate process behind it through a shared protocol crate.
- Commit the pipeline changes and the Simple Sketch archive.
- Point `origin` at `Inpainter-Team`.
- Write a new `AGENTS.md` once the mission exists.
