# Inpainter

Inpainter is a modular, AI-native desktop framework for creative work. A person opens one host application, and that host is the window. Separate sub-applications, each its own program, each take one kind of creative work. An assistant in the shell can read, understand, and change the work they own. Simple Sketch, a pressure-sensitive infinite-canvas sketching app, is the first. The person only ever launches the host, and the host starts Simple Sketch.

## What it solves

Each kind of creative work otherwise has its own program and its own window. Inpainter keeps one shell. This version puts Simple Sketch in that shell and leaves it as it is: the same look, the same behavior.

The host draws the common UI for every sub-application: the window, a tab strip with a new-tab control, a menu, a tool area, and the sub-application's status when it stops or stops responding. Simple Sketch fills the tool area with its tools and the space under the toolbar with the sketch. The properties panel and the grid appear with it. Those areas stay with the host. When Simple Sketch exits or stops responding, the window stays open and the host shows that status. The person can still use the common UI. The host and Simple Sketch stay separate programs.

## How people use it

Open Inpainter. Simple Sketch is already started. On Canvas 1, draw a smoothed pressure-sensitive stroke. Erase part of it: the ink in the path disappears, and the stroke stays whole. Undo brings that ink back. Then pan, zoom, and fit.

At the start, that window shows one tab labeled Simple Sketch, an empty toolbar, the grid, and the panel tab, and nothing else happens. The finished version fills that shell with Simple Sketch as it looks today: its tools, its properties, and the sketch.

Simple Sketch can be checked with no host running. A person still does not launch it. The check drives the sketching on its own.

## What to learn from existing apps

Simple Sketch is the reference for the shell and for sketching. One window holds the tab strip and new-tab button, the menu button, the toolbar, and the properties panel that opens from a tab at the panel's edge. Inpainter keeps that chrome. Swatches, drawing tools, and sketch commands stay with Simple Sketch. A later sub-application fills the same areas with its own work.

## How it should look and work

The host window appears first. The reference shows it light and compact, with rounded corners and a soft shadow. The title bar has minimize, maximize, and close at the right. The reference app shows a small orange pencil and the words Simple Sketch there. This shell's title reads Inpainter. A pale strip holds a menu button of three horizontal lines, the tabs, and a new-tab control. The active tab is white and meets the white toolbar. Inactive tabs are paler.

Simple Sketch's pictures fill the toolbar with color swatches, file commands, undo and redo, drawing-tool icons, a Size control, and zoom controls, separated by thin vertical lines. Under it is a near-white canvas with a faint square grid. The properties panel is a white column on the right, below the toolbar, opened from a small tab on its left edge. It begins on its first section, brush settings, eraser settings, or a color's settings, and has no title of its own.

## This version

Version 0.2 is Simple Sketch in that host window. Later, and still part of the product: the assistant, a persistent chat panel on the left, about a quarter of the window's width; any second creative app; and a sub-application that draws its own view by sending the host what to paint. The assistant can then carry a sequence of tasks across those apps, such as writing a story, turning its scenes into image prompts, and splitting the pictures into layers.
