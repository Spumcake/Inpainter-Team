# Inpainter interface

Inpainter is the host a person launches for creative work. This document describes the host's common UI, the shell on screen whichever sub-application is loaded. Product intent is in [the version 0.3 pitch](.project/documents/history/v0.3/PITCH.md). Version 0.3 added the assistant's chat panel, described under Assistant. These screens are the host's common UI.

## Journey and sitemap

The person launches the host and lands on **Shell**. This version stays in that window.

- **Shell** is the host window: title bar, tab strip with its new-tab control, menu button, tool area, the area a sub-application fills, and that sub-application's status when it stops or stops responding.
- **Assistant** is the chat panel on the left of Shell, present in every window from version 0.3. [agentic-harness-mockup.png](.project/renders/ui/agentic-harness-mockup.png) shows it in rough form. The logo at its top-left opens the **logo menu**, which assistant-logo-menu shows. Quitting, or opening or clearing a workspace, with unsaved workspace changes shows **Save this workspace?**, which workspace-save-prompt shows.
- **Properties** is the properties panel open on the right of Shell, for a tab whose sub-application declares it. [shell-panel.png](.project/renders/ui/shell-panel.png) shows it, with a tab on the panel's left edge. [shell.png](.project/renders/ui/shell.png) is Shell with that column closed.

The menu button is on Shell. [shell-menu.png](.project/renders/ui/shell-menu.png) shows its menu open.

## References

The shell's appearance is Simple Sketch 1.0.1's chrome, carried into the host in version 0.2. The swatches, tools, property pages, and drawing in these pictures are what Simple Sketch declares; the host draws them without knowing them.

- [shell-demo.png](.project/renders/ui/shell-demo.png) — the owner's picture of the shell with the template loaded, given on 2026-10-14. It shows the title bar, the tab strip with the menu button, one tab, and the plus, the empty white toolbar, the faint grid in the contents area, and the panel tab at the right edge. It reads "Untitled 1 — Simple Sketch" and its tab reads "Untitled 1". As the owner set on 2026-10-14, the title reads Inpainter and the template's tab reads Template.
- [shell.png](.project/renders/ui/shell.png) — the shell running Simple Sketch, with the panel closed: the tab strip and the toolbar above an empty canvas.
- [shell-panel.png](.project/renders/ui/shell-panel.png) — Simple Sketch and the template in one window, with a stroke on Canvas 1 and the panel open on the brush's page.
- [shell-menu.png](.project/renders/ui/shell-menu.png) — Simple Sketch's menu open from the menu button, with the select tool's toolbar and the panel open on Canvas 1's palette.

- [agentic-harness-mockup.png](.project/renders/ui/agentic-harness-mockup.png) — a generated picture of the shell with the assistant's chat panel on the left, supplied by the owner on 2026-10-15. As the owner set that day, it approximates layout and look only. Its features and behavior are not a reference: which controls the panel has is the owner's list under Assistant.

- **assistant-logo-menu** — [prompt](.project/renders/models/openai/gpt-image-2-5-sunburst/prompts/03-assistant-logo-menu.md), [image](.project/renders/models/openai/gpt-image-2-5-sunburst/assistant-logo-menu/v001/image.png), [run record](.project/renders/models/openai/gpt-image-2-5-sunburst/assistant-logo-menu/v001/status.json). Generated on 2026-10-15 from the mockup, shell-menu.png, and the light logo, `.project/assets/logo-light.svg`. The owner approved it on 2026-10-15 as the reference for the logo, its menu, and the chat panel's conversation and input row. Its toolbar leaves out Simple Sketch's swatches and file commands; the toolbar stays as Simple Sketch declares it.
- **workspace-save-prompt** — [prompt](.project/renders/models/openai/gpt-image-2-5-sunburst/prompts/04-workspace-save-prompt.md), [image](.project/renders/models/openai/gpt-image-2-5-sunburst/workspace-save-prompt/v001/image.png), [run record](.project/renders/models/openai/gpt-image-2-5-sunburst/workspace-save-prompt/v001/status.json). Generated on 2026-10-15 from assistant-logo-menu and shell-ask.png. The owner approved it on 2026-10-15 as the reference for the workspace's save question.
- [shell-ask.png](.project/renders/ui/shell-ask.png) — the host's existing dialog, Simple Sketch's "Keep this sketch?", captured from the running host on 2026-10-15 on a virtual display.

These three captures were taken from the running host on 2026-10-15, after the owner tried that build. They replace the Simple Sketch 1.0.1 renders this document cited before, which the owner removed from the project on 2026-10-15 once Simple Sketch was part of Inpainter. They show the inside of the window; the title bar is the desktop's, and reads Inpainter.

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

## Assistant

The assistant's chat panel is part of the host's common UI from version 0.3, present whatever sub-application is loaded. Its layout and look follow [agentic-harness-mockup.png](.project/renders/ui/agentic-harness-mockup.png) approximately. Its controls are the ones the owner listed on 2026-10-15: the conversation, the input box, an attachment button, the model picker, a microphone, and send.

The panel is a white column at the left of the window, about a quarter of its width, running the window's full height. A thin vertical line separates it from the shell. The tab strip, toolbar, contents area, and properties panel sit to its right, unchanged.

The conversation fills the panel from the top and scrolls. The person's messages are pale gray rounded bubbles on the right, with a small gray time under each. The assistant's replies are on the left in white rounded cards with a light border, also timed. A reply may hold short paragraphs and bulleted lines. When a message carried attachments, their file names show under its text.

