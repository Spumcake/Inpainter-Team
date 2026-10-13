# The Inpainter Studio premise and test apps

**UTC:** 2026-10-13T00:00:16Z
**Covers:** since [the Inpainter Studio discussion](2026-10-12T21-29-02Z-inpainter-studio-discussion.md).
**Repositories:** Team `master` is at `4a6afe0` (Record the Inpainter Studio discussion in the diary.), the same as `origin/master`, with this entry uncommitted. App `vector-sketch` `dev` is at `0c1d3b7`, the same as `origin/dev`, and clean.

**Status: experimental.** As in the previous entry, nothing here is decided. It records how the owner and Claude Code currently describe the idea, and apps proposed for testing it.

## What we did

Claude Code committed and pushed the previous entry and the note on the connector entry as `4a6afe0`.

The owner said "workspaces" gives an agent too little context. What they described is closer to a framework of separate apps in one interface that an agent can reach. They gave an end goal: ask the assistant for a short story in a word-processor app, then image prompts for its scenes, then a split of the images into layers in a paint app. They asked how to re-orient the project, and Claude Code drafted a description, a vocabulary, and a new opening prompt in the style of the original Simple Sketch prompt. The owner then asked for simple apps to test the framework.

## The premise as currently described

Inpainter Studio is one window that hosts separate creative apps and one assistant that works across them. Each app is its own program and owns one kind of work, with its own data, saving, undo, commands, and views. The host knows nothing about any app's subject. It provides the window, tabs, the shared drawing editor, shared widgets, and the assistant. Every app declares what it can do through one protocol, so the assistant can use any app, and pass one app's output to another, without per-app code. A short version for agents: a host for apps that bring their own UI, where an assistant drives every app through one protocol, close to an MCP host whose servers bring their own interface.

The proposed vocabulary:
- **Host:** Inpainter Studio itself: window, tabs, assistant, shared drawing editor and widgets, and installing apps.
- **App:** a separate program owning one kind of artifact. Apps never import each other. The user-facing name is still the owner's to choose.
- **Artifact:** a piece of work an app owns and saves.
- **Capabilities:** the commands, properties, tools, views, and skills an app declares, read by both the host's UI and the assistant.
- **Handoff types:** shared formats such as text, image, layered image, and drawing, so one app's output becomes another's input. These are new to the discussion and matter as much as the protocol.
- **Shared kits:** libraries any app can reuse, such as drawing and widgets.

The draft prompt keeps Rust, egui, and Iconoir, rules out Electron, Tauri, a web target, and forks, and requires boundaries enforced by code structure. Simple Sketch keeps working, and finishing it comes before any second app. The first milestone would be the same Simple Sketch running as an app inside the host, with a test that drives it through the protocol and no host running. Custom views stay unproven until the frame-strip experiment.

## Test apps proposed

Each app is small enough that its features don't matter. What matters is which part of the framework it tests:
1. **Hello:** one command, one property, one artifact. It tests discovery, launch, declared UI, and save. Writing it in Python would show the protocol doesn't depend on Rust.
2. **Notes:** a plain-text editor. It tests declared UI, its own format and undo, and the text handoff type. It is the seed of a word-processor app.
3. **Crash:** buttons that crash, freeze, or send a malformed message. The host must survive and offer a restart.
4. **Flipbook:** frames drawn with the host's brush, and a scrubbable strip the app draws itself. It tests the shared drawing kit, a custom view, and undo across strokes and frame moves. It doubles as the latency experiment.
5. **Swatches:** a palette maker whose palettes Simple Sketch imports. It tests the simplest handoff between two apps.
6. **Layers:** opens a PNG and splits it into one layer per color, with no AI. It tests the image and layered-image handoffs and larger textures. It is the seed of a paint app.
7. **Checklist:** add, check, reorder, and nest. It tests many small structured edits by the assistant.

Before any hosted model is connected, a fixed scripted chain would run through the assistant's protocol: Notes writes three lines, Checklist makes one item per line, Flipbook makes one labelled frame per item, and Layers imports a picture into frame 1. If that works, later failures with a model are the model's, not the framework's. Hello, Notes, Crash, and Flipbook were suggested as the first set.

## Where things stand

No code, pitch, systems, plan, or queue changed. The assistant and model-driven handoffs would end the current rule of no network service, which is the owner's decision.

## Next steps

All proposed, none agreed. Each is the owner's decision:
- Whether to adopt this premise, the word "app," and the draft prompt.
- Whether to save the premise as a draft pitch marked experimental, or make a shareable page of it.
- Which test apps to build, and whether to run the frame-strip experiment first.
- The steps in the previous entry: the rename and its timing, the boundary contract, and finishing Simple Sketch's v1 list.
