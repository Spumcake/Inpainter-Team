# Project map

Where every repository lives on disk, which GitHub repository it comes from, and how to rebuild the workspace from GitHub alone. Recorded on 2026-10-18. Update it whenever a repository is added, moved, or renamed; the worktrees skill checks it for drift.

## The workspace

The workspace root is `/mnt/Hot/Source`. Every path below is relative to it.

```text
.                                                   spum-pipeline (main)
└── projects/
    └── inpainter-team/                             Inpainter-Team (master)
        └── src/
            ├── inpainter-studio/worktrees/         Inpainter-Studio
            │   ├── .bare/                          bare clone
            │   └── dev/                            branch dev
            └── inpainter-public/                   inpainter-public (main)
                └── src/
                    ├── inpainter.app/worktrees/    inpainter.app
                    │   ├── .bare/
                    │   └── dev/                    branch dev
                    ├── inpainter-docs/worktrees/   Inpainter-Docs
                    │   ├── .bare/
                    │   └── dev/                    branch dev
                    └── media-team/                 media-team (main)
                        └── src/
                            ├── illustrious/worktrees/   illustrious
                            │   ├── .bare/               hooks: .githooks
                            │   └── dev/                 branch dev
                            ├── hosted-llm/worktrees/    hosted-llm
                            │   ├── .bare/
                            │   └── dev/                 branch dev
                            ├── remote-task/worktrees/   remote-task
                            │   ├── .bare/
                            │   └── dev/                 branch dev
                            └── runpod/worktrees/        runpod
                                ├── .bare/
                                └── main/                branch main
```

Each team repository ignores its own `src/`, so a nested repository is never committed into the one around it.

## Repositories

All are under https://github.com/Spumcake/.

- **spum-pipeline**, a private repository, is the root itself: `.` on branch `main`. It is the pipeline every team copies its `.claude/` from. It ignores `projects/`.
- **Inpainter-Team**, a public repository, is `projects/inpainter-team/` on branch `master`. It is the effort's team repository and holds the mission.
- **Inpainter-Studio**, a public repository, is `projects/inpainter-team/src/inpainter-studio/worktrees/`. It is a bare clone, with `dev` checked out. Its branches on GitHub are `dev`, `main`, and `scrap/v0.2-shared-session`, the first 0.2 attempt, kept and never merged.
- **inpainter-public**, a private repository, is `projects/inpainter-team/src/inpainter-public/` on branch `main`. It is the team repository for everything public-facing.
- **inpainter.app**, a private repository, is `projects/inpainter-team/src/inpainter-public/src/inpainter.app/worktrees/`. It is a bare clone, with `dev` checked out. Its only branch on GitHub is `dev`.
- **Inpainter-Docs**, a public repository, is `projects/inpainter-team/src/inpainter-public/src/inpainter-docs/worktrees/`. It is a bare clone, with `dev` checked out. Its branches on GitHub are `dev` and `main`.
- **media-team**, a private repository, is `projects/inpainter-team/src/inpainter-public/src/media-team/` on branch `main`. It is the internal studio's team repository. Its own map, `.project/documents/project-map.md` in media-team, lists every repository in its `src/`, including those not checked out yet.
- **illustrious**, a private repository, is `projects/inpainter-team/src/inpainter-public/src/media-team/src/illustrious/worktrees/`. It is a bare clone, with `dev` checked out, and `core.hooksPath` set to `.githooks`. Its branches on GitHub are `dev` and `main`.
- **runpod**, a private repository, is `projects/inpainter-team/src/inpainter-public/src/media-team/src/runpod/worktrees/`. It is a bare clone, with `main` checked out. Its only branch is `main`.
- **hosted-llm**, a private repository, is `projects/inpainter-team/src/inpainter-public/src/media-team/src/hosted-llm/worktrees/`. It is a bare clone, with `dev` checked out. Its branches on GitHub are `dev` and `main`. It is the terminal client for the Qwen model hosted on a self-deleting RunPod Pod, and reads its RunPod key from runpod's `.env`.
- **remote-task**, a private repository, is `projects/inpainter-team/src/inpainter-public/src/media-team/src/remote-task/worktrees/`. It is a bare clone, with `dev` checked out. It sends a task to any OpenAI-compatible model endpoint named in media-team's endpoint registry and checks the answer's shape.

## Rebuilding

From an empty workspace root, in this order, since each repository is cloned inside the one before it:

```bash
git clone https://github.com/Spumcake/spum-pipeline.git .
git clone https://github.com/Spumcake/Inpainter-Team.git projects/inpainter-team
git clone https://github.com/Spumcake/inpainter-public.git projects/inpainter-team/src/inpainter-public
git clone https://github.com/Spumcake/media-team.git projects/inpainter-team/src/inpainter-public/src/media-team
```

Each bare clone with a worktree is made the same way. Set `W` to its `worktrees/` folder, `URL` to its repository, and `B` to the branch to check out:

```bash
git clone --bare "$URL" "$W/.bare"
git -C "$W/.bare" config remote.origin.fetch '+refs/heads/*:refs/remotes/origin/*'
git -C "$W/.bare" fetch origin
git -C "$W/.bare" worktree add "../$B" "$B"
```

Do this for Inpainter-Studio, inpainter.app, and Inpainter-Docs with `B=dev`, for illustrious, hosted-llm, and remote-task with `B=dev`, and for runpod with `B=main`. Then, for illustrious, `git -C "$W/.bare" config core.hooksPath .githooks`.

When a folder is moved, its checkout loses track of its bare clone. `git -C <worktrees>/.bare worktree repair <path to the checkout>` reconnects it.

## What GitHub does not hold

These stay on this machine by design, and a rebuild has to recreate them by hand:

- `.env` files with API keys: the image-generation skill's, at `.claude/skills/openrouter-image-2-5-sunburst/.env` in the root and in Inpainter-Team, and runpod's, in its `main` checkout. Each repository's `.env.example` or README names the variables.
- media-team's `.local/` (server profiles reachable only from this computer), `.renders/` (render requests and results), and `.chat/`.
- media-team's server snapshot, `.project/projects/illustrious/servers/runpod.snapshot.json`, which is generated.
- Inpainter-Team's image-generation request and response payloads under `.project/renders/models/`.
- The untracked `.project/documents/` and `.project/presentation/` folders in the root, left from the pipeline's earlier history.
- Build output, such as each Rust checkout's `target/`.
- Model weights and ComfyUI storage, at `/mnt/Hot/Services/comfyui`, outside the workspace.
- The local backup of hosted-llm's Qwen model, at `/mnt/Hot/Resources/Models/qwen-3.8-27B-uncensored/`, outside the workspace. Pods download their own copy from Hugging Face.
