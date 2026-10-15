# Inpainter 0.2 todos

The previous list is [archived](../../history/v0.2/archive/2026-10-14T21-42-33Z-shared-session/todo.md).

- [x] Set the first attempt aside on `scrap/v0.2-shared-session`, reset `dev` to `a40e07c`, keep the scaffolding skill's fix, and scaffold Simple Sketch again.
- [x] Rewrite [reuse.md](reuse.md) and [systems.md](systems.md) on the line the owner chose on 2026-10-14.
- [x] Run the intent review on the rewritten map and systems. It found the plan's checks would let fakes pass. The owner chose on 2026-10-14 to run several sub-applications at once in 0.2 and to keep the template empty, and the checks were rewritten.
- [x] The intent review of the revised map, systems, and tasks passed with residual risks. The owner approved [reuse.md](reuse.md) and [systems.md](systems.md) on 2026-10-14, and accepted that `draw` exports `apply` and `revert`.
- [x] The owner approved both status images on 2026-10-14. They are in INTERFACE's Sub-application status.
- [x] Simple Sketch with no host, done on 2026-10-14 by Claude Code and uncommitted. The first build of this slice wrote a new, small session instead of carrying Simple Sketch's over, and passed its checks; the owner's review found it. Simple Sketch's real session, registry, store, and preferences were then moved whole, with every edit through the record, as [reuse.md](reuse.md) records under As built. All 88 of its own tests pass, and the no-host check passes against an in-process session, reply for reply and file for file.
- [x] Build the second slice, sketching from the host, and H-2, as [plan.md](plan.md) describes. Done on 2026-10-15 by Claude Code and uncommitted; [reuse.md](reuse.md) and [systems.md](systems.md) record what was built under As built.
- [ ] The owner looks at H-4, H-7, and H-2 in the running window: `cargo run -p inpainter -- --app simple-sketch`, and with `--app template` added.
- [ ] Check on the owner's tablet: pressure, and the stylus eraser end. The virtual display used for the hand run has no tablet.
- [ ] Check Save, Save As, and Open with the desktop's file dialog. The hand run could not open one.
- [ ] When the owner asks to commit: write the tests for the approved behavior in one pass, including the checks in systems' verification seams, and move the canvas and tablet tests with the host.
