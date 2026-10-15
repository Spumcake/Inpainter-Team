# Inpainter 0.2 post-mortem

The owner confirmed version 0.2 complete on 2026-10-15, after testing it on their own machine: it met every requirement. The [pitch](PITCH.md) and the [task list](tasks.md) are the record of what the version set out to deliver.

Version 0.2 began on 14 October 2026, when version 0.1 was re-scoped and Simple Sketch inside the host moved here. Its plan was first committed at `6c6d629`, 22:59 UTC that day. It closed at 18:00 UTC on 15 October 2026, about a day later.

## What was built

Simple Sketch 1.0.1 is now a sub-application of Inpainter. The person launches only the host and sketches in it. Simple Sketch's session, registry, store, and preferences were moved whole into `apps/simple-sketch`, and it runs as its own program. It owns each sketch, its undo, and its saving, and keeps its preferences and recovery files in its own folder inside Inpainter's data folder. The host carries over Simple Sketch's canvas, toolbar, panel, outliner, tabs, widgets, tablet input, and dialogs. It draws all of them from what the selected tab's sub-application declares. It previews each gesture, sends it when it ends, and shows the document that comes back. The shared `draw` crate holds the document model both sides need.

The host runs every sub-application `--app` names in one window. Selecting a tab of another sub-application swaps the toolbar, menu, panel, and contents. When a sub-application exits or stops answering, the host stays up and shows the notice from the two images the owner approved. `sketch-client` drives Simple Sketch with no host and compares every reply and saved file with an in-process copy. The work is Inpainter `b942352` and `1cd410f`, and the team repository through `36e4fd8`.

## What the owner decided

On 14 October the owner set the line every later choice followed: the host shows and previews, and Simple Sketch commits and owns. The owner also chose to run several sub-applications at once in this version, and to keep the template empty. The owner approved [reuse.md](reuse.md) and [systems.md](systems.md), accepting that `draw` exports `apply` and `revert`, and approved the two status images.

On 15 October the owner asked for the blink on new marks to be fixed. The fix holds each preview until its answer arrives. The owner also removed the Simple Sketch 1.0 archive from the project, now that Simple Sketch is part of Inpainter. INTERFACE now cites captures of the running host, and the mission was updated to match. What departed from the plan is under "As built" in [systems.md](systems.md) and [reuse.md](reuse.md).

## Left outside this version

These stay part of the product and come later:
- The scripted checks in systems' verification seams: the boundary check (H-3), the two-sub-application fixture, the one-owner stand-in, and the check that compares carried-over code with its source.
- Moving the old window's canvas and tablet tests into the host. Until they move, Simple Sketch keeps the window-side helpers those tests call.
- The assistant, and any second creative app.

An app sending the host paint commands stayed out of scope.

## What went wrong and what was learned

The first attempt ran Simple Sketch's whole session inside the host and kept two undo histories. It passed its checks, and only the intent review caught it. It is kept, never to merge, on `scrap/v0.2-shared-session`.

The first build of the no-host slice wrote a new, small session instead of moving Simple Sketch's. A check that compares a program with itself could not tell the difference; the owner's review of the code did. After that, the owner moved the work from Cursor to Claude Code.

Moving the real session found real defects. Marking every step-returning method `#[must_use]` exposed 20 places where an undo step was silently dropped.

The blink came from reading the one-owner rule too literally: the host ended its preview before the answer came back. Tests on a virtual display could not reach a tablet or the desktop's file dialogs. One Save test opened a dialog on the owner's real screen. The owner's own test on their machine was the check that closed the version.
