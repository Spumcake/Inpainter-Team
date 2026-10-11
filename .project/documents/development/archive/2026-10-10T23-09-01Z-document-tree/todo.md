# Simple Sketch todos

The previous list is [archived](archive/2026-10-10T22-48-33Z-object-model/todo.md).

- [x] The interface, pitch, systems contract, and delivery criteria describe canvases with palette layers, the properties panel, the outliner, and the object model. The renders are listed in [INTERFACE.md](../../../INTERFACE.md).
- [ ] The owner reviews the decisions this update made where they had no preference, listed in INTERFACE and systems. Implementation should not start until they accept them.
- [ ] Task-operations replaces the mark list with the document tree and registries in `src/vector-sketch/worktrees/dev`, as the first slice in [systems.md](systems.md). Drawing, erasing, undo, tabs, and the pen path keep working.
- [ ] Test pen pressure on the XP-Pen when more of the app is in hand. Thinning, alignment, the eraser end, and the hover cursor are still unchecked. See [the pressure audit](audits/2026-10-10T15-48-17Z-wayland-pen-pressure.md).
