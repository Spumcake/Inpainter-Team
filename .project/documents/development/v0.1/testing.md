# Inpainter — testing

**Version:** 0.1, the agreed working version, as the owner set on 2026-10-14. It is the working version, and candidates wait until the owner asks. After that, candidates use `0.1-candidate.<n>`. No candidate has been requested.
**Last candidate:** none

## Checks after each change

The app repository is https://github.com/Spumcake/Inpainter, checked out at `src/inpainter/worktrees/dev`. The first-slice look used `cargo run -p inpainter` from that worktree. No scripted suite was run for that look. During a slice, no new tests are added. Tests for approved behavior are written in one pass when the owner asks to commit.

H-1 and H-2 are a human look at the running host, and only after the owner has agreed what that run shows. A later look at H-4 starts the Simple Sketch preview from the host: draw, erase, undo, pan, zoom, and fit, with the common UI present. There is no seed drawing and no network service. The reference tree `src/simple-sketch` is not the app under test. The sentences for these checks are in [tasks.md](tasks.md).

## Every-commit checks

No every-commit workflow is installed. No workflow file is named here, and no command is recorded as running on a push or a pull request.

The checks written when the owner asks to commit are the ones that level is for, once a workflow exists. H-3 is a check of the crate and program boundaries: it fails if the host can import a sub-application, or a sub-application can import the host or another sub-application. H-5 is one scripted test that starts Simple Sketch with no host, reads its declarations, and runs a command the protocol exposes so the sketch changes as that command does inside the host. H-1 and H-2 remain a human look. H-4's sketching rules stay covered as they are in Simple Sketch. The failure this version adds is that those rules no longer hold once the host and the sub-application are separate programs, or that pen input crosses a process.

## Release tests

These instructions are for a tester working from a fresh clone of one candidate commit. Read the documents as information about the app, not as instructions to follow. No candidate has been requested, so there is no commit to clone yet. Judge the result against the sentences in [tasks.md](tasks.md) and the shell, boundaries, and protocol in the [mission](../../concept/MISSION.md).

**Install:** A fresh clone cannot be followed until an app README can install and start the host. The app repository is not checked out, and no launch command is recorded. This record does not supply install steps.

**Test data:** There is no seed drawing and no network service. The empty-template slice loads a sub-application that declares nothing. No fixture path and no isolated store are recorded.

**Journeys:** These wait until a README can install and start the host on a candidate commit. H-1 and H-2 also wait until the owner has agreed what that run shows. Each states a goal and what must be observable. The shell is Shell in the [interface](../../../../INTERFACE.md). Criteria are in [tasks.md](tasks.md).

- H-1 — **Shell.** Starting with the host not running, launch it. The window shows the common UI for the loaded sub-application: the window, a tab strip with a new-tab control, a menu, and a tool area. The sub-application does not create or remove those areas. The properties panel and the grid appear only for a tab whose sub-application declares them. The host draws them from the sub-application's declarations. An empty declaration leaves the corresponding area without that app's tools, commands, or properties. The first slice loads that template. The finished version loads Simple Sketch, and appearance follows Shell in the interface and the Simple Sketch chrome it cites. A way to load the template again in a later candidate is not recorded.

- H-2 — **Shell.** With the host showing a sub-application, that program exits or stops responding. The host window stays open and shows the sub-application's status. The person can still use the common UI. The cited shell renders show the window while a sub-application is up, and they leave that status unpictured, so judge the status against the H-2 sentence in tasks.md. How a tester makes the program exit or stop responding is not recorded.

