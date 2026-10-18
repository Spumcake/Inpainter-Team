# Development

How we build software in this project. It is project-agnostic: the project's own rules are in AGENTS.md, what the whole effort is lives in the mission, and what each project is lives in its overview. The person who owns the product and decides what it is, is the owner.

## Documents and their authority

- **The mission,** `.project/documents/MISSION.md`, says what the whole effort is, across every project and version. It outranks every other document. The **mission** skill drafts it.
- **Each project's overview,** `docs/overview.md` in that project's repository, says what the project is in every version: its lasting structure, boundaries, stack, and references. It outranks every other document about the project and agrees with the mission.
- **Each project's folder,** `.project/projects/<project>/`, holds everything about that project: its `documents/`, which are its guides, the version in progress, and its history, and any other material it needs, such as presets, workflows, or scripts. The team's own documents, the mission and the diary, are in `.project/documents/`.
- **Guides,** `documents/guides/<subject>-guide.md` in a project's folder, hold lasting knowledge by subject. A standards guide holds what the owner approved; the project's intent guide, `documents/guides/intent-guide.md`, names what it exists to prove and the fakes that would miss it. A findings guide holds evidence, and the owner decides scope from it.
- **The version folder,** `documents/development/<version>/` in a project's folder, holds the version in progress: its pitch, tasks (scope and acceptance), systems (contracts), reuse map, plan, and research briefs. The **version** skill opens and closes a version. These documents must agree with the mission, the overview, and the standards guides. Leaving something out of a version defers it; it does not remove it from the product.
- **History** is only added to: the diary, `.project/documents/diary/`, one stream for the team across its projects, and each project's closed versions and their post-mortems, in `documents/history/<version>/` in its folder. It explains how the project got here and is never an instruction. A correction goes in a new record.
- **INTERFACE.md,** at the team root, describes the current screens, only from visual references.

The mission, each overview, and each standards guide are the owner's. Draft a change with the **mission** or **documentation** skill, show the owner the exact lines added, removed, or replaced, with one sentence on why, and save only what they agree to, in their words if they edit it. Never change one to make a version or an implementation fit. When a request, a document, or your own work contradicts one of them, stop and ask the owner. The **documentation** skill says where every other document belongs and what may go into a public repository.

Documents hold decisions, not open questions. Write an owner's decision into the document that owns it, dated ("as the owner set on 2026-10-14"). Honor a project's existing paths and older documents, such as a todo or testing file, until the owner asks to change them.

## References come first

A reference is an image the owner supplied or approved, the running reference app, or a reference codebase, as the overview or the owner names them. Build what a reference shows and does, completely, and nothing it does not. Every visible element and behavior traces to a reference or a dated owner decision. Anything that traces to neither is not built.

- When a reference codebase covers the work, use the **appropriation** skill before writing contracts or code. Once the owner approves the reuse map, copy each file unchanged first, then make only the mapped edits. Rewriting what the reference already does, even more cleanly, is a defect.
- A view with no visual reference is not designed in words. Ask the owner to supply one, or offer generated renders for approval with the **interface** skill.
- When a feature has no reference, ask the owner. If they want to know what comparable software does, use the **journeys** skill; its findings are suggestions, and the owner decides scope.

Lean is a rule for the code, not for the feature: the fewest parts that deliver the agreed scope, never a thinner scope.

## Before building a slice

Build nothing until three things hold:

1. every view the slice shows has an approved visual reference;
2. when a reference codebase covers the slice, the owner has approved its reuse map;
3. the owner has agreed one or two sentences saying what they will see and be able to do when they run it, including the lasting structure the overview says is always present.

Before the owner approves a plan that decides structure, such as a reuse map, contracts, or a slice's criteria, run the **intent-reviewer** agent on it and bring its verdict with the plan. If the owner changes a slice's scope partway through, agree the sentences again.

## Building

You own the outcome and its integration. Work that is one piece, you do directly. Work with independent parts, you split as "Working in parallel" describes. A subagent also earns its place for fresh eyes (the intent reviewer) or a broad search that would flood the conversation. Every subagent starts with none of this conversation, so its brief carries the owner's outcome, links to the mission and the project's overview, and where to stop.

When an app repository has its own AGENTS.md or skills, follow them, and update them in the same change when the work changes what they describe.

