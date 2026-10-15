# The template is committed and pushed

**UTC:** 2026-10-14T17:20:09Z
**Covers:** since [the first slice](2026-10-14T16-05-34Z-first-slice-shell.md).
**Repositories:** The team repository is on `master` at `5502a30`, clean, and that commit is on `origin/master`. Inpainter is on `dev` at `a40e07c`, clean, and that commit is on `origin/dev`. `src/simple-sketch` was not part of this work.

## What we did

The owner asked to commit the uncommitted work in both repositories. Claude Code had made it directly in Inpainter's `dev` worktree: the demo sub-application was renamed to the template, the host finds sub-applications from `apps/<id>/manifest.json` (`host/src/apps.rs` and `protocol::Manifest`), and the repository gained `AGENTS.md` and the scaffolding skill under `skills/scaffolding/`.

Before the commit, task-operations added five protocol tests and ran `cargo test`. All eight tests passed: the five new ones and the three shell tests already in the tree. No product code changed for the tests. The new tests catch a label with spaces being split, a declaration that skips hello or speaks another protocol version, and a manifest with a bad id, a path as its executable, or another protocol version.

The branch-manager then committed Inpainter `dev` as `a40e07c`, "Register sub-applications from their manifests." No pre-commit hook is installed, so none ran. The owner had approved removing `work-shell` and `work-demo`. The branch check classed both as integrated: neither branch had commits that `dev` lacked, and their drafts had already been copied into `dev`. Both worktrees were removed and both branches deleted. `main` and `dev` stayed.

The team repository was committed on `master` as `5502a30`, "Record the template and how the host finds sub-applications," with the trailer `App-Commit: a40e07c Inpainter`.

The owner then asked to push. Inpainter's `dev` moved on GitHub from `8771bc8` to `a40e07c`. The team repository's `master` moved from `4c8cab0` to `5502a30`.

## What we learned

The earlier proposal that the host still started a program named `demo` is overtaken by this commit. The host now chooses a sub-application from its manifest. The leftover worktrees held nothing beyond what was already in `dev`, including after the demo folder had been renamed to `template`.

## Where things stand

Both working trees are clean, and both commits are on GitHub. The five protocol and manifest tests are in `a40e07c` with the rest of that work. The first slice's window check remains the one in [the audit](../v0.1/audits/2026-10-14T15-30-08Z-first-slice-shell.md). This stretch did not open the window again.

## Next steps

Nothing new was agreed. Still proposed, and still waiting on the owner, from the previous entry: Simple Sketch inside the host, which needs a systems contract for how the host's live copy follows the sub-application's document, and a visual reference for a stopped or unresponsive sub-application. A window icon of Inpainter's own is also still unchosen. Recording those here does not start them.
