# Inpainter

Inpainter is a modular, AI-native desktop framework for creative work. A person opens one host application, and that host is the window. Separate sub-applications, each its own program, each take one kind of creative work and run inside the host. An assistant in the shell can read, understand, and change the work they own. Simple Sketch, a pressure-sensitive infinite-canvas sketching app, is the first sub-application. The person only ever launches the host. The host starts each sub-application.

## What it solves

Each kind of creative work otherwise has its own program and its own window. Inpainter keeps one shell. The host draws the same common UI whichever sub-application is loaded, including one that declares nothing: the window, a tab strip with a new-tab control, a menu, a tool area, and the sub-application's status when it stops or stops responding. The host also offers shared components, the properties panel and the grid, which a sub-application uses only if it declares them. A sub-application fills those areas. The areas stay with the host.

The window stays up when a sub-application stops or stops responding, and the host shows that status. The assistant remains part of the product. Later it carries a sequence of tasks across sub-applications: it writes a story in a writing app, turns the scenes into image prompts, and splits the pictures into layers in a paint program. A later animatic app can place drawings on a timeline.

## How people use it

Open Inpainter. The host window appears with Simple Sketch already started. The tab strip holds the open work, the tool area holds Simple Sketch's tools, and the panel holds its properties. Sketching happens in that window.

The first slice loads an empty demo that declares nothing. The window, the tab strip and its new-tab control, the menu, and the tool area are still there. The demo declares the grid and the properties panel, so they appear too. The demo has nothing of its own to add.

With no host, Simple Sketch is driven entirely through the protocol. The person does not launch Simple Sketch. They launch Inpainter, and the host starts the sub-application.

## What to learn from existing apps

Simple Sketch is the reference for this shell: its window, tab strip and new-tab button, menu button, toolbar, and the properties panel that opens from a tab at the panel's edge. A new tab, the menu, the tools, and the properties stay in that one window. Inpainter keeps this chrome. Swatches, drawing tools, the grid, and sketch commands stay with Simple Sketch, which declares them. Another sub-application fills the same areas with what it declares, and the host goes on drawing the shell.

## How it should look and work

The host window is what appears first. The reference shows it light and compact. The title bar carries a small icon and a title, with minimize, maximize, and close at the right. A pale strip holds the menu button, the tabs, and the new-tab control. The active tab is white and meets the toolbar. Inactive tabs are paler. The properties panel is a white column on the right, below the toolbar, opened from a small tab on its edge, and it begins on its first section. Drawings, palettes, and tool icons in those pictures belong to sketching.

## This version

Version 0.1 separates the host from the sub-application. Simple Sketch runs inside the host, and it can also run with no host. The first slice is the host shell with an empty demo sub-application that declares nothing. The version continues past that slice to Simple Sketch inside the host. The assistant comes later, as a persistent chat panel on the left, about a quarter of the window's width. Any second creative app comes later as well. This version builds the shared shell and the first sub-application, on the way to many sub-applications in one window and an assistant that works across them.
