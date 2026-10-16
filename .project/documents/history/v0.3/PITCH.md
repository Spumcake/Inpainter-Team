# Inpainter

Inpainter is a modular, AI-native desktop framework for creative work. A person opens one host application, and that host is the window. Separate sub-applications, each its own program, each take one kind of creative work. At the center is the assistant, a chat panel in the shell that reads, understands, and changes the work those sub-applications own. Simple Sketch, a pressure-sensitive infinite-canvas sketching app, is the first sub-application. Version 0.3 is the first in which you can talk to the assistant.

## What it solves

With the assistant, you say what you want, and the work appears in the sketch you have open. The assistant looks at the canvas before it acts, and works with the same strokes, shapes, text, colors, and styles you use by hand.

The assistant doesn't know Simple Sketch in advance. Simple Sketch tells it what it can do with a sketch, and the assistant works out the rest. What it makes is ordinary work in your sketch, to change by hand, undo, or build on.

## How people use it

Ask "create a circle and a square next to each other", and both appear side by side on the open canvas, drawn the way your own would be.

Ask "make the circle pink and dashed". The assistant finds the circle it drew and changes its color and stroke style. Drag the square somewhere else, then ask "put a line between them". The assistant reads where both shapes are now, and joins them.

Draw something freehand and ask what it is, and the assistant looks at the canvas and tells you. Attach a picture and ask for something like it, and the assistant uses the picture as a guide.

While the assistant works, the app's tools are greyed out until it finishes. One undo then takes back one request.

Save the workspace from the logo menu, and Inpainter remembers your open sketches, their tabs, and the conversation. Reopen it later and everything is where you left it, chat included. Close without saving, and Inpainter offers to save it.

## What to learn from existing apps

tldraw's [agent starter kit](https://tldraw.dev/starter-kits/agent) puts a chat panel beside an infinite canvas. Its agent sees the canvas as a picture and as a list of shapes, and edits it with the canvas's own operations. Inpainter takes that idea, and moves the operations out of the assistant: each sub-application declares its own skills. Its workspace follows code editors such as [VS Code](https://code.visualstudio.com/docs/editing/workspaces/workspaces), where a workspace file gathers folders without owning them.

## How it should look and work

The chat panel is a column on the left, about a quarter of the window's width, as the [mockup](../../../renders/ui/agentic-harness-mockup.png) shows in rough form. The mockup is a guide to layout and look, not to features. To the panel's right, the shell is unchanged: the tab strip, the toolbar, the canvas with its grid, and the properties panel on the right.

At the panel's top-left is the Inpainter logo, which opens the workspace menu: New Window, Open Workspace, Recent Workspaces, Save Workspace, Save Workspace As, Clear Workspace, and Quit. Below it, the conversation fills the panel: your messages in pale bubbles on the right, the assistant's replies on the left. At the bottom is a rounded box with the placeholder "Ask the assistant…", a plus for attaching pictures or text files, the model picker, a microphone that does nothing yet, and a round send button.

## This version

Version 0.3 delivers the assistant working on Simple Sketch, through OpenRouter: the chat panel, the model picker, attachments, and an assistant that reads the open sketch and changes it with the skills Simple Sketch provides. It also delivers the workspace, which keeps the open files, the tabs, and the conversation together.

Left for later, and still part of the product: voice input behind the microphone; one request planned across several sub-applications, such as writing a story, turning its scenes into image prompts, and splitting the pictures into layers; any second creative app; and a sub-application that draws its own view by sending the host what to paint.
