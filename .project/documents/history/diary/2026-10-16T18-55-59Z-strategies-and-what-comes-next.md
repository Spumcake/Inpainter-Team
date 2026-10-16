# Strategies, and what comes after 0.3

**UTC:** 2026-10-16T18:55:59Z
**Covers:** since [the assistant and the workspace](2026-10-16T14-39-45Z-the-assistant-and-the-workspace.md). This stretch was mostly discussion about what follows version 0.3. It changed the mission and set up one new repository; no code changed.
**Repositories:** The team repository is on `master` at `0f5f9a7`, with the mission and AGENTS.md changes below uncommitted. Inpainter is on `dev` at `1c4d2c6`, clean. Inpainter-Strategies is new: `dev` and a local `main` at `29b1b58`, with `dev` pushed and `main` not.

## What we did

The owner opened a Claude Code consultancy thread to decide what comes after 0.3. They want the core framework to come before more sub-application interface work, and raised four ideas: how a person manages and upgrades Inpainter, the Inpainter platform (a media-generation orchestration service behind the commercial product), a roadmap skill in the pipeline, and a repository of the assistant's strategies.

On strategies, the owner set the idea out as a hierarchy, modeled on the pipeline. Each sub-application's skills cover only its own work. Above them, strategies are skills about the workspace as a whole: which sub-applications to use, and in what order, for a kind of problem. The owner's example was a workflow that builds a character LoRA dataset from a short description and reference tags. The assistant first produces the workflow as an intermediate step, the person edits its ingredients, and then the assistant carries it out. What the person learns while reviewing goes back into the strategy. Strategies live apart from the code because they will change more often than it does.

- With the owner's agreement, Claude Code added the hierarchy to the opening paragraph of the [mission](../../concept/MISSION.md).
- [AGENTS.md](../../../../AGENTS.md) names https://github.com/Spumcake/Inpainter-Strategies and its checkout.
- The owner decided that strategies use the same file format as the pipeline's skills, so a person can write their own.
- The owner also decided that the assistant's runtime prompt belongs in that repository. It is fixed text in the `assistant` crate today, and moves when a version builds the strategy loading.
- Claude Code set up the repository as `src/inpainter-strategies/worktrees`, the same layout as Inpainter. Its first commit is a README describing strategies and their format. At the owner's request, only `dev` is pushed. `dev` is the development branch; `main` is for installed versions.

Claude Code proposed how Inpainter would reach strategies: a development build reads the checkout, so an edited strategy takes effect on the next request with no rebuild; an installed copy reads strategies from its data folder, installed with Inpainter and updated from releases of the Strategies repository. This was not decided. How installed copies receive updates belongs with the upgrade path.

## What we learned

Claude Code first described the strategies as a workflow sub-application with a review step written into the mission. The owner set the simpler model: strategies are just skills one level up, discovered the way the pipeline's skills are. A strategy guides the assistant but never replaces discovery from each sub-application's own skills, so the mission's test with an unknown sub-application still holds. The runtime prompt has to stay generic for the same reason.

## Where things stand

The mission names the skill hierarchy. The Strategies repository exists with a README and no strategies. Inpainter does not read strategies yet; its runtime prompt is still in the code. No version is open.

## Next steps

- Proposed, awaiting the owner: the next version proves one request planned across two sub-applications, with a small test sub-application as the second.
- Proposed, awaiting the owner: Inpainter loads strategies from the repository, and the runtime prompt moves there.
- Proposed, awaiting the owner: the upgrade path, which includes how installed copies receive strategy updates.
- Proposed, awaiting the owner: a roadmap skill, which would be a change to spum-pipeline.
- Proposed, awaiting the owner: the Inpainter platform gets its own mission when the owner is ready, and the assistant's model connection stays swappable until then.
- Awaiting the owner: commit the mission and AGENTS.md changes, and this entry.
- Carried over from 0.3, proposed: fix the clipped toolbar on smaller screens, and try the untried parts of 0.3 with a real model.
