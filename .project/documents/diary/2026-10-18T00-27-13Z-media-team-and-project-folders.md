# media-team's mission, and a folder for each project

**UTC:** 2026-10-18T00:27:13Z
**Covers:** since [the effort reorganized](2026-10-17T20-31-47Z-the-effort-reorganized.md). This stretch was mostly discussion of media-team's place in the effort, and a change to how every team repository arranges its `.project` folder.
**Repositories:** Inpainter-Team is on `master` at `43d77a2`, with this entry uncommitted. Inpainter Public is on `main` at `f3788ae`. spum-pipeline, at `/mnt/Hot/Source`, is on `main` at `f3d8a35`. media-team is on `main` at `9dca382`.

## What we did

**media-team's mission.** The owner set out where media-team came from, and three origins are converging in it:
- The owner's own agentic workflows for generative media: ComfyUI on this machine, and the Illustrious and RunPod repositories, proven in Cursor.
- A team of specialized agents, first explored with Grok Bots. The roles covered editing, prompt engineering, media routing, the continuity of a fictional world, art direction, literary analysis, Google APIs, campaigns, and content strategy. The idea was a repository-driven production pipeline. The creator commits notes, character sketches, and stories, and the agents turn them into production-ready assets.
- Promotional and branded content for Inpainter as a commercial product.

The owner also settled two points:
- **The brand's positioning** is "a creative production studio that makes professional-quality, human-directed visual content". Its marketing is built around results.
- **The clients it aims to attract** are creative agencies, independent animation studios, and small viral brands.

media-team is not a product. It is an internal tool: the team makes Inpainter's own content, and tries tools there first. Internal tools are experiments and proofs of concept, and those that would work well in the commercial product graduate into platform tools. Client commissions are not offered, though the pipeline as conceived would take them easily.

Claude Code drafted media-team's mission from these words, and the owner approved it. It replaced media-team's earlier mission, which described only the automation tooling. One rule was kept from the old mission: nothing runs or costs money before the creator approves the plan, and the creator chooses what to keep.

**A folder for each project.** While rearranging media-team, the owner saw that a project's folder holds more than documents. Illustrious has presets, workflows, server profiles, and scripts beside its plans. So `.project` now has two levels:
- `.project/documents/` holds the team's own documents: the mission and the diary.
- `.project/projects/<project>/` holds everything about one project. Its documents go under `documents/`: guides, the version in progress, and its history. Other material sits beside them.

The owner asked for every team to match. Claude Code moved Inpainter-Team and Inpainter Public, fixed their AGENTS.md and INTERFACE.md paths, and changed the pipeline so every team uses this layout.

**One diary per team.** media-team's past entries sit inside Illustrious's documents. The owner decided each team keeps one diary, in `.project/documents/diary/`, since entries often span projects.

**Inpainter Public moved.** The owner moved Inpainter Public into Inpainter-Team's `src/`. Its AGENTS.md now finds the mission two folders up.

## What we learned

- A per-project folder that holds only documents pushes presets and workflows somewhere unrelated. Naming the folder for the project, with documents inside it, keeps everything a project owns together.
- Making the brand's positioning explicit settles what media-team's work must look like: results that would persuade agencies and studios.

## Where things stand

- The pipeline and both Inpainter team repositories use the new layout, and every path in their AGENTS.md and INTERFACE.md was checked.
- media-team's mission and folder changes are committed and pushed with this entry. Its server snapshot stays out of Git at its new path, as it was before.
- media-team's AGENTS.md and its own pipeline copy still describe the old mission and paths.
- Its diary has not yet moved to the team level.
- Its project overviews for Illustrious and RunPod are empty.

## Next steps

- Agreed, waiting on the owner: update the effort's mission. It should say what the brand's positioning is, and name the original films. Clients are not taken yet, but the pipeline could take them.
- Proposed: bring media-team's AGENTS.md, pipeline copy, and diary in line with its new mission and layout, when the owner wants to discuss media-team's pipeline.
- Proposed, awaiting the owner: where an internal project's overview lives, such as Illustrious's and RunPod's. The pipeline currently puts overviews in the project's own repository.
- Proposed: move Inpainter-Team's renders, which all belong to Studio, into `projects/inpainter-studio/`.
- Still open from [the previous entry](2026-10-17T20-31-47Z-the-effort-reorganized.md): the overviews for inpainter.app, Inpainter-Docs, and Studio, and the team repositories' READMEs.
