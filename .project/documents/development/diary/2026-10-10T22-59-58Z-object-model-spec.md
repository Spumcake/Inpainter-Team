# From one canvas to an object model, on paper

**UTC:** 2026-10-10T22:59:58Z
**Covers:** since [the shell entry](2026-10-10T21-05-19Z-pictured-shell.md).
**Repositories:** Team `master` is at `4403a22` (Record the diary from the shell through the polish.), the same as `origin/master` at https://github.com/Spumcake/vs-team. The document edits below, the new renders, and this entry are uncommitted. App `vector-sketch` `dev` is clean at `7920c85` (Add the pictured shell and Wayland pen pressure.), the same as `origin/dev`.

## What we did

After the last entry, both repositories were pushed, as the owner asked. The owner then added eight renders to `.project/renders/ui/` and renamed `ui.png` to `base-ui.png`. The new renders show:
- brush, eraser, rectangle, ellipse, text, and palette-color properties in a right-hand panel;
- the select tool with a Layers outliner and its own context actions.

One render was added twice by mistake. The owner later kept it as `selected-object-and-outliner-ui.png`.

The owner asked how to add objects, layers, grouping, and a properties panel while keeping the app data driven. One agent, Claude Code, read the renders and the code and answered in conversation. It proposed:
- an object tree;
- shared style components;
- one property schema behind one generic panel;
- tool and command registries that build the toolbar, shortcuts, and menus;
- a few generic undo steps.

It also listed where the renders conflict with each other and with the specification.

The owner then fixed the palette model. Colors own their value, stroke size, style, and layer order, and the order is sortable. Strokes live on canvas objects shown in the outliner, and each canvas has its own palette. A canvas's box is its drawn area plus padding. The brush works only on a highlighted canvas. The owner left the remaining choices to the agent.

The agent made those choices and rewrote the documents. [INTERFACE.md](../../../../INTERFACE.md), [the pitch](../../PITCH.md), [systems.md](../systems.md), and [tasks.md](../tasks.md) changed, and tasks gained SS-8, SS-9, and SS-10. [testing.md](../testing.md) and [AGENTS.md](../../../../AGENTS.md) were updated too. The previous plan and queue are in [the object-model archive](../archive/2026-10-10T22-48-33Z-object-model/plan.md). The agent also corrected its own claim that the team repository had no remote.

## What we learned

Some render details could not be adopted as drawn:
- Partial-opacity erasing and brush flow do not fit the polygon-difference ink, so they wait.
- The color panel's Join control has nothing to act on in freehand ink.
- The outliner render drops the tools and zoom from the toolbar.

Limiting the eraser to canvases means text never needs glyph outlines. Because a color owns stroke size, the Size control restyles every stroke in the active color. If a color later grows, ink can show beyond the edge of an older erasure.

## Where things stand

No code changed, and no tests ran; this was document work. The app still has the flat mark list from `7920c85`. The documents now describe a model it does not implement. The earlier open questions are settled in the specification, pending the owner's review: Fit returns, the three icons are New, Open, and Save, and the default palette has nine colors.

## Next steps

These are proposed and await the owner:
- Review the choices the agent made where the owner had no preference. The queue blocks implementation until they are accepted.
- Then task-operations replaces the mark list with the document tree and registries, the first slice in systems.
- Committing and pushing these documents and renders is the owner's call.

The XP-Pen check is still the owner's.
