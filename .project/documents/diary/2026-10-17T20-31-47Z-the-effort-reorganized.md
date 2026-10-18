# The effort reorganized, and the documents it still needs

**UTC:** 2026-10-17T20:31:47Z
**Covers:** since [strategies, and what comes after 0.3](2026-10-16T18-55-59Z-strategies-and-what-comes-next.md). This stretch was about how the effort's documents and repositories are organized. No product code changed.
**Repositories:** All pushed and clean, apart from this entry. Inpainter-Team is on `master` at `2a4273c`. Inpainter Studio, formerly Inpainter, is on `dev` at `0749a9a`. Inpainter Public is on `main` at `a6f1ddf`. spum-pipeline, checked out at `/mnt/Hot/Source`, is on `main` at `de63691`. The inpainter.app checkout under Inpainter Public has untracked work and no commits; Inpainter-Docs is empty on GitHub.

## What we did

**A mission for the whole effort.** The owner found that the mission described the desktop software, not the effort. With Claude Code they wrote a new, short one: Inpainter offers a human-directed, AI-native animation pipeline that stops between the brief and the final rendering pass, with staged intermediates the person can see and change. Animation is where it starts, not where it ends. It names two parts:
- **The software** is open source, and works fully without the platform through the person's own endpoints, such as OpenAI, fal.ai, or ComfyUI.
- **The platform** is closed and commercial. It runs its own ComfyUI instance and comes with skills that work only with it.

The old mission's detail moved unchanged into the software's `docs/overview.md`. Its fakes moved into an intent guide, and AGENTS.md was pointed at all three.

**The documentation skill.** The owner asked for rules on what goes where, since documentation will become public. Claude Code wrote the **documentation** skill in spum-pipeline:
- Each document's home follows from its readers and how long it stays true.
- Each project's repository holds its README and `docs/`, including its overview. Development documents stay in the team repository.
- Public documents hold no owner conversations, open questions, costs, or secrets.
- A guide is marked as standards the owner approved, or findings that inform without setting scope.

The mission skill now writes only the short mission. The intent reviewer reads the mission, the project's overview, and its intent guide. The approval rule for these documents is stated once, in development.md.

**The owner's reorganization.** On 2026-10-17 the owner filed documents by project, and the pipeline adopted the layout for every project:
- `.project/documents/projects/<project>/` holds a project's guides, its version in progress, and its history.
- The team repository is a project too, and holds the mission.
- The diary has its own folder.

The owner also made these changes:
- Renamed the app repository to Inpainter-Studio.
- Moved the strategies into its `strategies/` folder and deleted the Inpainter-Strategies repository; the latest strategies come from the source.
- Created Inpainter Public for everything that isn't the Studio app:
  - everything under the inpainter.app domain (the website, inpainter.app/dashboard, login.inpainter.app, api.inpainter.app) and the media-generation orchestration platform behind them;
  - Inpainter-Docs;
  - media-team, a team repository first meant for original films made with Inpainter. It is on hold until the owner discusses it.

A web version of the apps would still belong to Studio.

**Repairs.** Claude Code set up Inpainter Public as a team repository. It reconnected the moved inpainter.app and Studio checkouts, pointed Studio's remote at its new name, and fixed every path in both AGENTS.md files and INTERFACE.md. Claude Code had cloned a second copy of spum-pipeline under `projects/` by mistake. Its changes were moved to the real checkout at `/mnt/Hot/Source`, and the copy was deleted.

Known only from documents: another session built a local landing-page reference for inpainter.app and proposed an overview and intent standards for it. These are in Inpainter Public, under `projects/inpainter.app/development/v1.0/`, unapproved.

## What we learned

- The mission had grown into the software's specification, which made it the wrong north star once there was more than one project.
- Several of Claude Code's drafts went into detail the owner had not asked for. Mission work goes best in the owner's words, kept short.
- A repository named for its owner, such as Studio's strategies folder, avoids the confusion a separate repository caused.

## Where things stand

The effort's mission, Studio's overview, and Studio's intent guide exist. Everything else in the next steps is missing or out of date. Diary entries before this one keep links to the old paths, since history is not edited.

## Next steps

Mission documents, to be written with the owner one at a time, most consequential first:

1. Agreed: **update the effort's mission.** It names the software and the platform, but the owner now describes three parts: the app, the public-facing side, and original films made with it. The films are missing, and the public side is narrower than inpainter.app now covers. Waiting on the owner for a sentence or two on what the films are for.
2. Proposed: **inpainter.app's overview and intent guide.** The existing proposal covers only a landing page; the scope is now the whole domain and the platform.
3. Proposed: **Inpainter-Docs' overview and README.** Its purpose is not yet written down.
4. Proposed: **refresh Studio's overview, README, and intent guide.** It still says "Inpainter", its README says the template is the only sub-application, and the guide needs the new title and "Kind: Standards".
5. Proposed: **READMEs for Inpainter-Team and Inpainter Public.**
6. Blocked on the owner: **media-team**, until they have discussed it.

Also open:
- Proposed: where video creation strategy and artifacts, such as trailers made with InVideo AI, are kept now that `inpainter-media` is gone.
- Proposed: widen the **journeys** skill into a research skill that writes findings guides.
- Proposed: fix the overview's references that do not resolve in a public repository.
