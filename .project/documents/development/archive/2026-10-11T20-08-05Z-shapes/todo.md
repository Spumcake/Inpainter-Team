# Simple Sketch todos

The previous list is [archived](archive/2026-10-11T15-32-10Z-selection-outliner/todo.md).

- [x] Selection, transforms, the context actions, and the Layers list are committed on `dev` as `5567860`. Drawing, erasing, and the color page still work. See [the audit](audits/2026-10-11T16-31-33Z-selection-layers.md) and [the review](audits/2026-10-11T19-54-52Z-selection-layers-review.md).
- [x] The window journey passed: pick, move, group, hide, and the Layers chrome. The result is in [testing.md](testing.md).
- [ ] Say which pointer position moves a row into or out of a group. Sibling reorder works. The cross-parent drag waits on that rule.
- [ ] Decide how the select row fits the default 1280-point window: a wider window, a tighter toolbar, or scrolling.
- [ ] Test pen pressure on the XP-Pen when more of the app is in hand. Thinning, alignment, the eraser end, and the hover cursor are still unchecked. See [the pressure audit](audits/2026-10-10T15-48-17Z-wayland-pen-pressure.md).
