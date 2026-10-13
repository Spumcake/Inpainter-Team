# Simple Sketch plan

The interface now covers canvas objects whose palette colors are layers, a properties panel, a Layers outliner with groups, and filled shapes and styled text. `dev` is `7920c85`, also on `origin/dev`, with the stroke canvas, the pictured shell, and the Wayland pen path. It still has the earlier flat mark list, so the work below starts by replacing that model. The previous plan is [archived](archive/2026-10-10T22-48-33Z-object-model/plan.md).

## Document tree and registries

This comes first, because every later milestone edits the objects it defines. The mark list becomes the object tree from [systems.md](systems.md), and a new sketch starts with Canvas 1 and the default palette. Toolbar labels, icons, shortcuts, and hover text come from the tool and command registries. The pen becomes the brush, New sketch and Fit are wired, and the swatches show the highlighted canvas's palette. What a person sees barely changes, and drawing, erasing, undo, tabs, and the pen path keep working. It is complete when [SS-1](tasks.md), [SS-2](tasks.md), [SS-3](tasks.md), and [SS-5](tasks.md) hold on the new model, with SS-8's layer order and color restyling covered by tests.

## Properties panel

The schema, the panel, and the preferences store come next, starting with the brush, eraser, and color subjects, which reuse each other's sections. Palette editing, dashed and flat-cap ink, stabilization, and the pressure curve arrive here. It is complete when [SS-8](tasks.md) and [SS-9](tasks.md) hold for those subjects.

## Select, outliner, and groups

Selection, transforms, the select tool's context actions, and the Layers outliner come after the panel, because Layers is a panel subject. New canvas, group, and ungroup arrive here. It is complete when [SS-10](tasks.md) holds except the clipboard.

## Shapes and text

Shapes with `kurbo`, then text with Inter bundled. These two can proceed side by side once select has landed, with one writer for the shared document file. This is complete when [SS-6](tasks.md) holds and their tool and object panels work.

## Clipboard and saving

The clipboard completes SS-10. Snapshots and preferences persistence complete [SS-7](tasks.md) once every object kind exists, so the snapshot schema is written once.

No release version is agreed, so this plan does not include a candidate. Pen pressure on the owner's XP-Pen is still unchecked on a device.
