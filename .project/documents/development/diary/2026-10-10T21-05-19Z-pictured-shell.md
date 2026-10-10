# The pictured shell, then a closer match

**UTC:** 2026-10-10T21:05:19Z
**Covers:** since [the first entry](2026-10-10T15-28-29Z-first-sketch.md).
**Repositories:** Team `master` tracks https://github.com/Spumcake/vs-team. `6059801` is on the remote. `105d728` (Record the pictured shell, Wayland pen pressure, and polish.) is local only. This entry is new. App `vector-sketch` `dev` is clean at `7920c85` (Add the pictured shell and Wayland pen pressure.), one commit ahead of `origin/dev`.

## What we did

After the first entry, the owner named the team remote and we pushed `master` there.

The owner settled the toolbar from the picture. They do not see copy and paste icons. The picture is the visual authority, except that the far-right maximize controls become one toggle. The three icons after the swatches stayed unnamed. The coordinator rewrote the plan and the queue. The product designer and the technical planner updated the interface and the systems contract. The previous versions are in [the shell-plan archive](../archive/2026-10-10T16-22-05Z-shell-plan/plan.md).

One implementer built the shell. The window shares one session and one frame, so there was no second implementer. A review found that zoom and the maximize toggle could scroll away, that only the tab name switched sketches, and that closing the window dropped a dirty sketch. Those were fixed, and verification checked the window on X11. [The shell audit](../audits/2026-10-10T17-59-54Z-pictured-shell.md) records that pass.

Before the shell, one agent outside the usual roles wired Wayland pen pressure through a patched `octotablet`. [The pressure audit](../audits/2026-10-10T15-48-17Z-wayland-pen-pressure.md) records it. The owner had not tried the XP-Pen.

That same agent later reviewed the shell against the picture, at the owner's request. Presses on the menu and the confirmation no longer draw. The toolbar rows share one vertical center, and the colors, grid, and title bar were adjusted. The owner looked at each round. That agent committed the app as `7920c85` and the team documents as `105d728`. [The polish audit](../audits/2026-10-10T21-01-28Z-shell-review-and-polish.md) has the detail. The owner then asked for this diary and for those local commits to be pushed.

## What we learned

Published `octotablet` bound a tablet protocol version it could not parse, and the Wayland window closed. A vendored patch caps that version. Device pressure is still untested. The first shell layout hid zoom in a scroller. A later pass kept it at the far right. The picture shows zoom out before zoom in, and the interface lists zoom in first. The polish audit left that difference standing.

## Where things stand

`dev` draws, erases, and shows the pictured shell. Each tab keeps its own ink and view. Nineteen tests passed in the polish pass, including three that a press on the menu or the confirmation draws nothing. The owner reviewed the polished window. Device pressure, per-tab selection in the window, and the light title bar on Wayland were not confirmed here. Shapes, text, selection, and saving are not built.

## Next steps

The owner asked to publish the local commits, which is why this entry exists. The queue still asks the owner whether Fit returns to the toolbar, what the three icons do, and whether the swatch row is 9 or 11. Those decisions belong to the owner. The icons stay inactive until named. The XP-Pen check remains the owner's. Shapes, then text, then selection, have not been authorized.
