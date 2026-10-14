# Inpainter — mission

Inpainter is a modular, AI-native desktop framework for creative work. One host application provides a shared shell. Separate sub-applications, each its own program, own one kind of creative work, and an assistant in the shell can read, understand, and change the work they own. Simple Sketch, a pressure-sensitive infinite-canvas sketching app, is the first sub-application.

## Where it is going

Many sub-applications in one window, used by one agentic harness to carry out a sequence of tasks across them. For example, a person asks the assistant for a story with pictures: it writes the story in a writing app, turns the story's scenes into image prompts, and splits the resulting images into layers in a paint program. A later animatic app could arrange drawings in frames on a timeline. The assistant is part of the product even in a version that leaves it out.

## What is always true

**The shell.** The person only ever launches the host; the host starts sub-applications for them. The host always draws its own common UI, whatever sub-application is loaded, even one that declares nothing: the window, a tab strip with a new-tab control, a menu, a tool area, a panel area, and the sub-application's status when it stops or stops responding. Once it exists, the assistant is a persistent chat panel on the left, about a quarter of the window's width. Sub-applications fill these areas; they never create or remove them.

**The boundaries.** The host and each sub-application are separate programs. The host cannot import an app, and apps never import each other. The code structure enforces this, with separate crates and programs, not documents alone. One process serves every tab of a sub-application, and if it crashes or hangs, the host stays up. A sub-application can run with no host and be driven entirely through the protocol.

**The protocol.** Sub-applications declare their tools, commands, properties, views, and agent skills as data through one protocol. The host draws simple UI from those declarations. An app may draw its own custom view by running egui itself and sending the host what to paint; this stays unproven until an experiment shows that scrubbing a frame strip feels smooth.

**Shared drawing.** Drawing, erasing, shapes, text, and ordering live in a shared library that the host uses for live editing, so pen input never crosses between processes. Each sub-application owns how its work is arranged, how it is saved, and its undo.

**The stack.** Rust, egui through eframe, and Iconoir icons. No Electron, Tauri, or web target. Do not fork existing software, but reuse good components.

## References

**Simple Sketch** (`src/simple-sketch`, version 1.0.1 at `887f6f4`) is the reference for the host shell and for sketching. The shell's look and behavior are Simple Sketch's chrome: its window, tab strip and new-tab button, menu button, toolbar, and the properties panel that opens from a tab at its edge. They are shown in `base-ui.png` and the other renders in `.project/.simple-sketch/renders/ui/`, described in the archived `.project/.simple-sketch/INTERFACE.md`, and implemented in its code, including `tabs.rs`, `toolbar.rs`, `widgets.rs`, and `panel.rs`. That code is carried over into the host and the Simple Sketch sub-application, not rewritten. The shell is that chrome without what belongs only to sketching.

## What it will never be

A monolith that tries to do everything in one program. A general-purpose AI chat app: the assistant works on creative work owned by sub-applications.

## Words we use

- **Host:** Inpainter itself: the shell, the protocol, and the assistant.
- **Sub-application:** a separate program that owns one kind of creative work.
- **Artifact:** a piece of work a sub-application owns and saves.
- **Declarations:** what a sub-application tells the host it offers, as data.
- **Common UI:** what the host always shows, whatever sub-application is loaded.
