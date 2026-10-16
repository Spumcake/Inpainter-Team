# Inpainter 0.3 post-mortem

The owner closed version 0.3 on 2026-10-16. They had run it on their own machine, found that Claude Haiku 5.5 and Claude Sonnet 5.5 work well, and judged it "good enough for now". The [pitch](PITCH.md) and the [task list](tasks.md) are the record of what the version set out to deliver.

Version 0.3 opened at about 20:00 UTC on 15 October 2026, when its pitch was written, and closed at 14:38 UTC on 16 October 2026, about 19 hours later.

## What was built

The assistant now sits in a chat panel on the left of the shell. Sub-applications declare skills through the protocol. The assistant, running through OpenRouter, is offered every running sub-application's tools. The host routes each call to that application's program and passes its answer back. The host and the assistant know no skill or tool by name. Simple Sketch provides 26 tools covering what a person does to a sketch's contents by hand, including a picture of the canvas so the assistant can understand freehand work. The template provides one skill, which renames its tab: the mission's proof that the assistant learns its abilities only from sub-applications. While a request runs, the applications are locked, and one undo takes the whole request back in Simple Sketch's own history.

The workspace keeps the open files, the tabs, and the chat together in a `.inpainter` file that never holds the work itself. It is reached from the logo menu, which offers New Window, Open, Recent, Save, Save As, Clear, and Quit. Closing an unsaved workspace offers to save it. Checks run the real window code against real application programs and a scripted stand-in for OpenRouter, so they cost nothing. The work is Inpainter `1c4d2c6`.

## What the owner decided

On 15 October the owner chose OpenRouter. They treated the generated mockup as a guide to layout only, and listed the panel's controls: attachments, the model picker, a microphone that does nothing yet, and send. They asked for the assistant's full reach rather than one example, and for the mission's proof. They decided the applications lock while the assistant works. Partway through, they added the workspace to the mission and to this version, with the logo menu as its entrance, and approved two generated renders for it. On their first trial they chose Claude Haiku 5.5 as the default model, and asked for a short picker of recent, well-known models. What departed from the plan is under "As built" in [systems.md](systems.md).

## Left outside this version

These stay part of the product: voice input behind the microphone; one request planned across several sub-applications; any second creative app; and an app sending the host paint commands. Also left out: a check that compares the assistant's canvas picture with the screen, and JSON types on some tool properties, which stricter model providers may require.

## What went wrong and what was learned

- **The first plan missed the point.** It modeled Simple Sketch's skills on the circle-and-square example rather than on how a sketch is made. The intent review failed it, and the skills were rebuilt from Simple Sketch's own hand actions. A coverage check now enforces that.
- **The workspace plan had gaps.** It would have lost the Template tab on reopening, and saved copies of the drawing inside the chat history. The review caught both before any code was written.
- **The final review found risks a real model would hit,** and they were fixed before the trial. A request to recolor part of a drawing would have recolored all of it. The conversation would have grown without limit. Models that cannot see pictures would have failed.
- **The owner's first trial failed.** The default model resolved to a batch-only variant that cannot hold a conversation, and the picker listed every model OpenRouter offers. Choices made against a live catalog need checking against that catalog.
- **The toolbar is clipped.** On the owner's screen the window opened narrower than planned, which cuts off Simple Sketch's Size control. It is not fixed.
- **Not everything was tried.** The trial covered drawing with the default and other models. Whether the owner tried the workspace round trip, recoloring part of a freehand drawing, or New Window was not recorded.
