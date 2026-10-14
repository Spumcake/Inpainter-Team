# Inpainter 0.1 todos

- [x] Agree what the first slice shows when the host runs. The owner set it on 2026-10-14, in H-1 in [tasks.md](tasks.md), with [shell-demo.png](../../../renders/ui/shell-demo.png) as its picture. The reuse map was updated to match.
- [x] Write systems for the first slice from the reuse map. The contract is in [systems.md](systems.md): the host starts `demo`, reads its tab and grid declaration, and shuts it down when the window closes.
- [x] Build that slice in the Inpainter repository. From `src/inpainter/worktrees/dev`, `cargo build` then `cargo run -p inpainter` opens the window. The check is in [the audit](../../history/v0.1/audits/2026-10-14T15-30-08Z-first-slice-shell.md). The work is uncommitted, and GitHub still has no commits.