At the bottom is a rounded white box with a light border and a soft shadow. Its first line is the text field, with the placeholder "Ask the assistant…". Under it, from the left, are a plus button, the model picker, and, at the right, a microphone and a round black send button with a white upward arrow. The model picker is a quiet pill holding a lightning icon, the model's name, and a down chevron; clicking it opens a list of models. The plus opens a file dialog for pictures and text files; each chosen file shows as a small chip above the text field with its file name and a close mark. The microphone is shown and does nothing, as the owner set on 2026-10-15. While the assistant is working on a request, the send button shows a small white square instead of the arrow, and clicking it stops the assistant. Meanwhile the applications are locked, as the owner set on 2026-10-15: the toolbar, menu rows, and properties panel are drawn in their disabled, lighter look and do nothing, and drawing on the canvas does nothing. Panning, zooming, and switching tabs still work.

When the assistant cannot answer, because no OpenRouter key is set, the network fails, or the model returns an error, the reason appears as the assistant's reply in the conversation, in the same card.

At the panel's top-left, level with the tab strip, is the Inpainter logo, the light one in `.project/assets/logo-light.svg`, small, with a gray down chevron after it, as assistant-logo-menu shows. Nothing else is in that row. Clicking it opens the logo menu below it, in the menu look of shell-menu.png: a white rounded box with a thin border and a soft shadow, rows of plain text, and thin dividers. Its rows are New Window; then Open Workspace…, Recent Workspaces with a chevron to its submenu, Save Workspace, and Save Workspace As…; then Clear Workspace; then Quit; with no icons and no shortcuts, as the owner set on 2026-10-15.

Save this workspace? is the host's dialog in the look of shell-ask.png, as workspace-save-prompt shows: the window dimmed, and a small white box with the title "Save this workspace?", the workspace's name in gray under it, and the buttons Save, Discard, and Cancel.

The mockup's undo and redo at the panel's top, change card, and message buttons (copy, thumbs, share, more) are not built, as the owner chose on 2026-10-15.

## Properties

Properties is the properties panel open, as in shell-panel.png and shell-menu.png. It is the host's, and it appears only for a tab whose sub-application declares it.

The panel is a white column on the right, below the toolbar. A thin line runs down its left edge. A small white tab joins that edge near the top and overlaps the area beside it. The chevron on the tab points right. While the panel is closed, that same tab sits at the canvas's right edge, as the owner set on 2026-10-11. Clicking it shows or hides the panel, and the chevron points the way the panel will move. The column has no close control and no title of its own. shell-panel.png starts the column at a section heading, and thin lines separate later sections. Its sections are Simple Sketch's brush page; shell-menu.png shows a canvas's palette instead. Those pages are Simple Sketch's declarations. The column, its left edge, and the tab are the panel chrome.

## Template

Template is Shell with the template loaded, as in [shell-demo.png](.project/renders/ui/shell-demo.png), as the owner set on 2026-10-14. The title bar reads Inpainter. The tab strip holds the menu button, one tab labeled Template with its close mark, and the plus, drawn as in the picture. The template declares no menu rows and no new-tab command and has only that tab, so clicking the menu button, the plus, or the close mark does nothing: no popup, no new tab, no close. The toolbar is the white bar, empty, with its hairline. The contents area shows the faint grid, which the template declares, and nothing draws on it. The template declares the properties panel on the right, so the panel tab sits at the right edge, and opening it shows the empty white column. Nothing else happens by hand. From version 0.3 the template also provides one skill to the assistant, as the owner agreed on 2026-10-15: asked to, the assistant can change the template tab's label. It is the mission's proof that the assistant learns what it can do from a sub-application it was never told about.

## Sub-application status

These two renders were generated on 2026-10-14. The owner approved both on 2026-10-14 as the visual reference for H-2. Only the status notice is taken from them: one line of medium-gray text centered in the contents area, reading "Simple Sketch has exited." or "Simple Sketch is not responding.", the second with a small ring before it, with no button, card, or dimming. Everything else in them stays as the carried-over chrome draws it. The name in the notice is the sub-application's name.

Both are the same host window. **simple-sketch-exited** is that window after Simple Sketch has exited. **simple-sketch-not-responding** is that window after Simple Sketch has stopped responding. The host stays open, and the common UI can still be used. View the exited state first; the other was generated from it.

- **simple-sketch-exited** — [prompt](.project/renders/models/openai/gpt-image-2-5-sunburst/prompts/01-simple-sketch-exited.md), [image](.project/renders/models/openai/gpt-image-2-5-sunburst/simple-sketch-exited/v001/image.png), [run record](.project/renders/models/openai/gpt-image-2-5-sunburst/simple-sketch-exited/v001/status.json). Generated from [shell-demo.png](.project/renders/ui/shell-demo.png) and a Simple Sketch 1.0.1 render of the brush page, no longer kept in the project.
- **simple-sketch-not-responding** — [prompt](.project/renders/models/openai/gpt-image-2-5-sunburst/prompts/02-simple-sketch-not-responding.md), [image](.project/renders/models/openai/gpt-image-2-5-sunburst/simple-sketch-not-responding/v001/image.png), [run record](.project/renders/models/openai/gpt-image-2-5-sunburst/simple-sketch-not-responding/v001/status.json). Generated from the simple-sketch-exited image, with only the status changed. The run records name the folders the files had when they were generated.
