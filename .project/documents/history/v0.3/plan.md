# Inpainter 0.3 — plan

The owner agreed 0.3's sentence on 2026-10-15: the assistant in a chat panel on the left reads the open sketch and does there anything a person can do to its contents by hand, as ordinary work; and the template's skill proves that the assistant learns its abilities only from sub-applications. The owner then added the workspace to the mission and to 0.3. The documents are approved. Building starts with the contract.

## Now

- [x] Run the intent review on tasks and systems. It failed the first draft, whose skills covered shapes but not sketching. The revised documents passed, with three residual risks folded into systems.
- [x] The owner approved the documents, the two workspace renders, and the sentence on 2026-10-15. The intent review's workspace findings were fixed in systems.
- [x] Write the contract alone: the protocol's skill and workspace messages with their tests, and the `assistant` crate's public API with a placeholder loop. It is uncommitted in `dev` on top of `1cd410f`, and the protocol tests pass.
- [x] Build the three parts in parallel, in `work-skills`, `work-assistant`, and `work-panel`.
- [x] Integrate them into `dev`, and write the checks for skill discovery, one owner, and the workspace. All 145 tests pass. The intent review passed the whole, and its fixes before the trial are in.
- [x] The owner tried it with real models and closed the version on 2026-10-16. The app work is Inpainter `1c4d2c6`. See the [post-mortem](post-mortem.md).

## The contract

Done on 2026-10-15. The `skills`, `invoke`, and `result` messages, and the workspace's `path`, `saves`, `open`, `keep`, and `kept`, in `protocol`, and the request and event types the host and `assistant` exchange. Done alone, first, because every part builds on it. Done when it compiles and the protocol tests cover the new messages.

## Three parts in parallel

Each part owns its folders, with one writer per file:

1. **Simple Sketch's skills,** in `apps/simple-sketch` and `client`: tools for Simple Sketch's whole hand surface, as systems lists it, through the session's own operations, including the looking tool's picture of a canvas; one undo step per request; and `sketch-client` driving every tool, with the coverage check, and the looking tool's picture compared with the host window. Plus `open`, `keep`, and `--restore` in Simple Sketch, the template's renaming skill in `apps/template`, and the scaffolding skill updated to describe skills. Done when `sketch-client` passes. Covers A-2's tools, A-3's undo, and A-7's skill.
2. **The assistant,** in a new `assistant/`: the OpenRouter client, the model list, the loop, the stop, and the step limit, tested against the scripted stand-in. Done when its tests pass with no network. Covers A-4's list, A-8's failures, and A-9.
3. **The panel and the workspace,** in `host/`: the column, the logo menu, the conversation, the input box, the chips, the model picker, the microphone, send and stop, the lock, the routing of `invoke`, the workspace file and its flows, and the settings. Done when the window matches INTERFACE's Assistant against a stand-in model, and the workspace check passes. Covers A-1, A-5, A-6, A-9, and W-1 to W-5.

## Integration and trial

Bring the three parts into `dev`. Run the full suite and `sketch-client`, then the skill-discovery and one-owner checks with the fixture. Then run the intent review on the whole. The owner then tries the agreed sentence with a real model on their machine. Done when the owner confirms A-1 to A-9 and W-1 to W-5.