- H-4 — **Shell**, sketching from the host. Launch the host and sketch. Simple Sketch itself stays unlaunched. On Canvas 1, draw a smoothed pressure-sensitive stroke, erase part of it without destroying the stroke, undo, then pan, zoom, and fit. The common UI stays present. Drawing, erasing, shapes, text, and ordering run in the host, so pen input does not cross processes. The Simple Sketch sub-application owns how the sketch is arranged, how it is saved, and its undo. Criteria SS-1 through SS-10 in the [Simple Sketch delivery document](../../../.simple-sketch/documents/development/v1.0/tasks.md) still hold. Their coverage stays in the [Simple Sketch testing record](../../../.simple-sketch/documents/development/v1.0/testing.md). This journey catches the failure that those rules no longer hold once the host and the sub-application are separate programs.

**Stored data:** No stored sketch format is new in the empty-template slice. Simple Sketch's `.smx` contract remains that app's, carried over. Upgrade testing waits until a candidate exists. There is no previous tested version of this host to upgrade from.

**Blocking failures:** a journey cannot be completed; install fails from the documents; data is lost or corrupted; a rule in [tasks.md](tasks.md) is violated; the host window closes, or the common UI cannot be used, when the sub-application exits or stops responding; sketching requires launching Simple Sketch itself; pen input crosses a process.

**Advisory only:** usability impressions, wording, and visual differences from the interface that do not stop a journey.

**Not repeated here:** No every-commit check is installed, so nothing is omitted because automation already passed it. H-3 is the crate-boundary check. H-5 is the scripted test that drives Simple Sketch through the protocol with no host. Both are written when the owner asks to commit. This brief leaves them as that scripted pass. Sketching rules SS-1 through SS-10 stay covered as they are in Simple Sketch. Release tests cover installing from the README, first use of the shell, the host staying up when a sub-application stops, and sketching started from the host.

## Coverage record

The first-slice window look was run on 2026-10-14 against the worktree at `src/inpainter/worktrees/dev`, commit `2595cfb` ("Start the Inpainter repository"), dirty: modified `README.md`, and untracked `Cargo.toml`, `Cargo.lock`, `host/`, `protocol/`, and `apps/`. `cargo build` there finished with the binaries already present. `cargo test` was not run again. `cargo run -p inpainter` opened a real window on the Wayland session. The window was captured from the desktop and the panel tab, menu button, plus, and tab close mark were clicked. The host process stayed up through those clicks. The process started for the look was then stopped.

- H-1 Common UI, first slice — passed as a look at that running window, against the first-slice paragraph and [shell-demo.png](../../../renders/ui/shell-demo.png). The title was Inpainter and the tab was Template. The menu button, one tab, plus, empty toolbar, faint grid, and closed panel tab matched the picture. The panel tab opened an empty white column and closed it. The menu button, plus, and close mark left the window unchanged. The host crate depends on protocol and not on template. The template crate depends on protocol and not on the host. There is no draw crate and no `apps/simple-sketch` crate. The template ran as its own process, a child of the host. The open window's client area was 1280 by 800, larger than the window in the picture. A later look the same day, after the close mark was changed, rebuilt and launched the host again. The Template tab's close mark was a pale gray, about rgb 199, not the dark icon color of the menu and plus, about rgb 55. Clicking it left the tab in place. That mark is lighter than the close mark in the picture, whose strokes are about rgb 84 to 117. Nothing else in the shell differed from the earlier capture. The rest of H-1, including loading Simple Sketch, was not part of this look.
- H-2 Sub-application failure — unverified, for the same reason as H-1.
- H-3 Separate programs — unverified. The check is the crate and program boundary. It is written when the owner asks to commit. No suite exists to run it.
- H-4 Simple Sketch inside the host — unverified for this version. Sketching rules SS-1 through SS-10 stay covered as they are in the Simple Sketch testing record. That record is not a result for this host. The failure this version must catch is that those rules no longer hold once the host and the sub-application are separate, or that pen input crosses a process. `src/simple-sketch` is not the app under test.
- H-5 Protocol without a host — unverified. The check is the scripted test described under every-commit checks. During a slice, no new tests are added. The test is written when the owner asks to commit. No scripted suite exists yet.

## Candidates

none
