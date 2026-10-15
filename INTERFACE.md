# Inpainter interface

Inpainter is the host a person launches for creative work. This document describes the host's common UI, the shell on screen whichever sub-application is loaded. Product intent is in [the version 0.2 pitch](.project/documents/history/v0.2/PITCH.md). The assistant's chat panel comes in a later version. These screens are the host's common UI.

## Journey and sitemap

The person launches the host and lands on **Shell**. This version stays in that window.

- **Shell** is the host window: title bar, tab strip with its new-tab control, menu button, tool area, the area a sub-application fills, and that sub-application's status when it stops or stops responding.
- **Properties** is the properties panel open on the right of Shell, for a tab whose sub-application declares it. [shell-panel.png](.project/renders/ui/shell-panel.png) shows it, with a tab on the panel's left edge. [shell.png](.project/renders/ui/shell.png) is Shell with that column closed.

The menu button is on Shell. [shell-menu.png](.project/renders/ui/shell-menu.png) shows its menu open.

## References

The shell's appearance is Simple Sketch 1.0.1's chrome, carried into the host in version 0.2. The swatches, tools, property pages, and drawing in these pictures are what Simple Sketch declares; the host draws them without knowing them.

- [shell-demo.png](.project/renders/ui/shell-demo.png) — the owner's picture of the shell with the template loaded, given on 2026-10-14. It shows the title bar, the tab strip with the menu button, one tab, and the plus, the empty white toolbar, the faint grid in the contents area, and the panel tab at the right edge. It reads "Untitled 1 — Simple Sketch" and its tab reads "Untitled 1". As the owner set on 2026-10-14, the title reads Inpainter and the template's tab reads Template.
- [shell.png](.project/renders/ui/shell.png) — the shell running Simple Sketch, with the panel closed: the tab strip and the toolbar above an empty canvas.
- [shell-panel.png](.project/renders/ui/shell-panel.png) — Simple Sketch and the template in one window, with a stroke on Canvas 1 and the panel open on the brush's page.
- [shell-menu.png](.project/renders/ui/shell-menu.png) — Simple Sketch's menu open from the menu button, with the select tool's toolbar and the panel open on Canvas 1's palette.

These three were captured from the running host on 2026-10-15, after the owner tried that build. They replace the Simple Sketch 1.0.1 renders this document cited before, which the owner removed from the project on 2026-10-15 once Simple Sketch was part of Inpainter. They show the inside of the window; the title bar is the desktop's, and reads Inpainter.

## Presentation

The shell is light and compact. A pale strip at the top holds the menu button, the tabs, and a new-tab control. The active tab is white and joins the white toolbar. Inactive tabs are paler. In shell-panel.png the open panel is a white column on the right, with a thin line at its left edge and a small tab on that edge.

## Shell

Shell is the screen the person gets by launching the host. The host keeps this frame for every sub-application, including one that declares nothing. A sub-application fills the tool area and the area under the toolbar, and declares the shared components it uses there, such as the properties panel and the grid.

In [shell.png](.project/renders/ui/shell.png) the window holds the tab strip, the toolbar, and a large area beneath. The title bar above them is the desktop's and reads Inpainter.

The tab strip is the pale bar at the top. A menu button of three horizontal lines is at its left, then the tabs, then a plus control. shell.png shows one tab, Untitled 1, with its close mark. In shell-panel.png, Untitled 1 has unsaved changes, so a dot stands in for its close mark, and a second tab, Template, belongs to another sub-application. The active tab is white, and its lower edge meets the toolbar. Inactive tabs are paler, with a light border.

The menu is that button. [shell-menu.png](.project/renders/ui/shell-menu.png) shows it open as a list of rows under the button; a row that opens a submenu ends in a chevron, and a row that cannot run is drawn lighter.

The tool area is the white bar under the tabs. Thin vertical lines divide it into groups. In shell.png the groups are Simple Sketch's color swatches, file commands, undo and redo, the new-canvas button and the drawing tools, a Size control, and, pinned at the right, the zoom controls and the maximize toggle. Those contents are Simple Sketch's declarations. The bar is the host's tool area.

