# Inpainter interface

Inpainter is the host a person launches for creative work. This document describes the host's common UI, the shell on screen whichever sub-application is loaded. Product intent is in [the pitch](.project/documents/development/v0.1/PITCH.md). The assistant's chat panel comes in a later version. These screens are the host's common UI.

## Journey and sitemap

The person launches the host and lands on **Shell**. This version stays in that window.

- **Shell** is the host window: title bar, tab strip with its new-tab control, menu button, tool area, the area a sub-application fills, and that sub-application's status when it stops or stops responding.
- **Properties** is the properties panel open on the right of Shell, for a tab whose sub-application declares it. The update renders show it, with a tab on the panel's left edge. [base-ui.png](.project/.simple-sketch/renders/ui/base-ui.png) is Shell with that column absent.

The menu button is on Shell. None of the cited renders show the menu open.

## References

The shell's appearance is Simple Sketch's accepted chrome. Sketching tools, palettes, swatches, canvas objects, the grid, and drawing in these pictures stay with Simple Sketch, in `.project/.simple-sketch/INTERFACE.md`.

- [shell-demo.png](.project/renders/ui/shell-demo.png) — the owner's picture of the shell with the empty demo loaded, given on 2026-10-14. It shows the title bar, the tab strip with the menu button, one tab, and the plus, the empty white toolbar, the faint grid in the contents area, and the panel tab at the right edge. It reads "Untitled 1 — Simple Sketch" and its tab reads "Untitled 1". As the owner set on 2026-10-14, the title reads Inpainter and the demo's tab reads Demo.
- [base-ui.png](.project/.simple-sketch/renders/ui/base-ui.png) — the window, the tab strip, and the toolbar, with the panel closed.
- [update/brush-property-ui.png](.project/.simple-sketch/renders/ui/update/brush-property-ui.png), [update/eraser-property-ui.png](.project/.simple-sketch/renders/ui/update/eraser-property-ui.png), and [update/color-property-ui.png](.project/.simple-sketch/renders/ui/update/color-property-ui.png) — the panel open. Simple Sketch's interface accepts these three as the panel chrome.

The update renders are cropped above the tab strip. The title bar is only in base-ui.png. These are existing Simple Sketch renders, inspected here for the host shell.

## Presentation

base-ui.png shows a light, compact window with rounded corners and a soft shadow. The title bar is at the top. Under it, a pale strip holds the menu button, the tabs, and a new-tab control. The active tab is white and joins the white toolbar. Inactive tabs are paler. In the update renders the open panel is a white column on the right, with a thin line at its left edge and a small tab on that edge.

## Shell

Shell is the screen the person gets by launching the host. The host keeps this frame for every sub-application, including one that declares nothing. A sub-application fills the tool area and the area under the toolbar, and declares the shared components it uses there, such as the properties panel and the grid.

In [base-ui.png](.project/.simple-sketch/renders/ui/base-ui.png) the window has a title bar, the tab strip, the toolbar, and a large area beneath. Minimize, maximize, and close sit at the right of the title bar. At the left, that picture shows a small orange pencil and the words Simple Sketch. That icon and those words are the reference app's.

The tab strip is the pale bar under the title bar. A menu button of three horizontal lines is at its left, then the tabs, then a plus control. base-ui.png shows three tabs, each with a close mark: Untitled 1, Notes, and Sketches. Those are the reference's document names. Untitled 1 is the active tab, white, and its lower edge meets the toolbar. Notes and Sketches are paler, with a light border. The update renders show a single tab, Untitled 1, with a close mark, then the plus.

The menu on these screens is that button. The renders show the button and include no open menu.

The tool area is the white bar under the tabs. Thin vertical lines divide it into groups. In these renders the groups are Simple Sketch's color swatches, file commands, undo and redo, drawing-tool icons, a Size control, and the zoom controls at the right. [update/eraser-property-ui.png](.project/.simple-sketch/renders/ui/update/eraser-property-ui.png) shows a hover label, Brush (B), on the brush icon. Those contents belong to sketching. The bar is the host's tool area.

Under the toolbar is the area a sub-application fills. In base-ui.png it runs to the right edge of the window: a faint square grid with sample drawing. In the update renders the same gridded area stops where the panel begins. The grid and the drawing belong to sketching.

The properties panel is a shared component of the host, as the owner set on 2026-10-14. The host draws it, always the same way, only for a tab whose sub-application declares it. The sub-application declares whether a tab has the panel, which side it sits on, and what it holds. Only the right side has a reference, so only the right side is built. Without it, there is no panel tab and F4 does nothing. The panel starts closed. While it is closed, the same tab sits at the canvas's right edge, as the owner set on 2026-10-11 in [the archived interface](.project/.simple-sketch/INTERFACE.md). Clicking it shows or hides the panel, and the chevron points the way the panel will move.

Each tab is a document its sub-application reports. Its label is the name of the file that sub-application manages or, when it manages no file, the sub-application's name. The menu's contents are the commands declared by the sub-application of the selected tab. The title always reads Inpainter, whatever tab is selected, as the owner set on 2026-10-14. The faint grid is a shared component the host draws in the contents area when the tab's sub-application declares it.

When a sub-application stops or stops responding, the host stays up and shows that status. The cited renders do not show that status.

## Properties

Properties is the properties panel open, as in the three update renders. It is the host's, and it appears only for a tab whose sub-application declares it.

The panel is a white column on the right, below the toolbar. A thin line runs down its left edge. A small white tab joins that edge near the top and overlaps the area beside it. The chevron on the tab points right. While the panel is closed, that same tab sits at the canvas's right edge, as the owner set on 2026-10-11 in [the archived interface](.project/.simple-sketch/INTERFACE.md). Clicking it shows or hides the panel, and the chevron points the way the panel will move. The column has no close control and no title of its own. Each render starts the column at a section heading. Thin lines separate later sections. The sections in these pictures are Simple Sketch's property pages: brush settings, eraser settings, and one color's settings. Those pages are sketching. The column, its left edge, and the tab are the panel chrome.

## Demo

Demo is Shell with the empty demo loaded, as in [shell-demo.png](.project/renders/ui/shell-demo.png), as the owner set on 2026-10-14. The title bar reads Inpainter. The tab strip holds the menu button, one tab labeled Demo with its close mark, and the plus, drawn as in the picture. The demo declares no menu rows and no new-tab command and has only that tab, so clicking the menu button, the plus, or the close mark does nothing: no popup, no new tab, no close. The toolbar is the white bar, empty, with its hairline. The contents area shows the faint grid, which the demo declares, and nothing draws on it. The demo declares the properties panel on the right, so the panel tab sits at the right edge, and opening it shows the empty white column. Nothing else happens.
