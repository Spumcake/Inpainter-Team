# Every overview written, a project map, and media-team's first strategies

**UTC:** 2026-10-18T12:55:26Z
**Covers:** since [media-team's mission, and a folder for each project](2026-10-18T00-27-13Z-media-team-and-project-folders.md). The owner and Claude Code worked through the mission documents one at a time, then made the workspace rebuildable, then gave media-team its first strategies.
**Repositories:** All pushed and clean, apart from this entry:
- Inpainter-Team is on `master` at `849e148`.
- Inpainter Studio is on `dev` at `c1ceb77`.
- Inpainter Public is on `main` at `80b3068`.
- inpainter.app is on `dev` at `4bcc09a`.
- Inpainter-Docs is on `dev` at `99d1870`.
- media-team is on `main` at `9442bbe`.
- Illustrious is on `dev` at `7f8099f`.
- spum-pipeline is on `main` at `52b1ed7`.

## What we did

**The effort's mission** now opens with the brand's positioning: Inpainter is a creative production studio that makes professional-quality, human-directed visual content, marketed through results. The owner's animation-pipeline paragraphs follow unchanged. The effort's parts are now a hierarchy:
- **Inpainter Studio,** the open-source software.
- **Inpainter Public,** the commercial side, which holds three projects:
  - inpainter.app, everything under the domain plus the closed platform;
  - Inpainter Docs, the public documentation for both;
  - media-team, the internal studio that makes Inpainter's own content, from trailers to original films.

Both team repositories' AGENTS.md were brought in line with it.

**Overviews and intent guides.** Each was drafted by Claude Code, cut by the owner to what matters, and saved in the owner's words:
- **inpainter.app:** the official brand and platform website for Inpainter, a computer graphics (CG) authoring application. At its core it is a media-generation orchestration platform: a creative interface over AI models, rendering services, and background jobs. Its intent guide has three points: marketing built around results, human-directed work, and a creative interface over the orchestration. The earlier landing-page proposal was retired; its checks stay in that version's tasks.
- **Inpainter Docs:** public documentation for using Studio and the platform, and for extending Studio, such as by writing skills. Claude Code set up its checkout under Inpainter Public, and the overview is its first commit.
- **Inpainter Studio:** a new opening, starting "the open-source computer graphics (CG) authoring application behind the Inpainter platform". The architecture rules stay below it, since contributors need them; the owner chose that over moving them to a guide. Its intent guide kept all eight points under the new title and kind.
- **Illustrious and RunPod:** short overviews in media-team's project folders, written to stand on their own without naming media-team.

**The project map.** Everything is on GitHub, but rebuilding the nested layout by hand would be hard. Claude Code wrote [project-map.md](../project-map.md). It records every repository's folder, address, and branches, the order to clone them in, and what GitHub does not hold, such as `.env` files. At the owner's request, the pipeline now checks the map for drift:
- at the start of a session that touches repositories, before every commit, and after any repository moves;
- a checkout that has lost track of its bare clone is repaired;
- a branch that exists only on this machine is named to the owner.

Two such branches were pushed: Studio's `scrap/v0.2-shared-session` and Inpainter Docs' `main`. Moving Inpainter Public had broken the inpainter.app, Illustrious, and RunPod checkouts' links again, and they were repaired.

**media-team's strategies.** The owner defined a strategy concretely: a workflow for one kind of job, with an example request likely to trigger it. The first workflow renders Illustrious images with no LoRAs from Danbooru post tags, as one image or a set. It has two strategies in `.project/strategies/`, one for this machine and one for RunPod, with an overview listing both. Each strategy runs the existing render skill and waits for the owner's yes to the plan; the RunPod one also states the cost.

While checking them, Claude Code found that the reorganization had broken rendering:
- Seven skills, AGENTS.md, README, and TEAM.md pointed at the old folders. They were fixed.
- Illustrious's runner looked for shared profiles and graphs in `.project/servers/` and `.project/workflows/`. With the owner's approval, its code now uses `.project/projects/illustrious/`. The 20 unit tests pass.
- The RunPod model-sync script found the team root by counting folders up from itself. It was fixed for its deeper location.

AGENTS.md now tells the agent to read the strategies before carrying out a request, since Claude Code only discovers skills in `.claude/skills/`.

## What we learned

- The owner's overviews are short: a paragraph or two, and no decisions the product has not made. Studio's is the exception, because its architecture rules guide every build.
- Moving folders breaks more than links. Code and scripts that find files by location broke silently. The project map's drift check now catches lost checkouts; it does not catch code paths.
- A strategy needs an example request, or nothing tells the agent when to use it.

## Where things stand

- A dry check of the Smooth Mix preset reports that both this machine and RunPod can run it. No image was rendered, so neither strategy has run end to end.
- The first paragraph of media-team's AGENTS.md still describes media-team as it was before its new mission.
- media-team's pipeline copy has neither the documentation skill nor the drift check.
- media-team's old diary sits inside Illustrious's documents.

## Next steps

- Proposed: run one strategy end to end with a real Danbooru post, starting locally, which costs nothing.
- Proposed: Studio's README, which still says "Inpainter" and calls the template the only sub-application; READMEs for inpainter.app, Inpainter Docs, Illustrious, and both team repositories.
- Proposed: intent guides for Illustrious and RunPod, from media-team's four retired fake tests: one request runs every phase, tools and servers are interchangeable, and nothing costs money before approval.
- Awaiting the owner: what the empty `inpainter-public` project folder in Inpainter Public is for.
- Waiting on the owner's review: media-team's pipeline, its AGENTS.md opening, and where its diary lives.
- Deferred until a version builds intermediates: a Studio intent point for shaping staged intermediates before the final render.
