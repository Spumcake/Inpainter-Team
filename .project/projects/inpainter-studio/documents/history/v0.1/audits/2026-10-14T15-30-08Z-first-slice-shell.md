# First slice: host shell and empty demo

**UTC:** 2026-10-14T15:30:08Z
**Scope/baseline:** Inpainter version 0.1, the host shell with the empty demo, judged against the first-slice paragraph of H-1. App checkout `src/inpainter/worktrees/dev` at `2595cfb` ("Start the Inpainter repository."), dirty: the slice is untracked on top of that commit, and `README.md` is modified. Nothing has been pushed. GitHub's Inpainter repository still has no commits.
**Outcome:** The first milestone is built and the running window was checked. It is not committed.

## Goal and participants

The owner asked for the agreed first slice, checked against what running it shows, and left runnable. The coordinator assigned the work. The technical planner wrote the first-slice protocol in `systems.md`. The branch-manager created local `main` and `dev` because the GitHub repository was empty, then added `work/shell` and `work/demo` from `2595cfb`. Two task-operations workers ran at the same time: one wrote `protocol/` and `apps/demo/` on `work/demo`, the other carried the shell into `host/src` and `host/assets` on `work/shell`. A third task-operations worker integrated those trees in the dev worktree and wrote the workspace manifest, `host/src/process.rs`, `host/src/main.rs`, and the README. Verification and code review then ran together. No merge commit exists: the work branches were never committed, so both merges reported already up to date, and the files were copied into the dev worktree instead.

## Attempts and result

The host starts `demo` from the same directory as its own executable, reads `tab demo Demo`, `contents demo grid`, and `end`, and draws that shell. Menu, plus, and the tab close mark do nothing. The panel tab opens and closes an empty column. If the demo process is gone, the host stays up and does not draw a status; that picture is H-2.

Code review asked for a quieter close mark. That change was built and captured, and the mark was lighter than `shell-demo.png` (about rgb 199 against the picture's rgb 84–117). It was reverted to the darker mark, which was the one in the capture that matched the picture. Review also asked to remove the README. It stays, because the owner asked for run instructions, and the command that was actually run is recorded in `tasks.md`.

## Evidence and limits

`cargo build` and `cargo test` passed in the dev worktree. The three tests were `the_title_bar_is_left_to_the_desktop_theme`, `the_panel_tab_toggles_the_panel_without_drawing`, and `f4_opens_the_panel_and_narrows_the_canvas`. Verification launched `cargo run -p inpainter` on the Wayland session, captured the window, and clicked the panel tab, menu, plus, and close mark. The host stayed up. The title was Inpainter and the tab was Demo. The client area was 1280 by 800, larger than the window inside the picture file. The final tree was not captured again after the close-mark revert; that revert restores the line from the passing capture. H-2, H-4, and H-5 were not run. No commit was requested, so the slice is not on a commit past `2595cfb`.

## Next action

The next milestone, Simple Sketch inside the host, is not started.
