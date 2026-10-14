# Inpainter 0.1 todos

- [x] Agree what the first slice shows when the host runs. The owner set it on 2026-10-14, in H-1 in [tasks.md](tasks.md), with [shell-demo.png](../../../renders/ui/shell-demo.png) as its picture. The reuse map was updated to match.
- [x] Write systems for the first slice from the reuse map. The contract is in [systems.md](systems.md): the host starts `template`, reads its tab and grid declaration, and shuts it down when the window closes.
- [x] Build that slice in the Inpainter repository. From `src/inpainter/worktrees/dev`, `cargo build` then `cargo run -p inpainter` opens the window. The check is in [the audit](../../history/v0.1/audits/2026-10-14T15-30-08Z-first-slice-shell.md). It is committed on `dev` as `8771bc8` and pushed to GitHub.
- [x] Rename the demo to the template, register sub-applications by manifest, and write the scaffolding skill, as the owner set on 2026-10-14. The host finds `apps/<id>/manifest.json` and starts the app `--app <id>` names. The skill is `skills/scaffolding/SKILL.md` in the Inpainter repository, and its `AGENTS.md` points to it.
- [ ] Test the scaffolding skill: a fresh agent, given only the Inpainter repository, scaffolds a throwaway sub-application with it. Fix the skill from whatever goes wrong, then remove the throwaway app.
- [ ] Scaffold Simple Sketch with the skill, so the host shows an empty Simple Sketch tab, then map what carries over from `src/simple-sketch` before any of its code moves.
