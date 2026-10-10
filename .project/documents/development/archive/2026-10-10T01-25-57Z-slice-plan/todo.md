# Simple Sketch todos

- [ ] Have the branch-manager add `src/vector-sketch/worktrees/dev` on `dev` once https://github.com/Spumcake/vector-sketch has a commit on `main`. The bare clone is already at `src/vector-sketch/worktrees/.bare`. The stroke slice waits on that checkout.
- [ ] Have task-operations implement the stroke model and infinite canvas in that `dev` checkout, so a person can draw, erase, undo, pan, zoom, and fit on one sketch. This depends on that checkout. It covers SS-1, SS-2, SS-3, and the pen and erase parts of SS-4 in [tasks.md](tasks.md).
- [ ] Have verification run the stroke and camera journey on that preview after the slice builds, and record the result in [testing.md](testing.md).
