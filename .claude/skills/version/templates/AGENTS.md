# AGENTS.md — [PLACEHOLDER: project name]

[PLACEHOLDER: at most two sentences summarizing the mission: what is being built and for whom.] The mission is in `.project/documents/MISSION.md` and outranks every other project document. What each project is in detail is in its overview, [PLACEHOLDER: each project's `docs/overview.md`], and the fakes it is judged against are in each project's `.project/projects/<project>/documents/guides/intent-guide.md`. Only the owner changes these. The current version's scope and acceptance criteria are in [PLACEHOLDER: tasks path].

The stack is [PLACEHOLDER: the agreed stack, or remove this line].

**Language:** English for code, comments, documentation, logs, commit messages, and generation prompts, unless the project requires another language.

## Starting and resuming work

Read the mission, the overview, and the intent guide, then the current milestone in [PLACEHOLDER: plan path] and the latest diary entry in `.project/documents/diary/`, if one exists. Before a change, read its acceptance criteria and contracts. Before visual work, read `INTERFACE.md` and the references it cites.

## References

[PLACEHOLDER: each reference the overview names, with its path and what it is the reference for, and the reuse map that governs carrying it over. Omit when none.]

## Repositories

[PLACEHOLDER: the team repository URL, each app repository URL and its worktree path, and any dependency rules between the project's parts. Omit when the app lives in this repository.]

The project map, `.project/documents/project-map.md` [PLACEHOLDER: or the outer team repository's map], records where every repository lives and how to rebuild the workspace.

## Verification

[PLACEHOLDER: the agreed sentence for what running the current slice shows, the command that runs it, and where acceptance criteria live. Name the project's essential journey.]

## Boundaries

[PLACEHOLDER: established limits only: stack exclusions, data, spending, external actions. Do not invent budgets or rules.]

Keep secrets out of version control. Commit and push only when the owner asks.
