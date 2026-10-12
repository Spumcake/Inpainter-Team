# Diagram connectors, an idea

**UTC:** 2026-10-12T18:57:33Z
**Covers:** since [the text, palettes, and review entry](2026-10-12T18-38-11Z-text-palettes-and-review.md).
**Repositories:** Team `master` is at `a1e0fcb` (Record the diary from shapes through the toolbar and text review.), the same as `origin/master`, with this entry uncommitted. App `vector-sketch` `dev` is at `0c1d3b7` (Shape the text box and fix the owner's review of the toolbar.), the same as `origin/dev`, and clean.

## What we did

At the owner's request, Claude Code pushed both repositories. Team `master` went from `7335fe4` to `a1e0fcb`, and app `dev` from `94ad2c9` to `0c1d3b7`. Both were clean when pushed.

The owner then said the text tool is their favorite of its kind. They usually draw diagrams in draw.io and find it clunky. They asked what it would take to connect objects into a node diagram. Their idea is a toggle in the select tool that turns the transform handles into connection controls. They showed three draw.io screenshots:
- a selected box with blue arrows on its four sides and attach points along its edges;
- an elbow connector with an arrowhead running between two boxes;
- the same connection as a curve.

Claude Code answered with a design and an estimate. Nothing was built or written into the plan.

## The idea as proposed

**The connector**
- A new kind of object in the sketch tree.
- It stores what each end is attached to, not where the end is: an object and a point on its edge, or a free point.
- A route: straight, elbow, or curved.
- A palette of one Stroke color with size and dash, as the line tool has, and an arrowhead choice for each end.

**Following and drawing**
- Each end's position is worked out every frame from its object's bounds, as ink outlines already are. Moving or scaling a shape drags its connectors along with no extra undo steps.
- Drawing reuses the line tool's path and dash code; arrowheads are small extra shapes.

**Interaction**
- The owner then described how they picture it. A node editor mode is turned on from the select tool's actions by a button with Iconoir's `network` icon, placed after Flip. Iconoir has that icon, so it can be a registry entry like the other actions.
- While the mode is on, every object that can be connected shows the faint dashed outline the other tools already draw, so it is clear what can take a connector. This is the owner's direction, not a proposal.
- In connect mode, the selected or hovered object shows its attach points and four arrows. Dragging from one draws a rubber-band line.
- The object under the pointer is highlighted, and the end snaps to its nearest attach point. Releasing makes the connector in one undo step.
- A selected connector has handles at its ends to re-attach them, and one for an elbow's bend.

**Touch points**
- Hit testing by stroke.
- Outliner rows.
- What happens to a connector when its object is deleted.
- Connectors whose end is inside a group.
- The coming clipboard and snapshots, which must re-point copies and store each end's attachment. Building connectors before snapshots would put them in the saved format from the start.

**Stages**
1. Straight and elbow connectors attached at edge midpoints, with arrowheads, following moves, selection, delete, undo, and outliner rows.
2. Curves, re-dragged ends, and moving the bend.
3. Labels and more attach points, if wanted.

Routing around other objects, as draw.io does, was judged the hard part and left out. Claude Code put the first stage at about the size of the shapes slice, plus the work on those touch points.

## What we learned

The owner sees the app's text box as a reason to do diagrams here. A connector label would reuse that text box.

## Where things stand

Both repositories are pushed and clean. The connector idea exists only in this conversation and this entry. The pitch, plan, systems, and queue do not mention it.

## Next steps

These are the owner's decisions. One is settled; the rest await a choice:
- Scope. The pitch describes sketching and annotating; connectors extend the product. If agreed, they go into the pitch and the plan as a slice.
- Settled by the owner: node editor mode is a toggle in the select tool's actions, with the `network` icon after Flip, and it outlines every connectable object. Still open: whether the toggle stays on until turned off, and whether text, which has no rotate or flip, can be connected (the proposal assumed it can).
- Attach points: edge midpoints, a few fixed points per side, or anywhere on the outline.
- Whether deleting an object leaves its connectors with loose ends or removes them.
- Whether freehand canvases can be connected, given that their bounds change as ink is added.
- Whether connectors come before or after the clipboard and snapshots.

Proposed, not agreed: once those are answered, Claude Code writes the slice into the pitch, plan, and systems, then builds the first stage. The owner's live check of text and the text toolbar's width are still open in [the queue](../todo.md).
