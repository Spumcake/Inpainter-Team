# Inpainter 0.3 — tasks

**Sources:** [Mission](../../concept/MISSION.md), [pitch](PITCH.md), [INTERFACE](../../../../INTERFACE.md) (Assistant), and [systems](systems.md). The look of the chat panel is [agentic-harness-mockup.png](../../../renders/ui/agentic-harness-mockup.png), which approximates layout and look only, as the owner set on 2026-10-15. No reference codebase covers the assistant, so there is no reuse map. The panel is drawn with the host's carried-over widgets and colors. Sketching is Simple Sketch as version 0.2 left it.

**Objective and exclusions:** Version 0.3 delivers the assistant, as the owner set on 2026-10-15: a working chat assistant in the shell that reads the active document and changes it through tools the sub-application provides. The models come through OpenRouter. The panel has the conversation, the input box, attachments, the model picker, a microphone that does nothing yet, and send. The version includes the mission's proof that the assistant learns its abilities only from sub-applications. It also delivers the workspace the mission describes, as the owner set on 2026-10-15: the host's file that keeps the open files, the tabs, and the chat history together, reached from the logo menu at the chat panel's top-left. Left for later, and still part of the product: voice input; one request planned across several sub-applications; any second creative app; a sub-application drawing its own view with paint commands.

**The agreed sentence,** as the owner agreed on 2026-10-15 after adding the workspace: Inpainter opens with Simple Sketch and the chat panel on the left. Anything I can do to a sketch's contents by hand, I can ask the assistant for in plain words; it looks at the open canvas and does it there as ordinary work, while the tools wait, and one undo takes a request back. The template, which the assistant was never told about, provides a skill it uses when asked. From the logo menu I save my workspace and reopen it later with the same tabs and the same chat, and closing an unsaved one offers to save it.

**Version convention:** 0.3 is the agreed working version. Do not tag a candidate until the owner asks. After that, candidates use `0.3-candidate.<n>`.

## Acceptance criteria

A-1 The panel. `cargo run -p inpainter -- --app simple-sketch` opens the shell with the chat panel on the left, about a quarter of the window's width, as INTERFACE's Assistant describes. The tab strip, toolbar, canvas, grid, and properties panel are to its right and work as in 0.2. With `--app template` the panel is the same. Checked by the owner trying it.

A-2 Asking for work. With a sketch open on Canvas 1, the person types a request and sends it. Their message appears in the conversation. The assistant reads what is on the open canvas, makes the change there, and replies saying what it did. Each change appears on the canvas as the assistant makes it. Examples the owner tries, none of them special-cased: "create a circle and a square next to each other"; "make the circle pink and dashed"; "write 'Hello' above them in large text"; "put a line between them"; "delete the square"; "bring the circle to the front"; "what's on this canvas?". Anything a person can do to a sketch's contents by hand can be asked, as systems lists it from Simple Sketch's own commands, gestures, properties, and selection: freehand strokes in a palette color and erasing part of them without destroying them; shapes and text; cut, copy, paste, delete, group, ungroup, align, rotate, flip, and order; every property the panel edits on objects, text, and palette colors, such as color, opacity, size, dash, cap, visibility, and lock; the selection; and canvases. Undo and redo stay with the person. The owner also draws something freehand by hand, asks what it is, and asks for a change to part of it, such as "make the cat's ears pink"; the assistant looks at the canvas to understand it. A request the skills cannot carry out gets a reply saying so, and nothing changes. Checked by the owner trying it, and by the no-host check for each tool.

A-3 One flow with hand work. After the assistant draws, the person moves or restyles its objects by hand, and the next request works from where they are now. The person selects objects by hand and asks "make these blue", and the selected objects change. Undo removes the assistant's work the way it removes the person's, in Simple Sketch's own history, and redo brings it back. Saving saves it like any other work. The host keeps no history of its own. Checked by the owner trying it, and by the one-owner check in systems.

A-4 The model picker. The picker lists the OpenRouter models that can use tools. The chosen model answers the next request. The choice is remembered in Inpainter's settings the next time it opens, as the owner accepted on 2026-10-15. Checked by the owner trying it.

A-5 Attachments. The plus opens the desktop's file dialog for pictures and text files. Chosen files show as chips above the text field and can be removed. When sent, the model receives them with the message, and their file names show under it in the conversation. Asked "draw something like this", the assistant uses the picture as a guide. Checked by the owner trying it.

A-6 The microphone. The microphone shows in its place and does nothing when clicked, as the owner set on 2026-10-15. Checked by the owner trying it.

