# From the document tree to a standing panel chrome

**UTC:** 2026-10-11T15:17:22Z
**Covers:** since [the object-model entry](2026-10-10T22-59-58Z-object-model-spec.md).
**Repositories:** Team `master` is at `cd52824` (Record the properties panel, its correction, and the restyle.), two commits ahead of `origin/master`, which is still at `1f48e1d`. Uncommitted on top of that are the chrome edits in [INTERFACE.md](../../../../INTERFACE.md), [plan.md](../plan.md), [systems.md](../systems.md), and [tasks.md](../tasks.md), plus this entry. App `vector-sketch` `dev` is clean at `4398ea6` (Add the properties panel for the brush, eraser, and palette colors.), two commits ahead of `origin/dev` and not pushed.

## What we did

The owner accepted the object-model documents by asking for them to be committed and pushed, then for a plan of the first slice. That commit is `1f48e1d`. The slice was the document tree and registries. One task-operations worker wrote it. Verification and review ran together. The record is [the document-tree audit](../audits/2026-10-11T00-15-02Z-document-tree.md).

The owner then reported that a stroke joined its color only after release, and that the title bar stayed light on a dark desktop. They also asked for the clean-sketch rule and tighter toolbar padding. One agent, Claude Code, fixed those, tested, and committed the app as `6210d8e`. The team repository recorded that as `9949db9`. The record is [the document-tree review](../audits/2026-10-11T01-07-33Z-document-tree-review.md).

The owner asked what was newly testable, then agreed the next slice was the brush, eraser, and color panel. A task-operations worker started it. Cursor crashed before that pass returned. The files were still in the worktree, and the same role finished them. Verification rechecked F4 after a panic. The record is [the properties-panel audit](../audits/2026-10-11T01-53-00Z-properties-panel.md).

The owner then found the color page had been built from the extra color render. They supplied updated renders and asked for the title bar and close button to become a toggle tab. Claude Code corrected the page, restyled the panel, and committed the app as `4398ea6`. The team repository recorded that as `cd52824`. The record is [the panel review](../audits/2026-10-11T15-07-46Z-properties-panel-review.md).

After that, the owner asked for the chrome to persist across the app even where a render still draws the old header and card. The product designer updated the interface. The coordinator recorded the same rule in the plan, the acceptance criteria, and the panel contract. Those edits are the uncommitted files above.

## What we learned

The first clean-sketch failure was a click on the new-tab plus, not the tab's close mark. A later contradiction was real: systems had required the first color to be active for a sketch to count as clean, while also saying the active color does not dirty a sketch. The clean rule now ignores the active color.

The color page followed an earlier interface sentence that mixed two renders. It is now taken from `color-property-ui.png` alone. Join and the third cap option are dropped. Palette order is a swatch drag. The extra color render is only a future reference.

The panel's first F4 press panicked on a negative width. Undo during a slider drag could also commit over the history change. Both were fixed before the slice was called done.

## Where things stand

A new sketch is Canvas 1 with the nine-color palette. The brush, eraser, and color pages open with F4 or the edge tab. The panel has no Properties header, no close button, and no tool-name and description card, and that chrome is now written for shape, text, and Layers as well. Drawing, erasing, and tabs still work. Neither of the two app commits has been pushed.

Still unchecked in a live window, from the audits: pen pressure on the XP-Pen, settings after a restart, flat cap, Duplicate and Delete from the color menu, hiding or locking a color, and typing into number fields. Related shades still has no defined behavior.

## Next steps

The chrome rule is settled. Committing the uncommitted specification edits, and pushing `dev` and `master`, remain the owner's call.

Proposed, and not agreed: selection and the Layers outliner, the next milestone in the plan. Those pages are to use the same panel chrome even though the Layers render still draws a header and a close button.

Blocked on the owner: what Related shades does, and the XP-Pen check. Recording these does not start them.
