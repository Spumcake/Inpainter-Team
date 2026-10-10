# Stroke canvas review and fixes

**UTC:** 2026-10-10T15:12:57Z
**Scope/baseline:** First milestone of Simple Sketch in `src/vector-sketch/worktrees/dev`, branch `dev` at `697f231`. The crate is still untracked, and nothing was committed or pushed. Follows [the stroke canvas audit](2026-10-10T02-37-47Z-stroke-canvas.md).
**Outcome:** Completed for the requested review and four fixes. The owner confirmed the anti-aliasing in the window.

## Goal and participants

The owner asked for an examination of the slice. They then reported slow, complex sketches and triangular fill artifacts, and asked for four fixes. One agent, Claude Code, did the review, the fixes, and the checks. No Cursor roles ran. The review was self-review.

## Attempts and result

The review found these defects:
- Every frame rebuilt every mark's outline, difference, and tessellation.
- Ink meshes had no anti-aliasing.
- egui 0.36 on winit 0.30.13 gives no pen pressure on Linux.
- The interface spec misreads the reference: New, Open, and Save are recorded as copy, paste, and cut, and the spec lists 11 swatches where the picture shows 9.

A scratch harness reproduced the fill artifacts on long, self-overlapping strokes, 60 of 60. The `perfect_freehand` outline was correct. `earcutr` filled its self-crossing ring wrongly, and only erased marks passed through `i_overlay`. Earlier probes with loops and zigzags did not reproduce it. The owner's hint about self-overlap did.

Changes:
- [sketch.rs](../../../../src/vector-sketch/worktrees/dev/src/sketch.rs) resolves every outline with `i_overlay` before earcut, including the stroke being drawn.
- [sketch.rs](../../../../src/vector-sketch/worktrees/dev/src/sketch.rs) also caches each mark's local shapes, triangles, and bounds. The cache is dropped when an erase or a text change is applied or undone. Bounds checks run before hit testing and eraser intersection.
- [canvas.rs](../../../../src/vector-sketch/worktrees/dev/src/canvas.rs) sets 4× MSAA.
- [session.rs](../../../../src/vector-sketch/worktrees/dev/src/session.rs) adds a seeded regression test for self-overlapping scribbles.

## Evidence and limits

`cargo test` passed 9 of 9. The new test failed with 295 stray cells when the fix was removed. Clippy reported only collapsible-`if` style warnings, in code these changes did not touch.

Per frame for 300 strokes, the release build went from 29 ms to 0.8 ms. With erases it went from 345 ms to 1.4 ms. The debug build still takes 18–33 ms.

The release build started headless under Xvfb with MSAA on. The owner judged the anti-aliasing in the window. The owner has not reported a re-check of the artifacts or performance. Pressure and the spec misreading are unchanged. [systems.md](../systems.md) and [tasks.md](../tasks.md) still describe the app checkout as absent.

## Next action

The owner decides the toolbar contents: the reference picture or INTERFACE.md. The owner also decides whether to commit `dev`. The pictured shell milestone in [plan.md](../plan.md) stays unauthorized.