A-7 The proof. With Simple Sketch and the template running in one window and a Simple Sketch tab selected, the person asks the assistant to rename the Template tab. Every running sub-application's skills are on offer. The assistant picks the template's skill among them, and the Template tab's label changes. Nothing in the host's or the assistant's code or prompts names the template, Simple Sketch, or any of their skills or tools. Checked by the owner trying it, and by the skill-discovery check in systems.

A-8 Without the assistant. With no OpenRouter key, Inpainter opens and Simple Sketch does everything it did in 0.2. The `sketch-client` no-host check still passes. Sending a request then replies that no key is set. A network failure or a model error also shows as a reply, and the window never waits on the assistant. Checked by the owner trying it, and by running `sketch-client`.

A-9 Working and stopping. While the assistant works on a request, the applications are locked, as the owner set on 2026-10-15: every sub-application's toolbar, menu rows, properties panel, and drawing on the canvas are greyed out and do nothing, and their shortcuts do nothing, until the request finishes or is stopped. Panning, zooming, and switching tabs still work, so the person can watch. The send button becomes a stop button. Clicking it stops the request after the change in progress. Work already done stays, and undo removes it as usual. Checked by the owner trying it.

W-1 Saving and reopening a workspace. With a saved sketch and an untitled one in Simple Sketch tabs, a Template tab, and a conversation, the person chooses Save Workspace from the logo menu and picks a file name. Simple Sketch first asks for a file name for the untitled sketch, with its own Save, as the owner set on 2026-10-15. The person quits, starts Inpainter, and opens that workspace: the same tabs come back in the same order, with the same tab selected and each canvas where it was, and the conversation is back, so the assistant can continue from it. The workspace file holds each sketch's path, not the sketch: a change saved to that sketch on its own shows when the workspace reopens. These are the mission's workspace test. Checked by the owner trying it, and by the workspace check in systems.

W-2 Closing an unsaved workspace. When the person quits, by the window's close or by Quit in the logo menu, and the workspace has changes it has not saved, Inpainter asks "Save this workspace?" with Save, Discard, and Cancel, in the existing dialog look. Then Simple Sketch asks about each unsaved sketch, as in 0.2. Cancel keeps everything open. A workspace with nothing in it but a fresh untouched tab is not asked about. Checked by the owner trying it.

W-3 The logo menu. Clicking the logo at the chat panel's top-left opens the workspace menu: New Window, Open Workspace…, Recent Workspaces, Save Workspace, Save Workspace As…, Clear Workspace, and Quit, as the owner set on 2026-10-15. Open Workspace offers to save the current one first, then replaces its tabs and conversation. Recent Workspaces lists the workspaces opened or saved most recently and opens one when chosen. Checked by the owner trying it.

W-4 New Window and Clear Workspace. New Window opens another Inpainter window with a fresh workspace and an empty conversation. The first window is unchanged. Clear Workspace offers to save the workspace, closes every tab, letting each sub-application ask about unsaved work, and empties the conversation, leaving one new tab as when Inpainter starts. Checked by the owner trying it.

W-5 Files join the workspace. A sketch opened with the hamburger menu's Open, or created with the plus, joins the current workspace, and the workspace's conversation continues with it, as the owner set on 2026-10-15. The same sketch may belong to two workspaces, each with its own conversation. Checked by the owner trying it.

## Running it

From `src/inpainter/worktrees/dev`, with the key in the environment:

```bash
OPENROUTER_API_KEY=… cargo run -p inpainter -- --app simple-sketch --app template
```

Each request costs money on the owner's OpenRouter account, priced by the chosen model. There is no seed sketch: a new sketch is the starting state. Nobody has run these commands yet for 0.3.

## Checks

Run tests with `TMPDIR=$PWD/target/tmp` from the app worktree. `cargo test` runs the protocol and assistant tests. `cargo run -p sketch-client` is the no-host check, which gains a drive of every Simple Sketch tool and the coverage check. The checks that need no model are scripted. They use a scripted stand-in for OpenRouter, so they spend nothing. A-1 to A-7 and A-9 are judged by the owner trying the real window with a real model. Results are recorded here as they happen.

**Results on 2026-10-15, uncommitted on `1cd410f`:** `cargo test` passes all 145 tests, and `sketch-client` passes, covering every tool, one undo per request, the workspace messages, and coverage. The scripted checks cover the mechanics of A-2, A-3, A-7, A-8, and W-1 with no real model. The window was captured on a virtual display with the panel, the logo menu, the save question, and the lock. Nobody has tried A-1 to A-9 or W-1 to W-5 with a real model yet; that is the owner's trial.