When the owner will try a frontend, hand them the real screen: the persistent controls and regions the product uses to operate it, as the references show them. A blank window, or a screen with that chrome removed, is not a trial, and a headless or protocol check does not replace it.

Tests catch failures that matter. Run the existing suite when a change is complete. Do not write tests that pin behavior the owner has not approved yet. Tests that prove a boundary, or protect stored data, are welcome at any time. Never weaken a correct test to get a pass, and do not answer a stuck check by adding more tests.

## Working in parallel

Speed matters as much as cost. Do one thing at a time only when that gives the better result. Concurrency comes from splitting the work by part of the product, never by kind of job: a chain of specialists, such as design, then plan, then build, then check, runs one step at a time and loses some of the owner's intent at every handoff.

- **Within a turn:** send independent reads, searches, builds, and checks together, in one batch of tool calls.
- **Within a slice:**
  1. **Settle the shared contract first,** such as the messages, interfaces, or schema the parts meet at, and write it in systems. This is the one step done alone, because everything else builds on it.
  2. **Split by part of the code.** Each assignment owns its folders and files, with one writer per file. Work that must change a shared file stays with one agent.
  3. **Give each part its own worktree** (the **worktrees** skill) and a self-contained brief: the outcome, the mission and the overview, the contract, the paths it owns, its reuse-map entries, the checks to run, and where to stop. Anything not in the brief does not exist for that agent.
  4. **Launch the parts together, in the background,** and meanwhile do the next independent piece yourself instead of waiting.
  5. **Integrate in one place.** Bring each result into `dev`, resolve conflicts by reading both sides, run the full suite once, then run the intent review on the whole.
- **Across a version:** while one slice is built, prepare the next one alongside, such as its reuse map, image prompts, or research, as long as it does not depend on decisions the current slice will make.
- **Reading:** before a decision that needs several areas understood, send a few agents to read one area each and return short summaries, instead of reading them one after another.

Stay sequential for:
- decisions the owner has not made;
- designing the contract;
- integration;
- a single shared file;
- pieces small enough that the brief would cost more than the work.

Each parallel agent rereads its context, which costs usage. So prefer two or three substantial, well-bounded parts over many thin ones. When reporting, say what ran in parallel, what did not, and why.

## Done

A slice is done when it meets its agreed sentences and acceptance, and the intent reviewer finds its point achieved. "It works, but it misses the point" is a failure, not a suggestion. Run that review before asking the owner to try or commit the slice.

Report what changed, what you checked by running it, what you only inspected, and what nobody has checked yet. Record departures from the plan as "As built" in the document that owns them, and keep the plan current. When a session changed the project, offer a diary entry in one line at the end; write it with the **diary** skill only if the owner accepts.

## Speaking to the owner

The owner knows the product and does not read the code. Every message follows the same order:

1. The outcome first, in a sentence or two, and whether the news is good or bad.
2. What it means for them, as a person using the product would notice it: "undo does nothing", not a function name.
3. Why, briefly, when it matters for what happens next, including your own part in it.
4. What you need from them: a clear choice with your recommendation, a command in its own code block, or "nothing; I'll continue with …".
5. Details last, and only as far as they help.

Use the product's words. One idea per sentence. Most messages fit in 200 words. Never end on a colon.

Ask a question that changes the product, scope, stored data, dependencies, spending, or anything hard to reverse. Decide routine choices inside the agreed scope yourself. Put related questions together, most consequential first, each with options and your recommendation. Keep suggestions apart from questions. When the owner gives feedback, account for every item at the end: done, done differently and why, or not done and why.

## Git

The **worktrees** skill describes the layout: a team repository for documents and this pipeline, and app repositories under `src/<repo>/worktrees/`.

- Commit only when the owner asks, and push only when they ask. Before committing in an app repository, run the branch check in the worktrees skill.
- Team-repository commits about app work carry an `App-Commit: <short hash> (<owner>/<repo>)` trailer.
- Never force, rewrite history, tag, or change remotes. The owner tags releases and merges into `main` on GitHub.
- When the owner asks for a build or a release, use the **build** skill, then the **release** skill.
- Keep secrets, `.env` files, and large generated payloads out of commits; say what you left out.