Under the toolbar is the area a sub-application fills. In shell.png it runs to the right edge of the window: a faint square grid. In shell-panel.png the same gridded area stops where the panel begins. The grid is the drawing surface's; the drawing on it is Simple Sketch's document.

The properties panel is a shared component of the host, as the owner set on 2026-10-14. The host draws it, always the same way, only for a tab whose sub-application declares it. The sub-application declares whether a tab has the panel, which side it sits on, and what it holds. Only the right side has a reference, so only the right side is built. Without it, there is no panel tab and F4 does nothing. The panel starts closed. While it is closed, the same tab sits at the canvas's right edge, as the owner set on 2026-10-11. Clicking it shows or hides the panel, and the chevron points the way the panel will move.

Each tab is a document its sub-application reports. Its label is the name of the file that sub-application manages or, when it manages no file, the sub-application's name. The menu's contents are the commands declared by the sub-application of the selected tab. The title always reads Inpainter, whatever tab is selected, as the owner set on 2026-10-14. The faint grid is a shared component the host draws in the contents area when the tab's sub-application declares it.

When a sub-application stops or stops responding, the host stays up and shows that status, as Sub-application status below shows.

## Properties

Properties is the properties panel open, as in shell-panel.png and shell-menu.png. It is the host's, and it appears only for a tab whose sub-application declares it.

The panel is a white column on the right, below the toolbar. A thin line runs down its left edge. A small white tab joins that edge near the top and overlaps the area beside it. The chevron on the tab points right. While the panel is closed, that same tab sits at the canvas's right edge, as the owner set on 2026-10-11. Clicking it shows or hides the panel, and the chevron points the way the panel will move. The column has no close control and no title of its own. shell-panel.png starts the column at a section heading, and thin lines separate later sections. Its sections are Simple Sketch's brush page; shell-menu.png shows a canvas's palette instead. Those pages are Simple Sketch's declarations. The column, its left edge, and the tab are the panel chrome.

## Template

Template is Shell with the template loaded, as in [shell-demo.png](.project/renders/ui/shell-demo.png), as the owner set on 2026-10-14. The title bar reads Inpainter. The tab strip holds the menu button, one tab labeled Template with its close mark, and the plus, drawn as in the picture. The template declares no menu rows and no new-tab command and has only that tab, so clicking the menu button, the plus, or the close mark does nothing: no popup, no new tab, no close. The toolbar is the white bar, empty, with its hairline. The contents area shows the faint grid, which the template declares, and nothing draws on it. The template declares the properties panel on the right, so the panel tab sits at the right edge, and opening it shows the empty white column. Nothing else happens.

## Sub-application status

These two renders were generated on 2026-10-14. The owner approved both on 2026-10-14 as the visual reference for H-2. Only the status notice is taken from them: one line of medium-gray text centered in the contents area, reading "Simple Sketch has exited." or "Simple Sketch is not responding.", the second with a small ring before it, with no button, card, or dimming. Everything else in them stays as the carried-over chrome draws it. The name in the notice is the sub-application's name.

Both are the same host window. **simple-sketch-exited** is that window after Simple Sketch has exited. **simple-sketch-not-responding** is that window after Simple Sketch has stopped responding. The host stays open, and the common UI can still be used. View the exited state first; the other was generated from it.

- **simple-sketch-exited** — [prompt](.project/renders/models/openai/gpt-image-2-5-sunburst/prompts/01-simple-sketch-exited.md), [image](.project/renders/models/openai/gpt-image-2-5-sunburst/simple-sketch-exited/v001/image.png), [run record](.project/renders/models/openai/gpt-image-2-5-sunburst/simple-sketch-exited/v001/status.json). Generated from [shell-demo.png](.project/renders/ui/shell-demo.png) and a Simple Sketch 1.0.1 render of the brush page, no longer kept in the project.
- **simple-sketch-not-responding** — [prompt](.project/renders/models/openai/gpt-image-2-5-sunburst/prompts/02-simple-sketch-not-responding.md), [image](.project/renders/models/openai/gpt-image-2-5-sunburst/simple-sketch-not-responding/v001/image.png), [run record](.project/renders/models/openai/gpt-image-2-5-sunburst/simple-sketch-not-responding/v001/status.json). Generated from the simple-sketch-exited image, with only the status changed. The run records name the folders the files had when they were generated.
