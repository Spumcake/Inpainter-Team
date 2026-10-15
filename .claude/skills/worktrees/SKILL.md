---
name: worktrees
description: The workspace layout (a team repository for the pipeline and project documents, with each app repository's branches in worktree folders under src/) and how worktrees are set up, used for parallel work, checked before a commit, and cleaned up. Read before committing in an app repository, before running work in parallel, or when setting up a repository.
---

# The team repository and its app worktrees

```text
<team>/                                 team repository (open the session here)
  CLAUDE.md, AGENTS.md                  working instructions
  .claude/                              this pipeline
  INTERFACE.md
  .project/documents/concept/           MISSION.md
  .project/documents/development/<v>/   the version in progress
  .project/documents/history/           diary, closed versions
  .gitignore                            includes /src/
  src/<repo>/worktrees/                 one app repository, ignored by the team repository
    .bare/                              bare clone
    dev/                                the permanent checkout, on branch dev
    work-<name>/                        temporary checkout, on branch work/<name>
```

- **The team repository** versions everything except the apps. **Each app repository** holds only its app: source, tests, README, workflows, and its own AGENTS.md and developer skills, which belong to the app. Never put pipeline files or project documents in an app repository. AGENTS.md records every repository's URL.
- An existing project that keeps its app inline stays that way until the owner asks to migrate.
- **`dev/`** is the only permanent checkout. Work happens and is integrated there.
- **`work-<name>/`** folders exist only for parallel work: one per independent assignment, on branch `work/<name>` from the current `dev` commit. Use them only when assignments are truly independent; a single line of work stays in `dev/`.
- **`main`** is not checked out. It changes only when the owner merges a pull request on GitHub.
- **The owner** pushes, tags, and merges on GitHub. Commit and push only when they ask.

## Parallel work

Create each worktree from the bare clone:

```bash
git -C src/<repo>/worktrees/.bare worktree add -b work/<name> ../work-<name> dev
```

Each subagent works only in its own worktree, and each file has one writer. Bring the results into `dev`, by merging committed branches or by copying uncommitted work, and resolve conflicts yourself, reading both sides; never take one side wholesale.

## Branch check

Before every commit in an app repository, and at the start of a slice when a `work-*` worktree exists, settle each `work-*` worktree. In the bare clone, run `git worktree list` and `git worktree prune`, then classify each one:

1. **Integrated:** its branch has no commits `dev` lacks (`git rev-list --count dev..work/<name>` is 0) and every file it changed or added is already in `dev`. Remove it with `git worktree remove --force` and its branch with `git branch -d`. Force is allowed only here, because nothing in it would be lost.
2. **Merged:** its branch has commits `dev` lacks and the slice being committed includes them. Merge it into `dev` with `--no-ff`, then remove the worktree and branch.
3. **Reported:** anything else. Touch nothing, tell the owner what is not in `dev`, and remove it only with their approval.

Never force anything else, reset `--hard`, rewrite history, or delete `main`, `dev`, or an unmerged branch without the owner's approval.

## Setting up a repository

When the owner asks to add an app repository that has no worktrees yet:

1. Make sure the team repository's `.gitignore` contains `/src/`.
2. Clone it:

```bash
git clone --bare <url> src/<repo>/worktrees/.bare
git -C src/<repo>/worktrees/.bare config remote.origin.fetch '+refs/heads/*:refs/remotes/origin/*'
git -C src/<repo>/worktrees/.bare fetch origin
git -C src/<repo>/worktrees/.bare worktree add ../dev dev
```

When `dev` does not exist, create it from `main`: `git -C src/<repo>/worktrees/.bare worktree add -b dev ../dev origin/main`. When the repository has a `.githooks/` folder, enable it with `git -C src/<repo>/worktrees/.bare config core.hooksPath .githooks`, and never bypass a hook that rejects a commit.

3. For a new app with no URL yet, initialise the bare repository with `main` as its default branch, make the first commit through a temporary worktree, add `dev`, and tell the owner the repository they need to create on GitHub.
