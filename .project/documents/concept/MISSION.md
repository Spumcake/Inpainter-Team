# Inpainter — mission

Inpainter is a modular, AI-native desktop framework for creative work. One host application provides a shared shell. Separate sub-applications, each its own program, own one kind of creative work. At the center of how a person uses Inpainter is the assistant, an agentic harness in the shell. Each sub-application provides skills that tell the assistant what it can do with that kind of work. The assistant discovers its capabilities from those skills, plans from the capabilities it knows it has, and carries out the plan by reading, understanding, and changing the work the sub-applications own. This is what sets Inpainter apart: a person asks for something, and the assistant works out how to make it with the apps at hand. Working with the assistant and working by hand are one seamless flow. Every sub-application works fully on its own, without the assistant. With it, the assistant's automated work amplifies what the person does by hand. Simple Sketch, a pressure-sensitive infinite-canvas sketching app, is the first sub-application.

## Where it is going

Many sub-applications in one window, and one agentic harness that plans a sequence of tasks across them from the skills they provide, then carries it out. For example, a person asks the assistant for a story with pictures: it writes the story in a writing app, turns the story's scenes into image prompts, and splits the resulting images into layers in a paint program. A later animatic app could arrange drawings in frames on a timeline. The assistant is part of the product even in a version that leaves it out.

## What is always true

**The shell.** The person only ever launches the host; the host starts sub-applications for them. The host always draws its own common UI, whatever sub-application is loaded, even one that declares nothing: the window, a tab strip with a new-tab control, a menu, a tool area, and the sub-application's status when it stops or stops responding. Once it exists, the assistant is a persistent chat panel on the left, about a quarter of the window's width. Sub-applications fill these areas; they never create or remove them. The host also provides shared components that a sub-application chooses to use, such as the properties panel and the grid. The host draws a shared component, always the same way, only where a sub-application declares it; the sub-application declares whether it uses one, what it holds, and, for the panel, which side it sits on.

**The boundaries.** The host and each sub-application are separate programs. The host cannot import an app, and apps never import each other. The code structure enforces this, with separate crates and programs, not documents alone. One process serves every tab of a sub-application, and if it crashes or hangs, the host stays up. A sub-application can run with no host and be driven entirely through the protocol.

**The protocol.** Sub-applications declare their tools, commands, properties, views, and agent skills as data through one protocol. The host draws simple UI from those declarations. An app may draw its own custom view by running egui itself and sending the host what to paint; this stays unproven until an experiment shows that scrubbing a frame strip feels smooth.

**Shared drawing.** Drawing, erasing, shapes, text, and ordering live in a shared library that the host uses for live editing, so pen input never crosses between processes. Each sub-application owns how its work is arranged, how it is saved, and its undo.

**The stack.** Rust, egui through eframe, and Iconoir icons. No Electron, Tauri, or web target. Do not fork existing software, but reuse good components.

## What it is, and what would fake it

- **It is** a host that runs sub-applications it knows nothing about. **The fake** is a host with one sub-application's code built into it and a second process running alongside, so the boundary exists only in the dependency list. **The test:** a test-only sub-application, which the host was not written for, runs beside Simple Sketch in the same window and gets a working toolbar, menu, and panel purely from what it declares, with labels Simple Sketch does not use, and the host still builds with no sub-application's code in it.
- **It is** a sub-application that owns its document. **The fake** is a host that keeps its own copy and history and mirrors them to the sub-application. **The test:** undo and saving happen only in the sub-application's process, and the host has no undo history of its own.
- **It is** a shell whose toolbar, menu, and panel come from the selected tab's declarations. **The fake** is a shell drawn from one sub-application's internal state. **The test:** selecting a tab of a different sub-application changes those contents with no change to the host's code.
- **It is** Simple Sketch's chrome, carried over. **The fake** is a new design, or a rewrite that only looks similar. **The test:** the host's chrome code matches the reference functions it came from, and a screenshot matches the references.
- **It is** a sub-application that works with no host. **The fake** is a protocol that only works while the window is open. **The test:** a test with no host creates, edits, undoes, and saves through the protocol alone.
- **It is** an assistant that knows what it can do because the sub-applications told it. **The fake** is an assistant whose knowledge of each app is written into its prompts or code. **The test:** a test-only sub-application provides a skill the assistant was never told about, and when asked for something only that skill does, the assistant plans with it and uses it.
- **It is** hand work and assistant work on the same piece of work. **The fake** is an assistant that works in a separate mode or on its own copy, or a sub-application that needs the assistant to work. **The test:** with the assistant off, every sub-application does all it did before. With it on, the person changes by hand what the assistant made, and the assistant continues from what the person did, in the same tab.

## References

**Simple Sketch** 1.0.1 (at `887f6f4`) is part of Inpainter. Its chrome is the host shell: the window, tab strip and new-tab button, menu button, toolbar, and the properties panel that opens from a tab at its edge, as `INTERFACE.md` describes and the renders in `.project/renders/ui/` show. Its sketching is the Simple Sketch sub-application. Both were carried over from its code, not rewritten, and the shell is that chrome without what belongs only to sketching.

## What it will never be

A monolith that tries to do everything in one program. A general-purpose AI chat app: the assistant works on creative work owned by sub-applications.

## Words we use

- **Host:** Inpainter itself: the shell, the protocol, and the assistant.
- **Sub-application:** a separate program that owns one kind of creative work.
- **Artifact:** a piece of work a sub-application owns and saves.
- **Declarations:** what a sub-application tells the host it offers, as data.
- **Common UI:** what the host always shows, whatever sub-application is loaded.
- **Shared component:** something the host draws the same way for any sub-application that declares it, such as the properties panel and the grid.
