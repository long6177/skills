# Concept Cards

Answer lookup only — hand the user a card when they ask what a term means, then continue the task. Git section first, GitHub section second; each section has 1-2 reserved slots for cards grown from real questions.

## Git

**detached HEAD** — HEAD points at a commit instead of a branch. You can look around and commit, but commits are orphaned when you switch away. Escape: create a branch from here if you want to keep them.

**remote-tracking branch** — `origin/main` is your local bookmark of where the remote's branch was at last fetch. It doesn't update on its own; `git fetch` refreshes it. Divergence between it and your branch is what `branch -vv` reports.

**origin vs upstream** — `origin` is where you push (usually your repo); `upstream` is the source you track (in fork workflows). Fork setups have both; normal repos have only origin.

**fast-forward vs merge commit** — fast-forward moves the branch label when no divergence exists (linear history); a merge commit stitches two diverged lines together and records that a merge happened. Neither is wrong; repos pick a convention.

**rebase vs merge** — rebase replays your commits onto a new base (linear, rewritten); merge preserves history as it happened. Rule of thumb: rebase your own unpushed work, merge or let PRs handle anything shared.

**force push & --force-with-lease** — force push overwrites remote history. `--force-with-lease` refuses if the remote moved since your last fetch — use it always over bare `--force`.

**.git/info/exclude** — a private ignore file, same syntax as `.gitignore`, but not committed. For machine-specific junk; see `ignore-governance.md` for the ownership rule.

**worktree** — multiple checkouts of one repo in separate directories, each on its own branch. Full lifecycle in `worktree.md`.

**reflog** — the local journal of where HEAD/branches have been, kept ~90 days. The "undo" for almost any local accident: find the pre-disaster state and reset/branch to it.

**checkout's many jobs** — one command, three meanings: switch branch (`checkout <branch>`), discard file changes (`checkout -- <file>`, same as `restore`), create-and-switch (`checkout -b`). Modern git splits these into `switch` and `restore`; confusion here is normal.

**(reserved slot — grow from real questions)**

## GitHub

**fork vs clone** — fork is a copy on GitHub's side (yours to push to); clone is a local copy on disk. Contributors usually clone their fork, not the original repo.

**Pull Request lifecycle** — open (or draft) → review (comments/approve/request changes) → CI must pass → merge or close. Draft PRs signal "look, don't merge yet". A PR is a conversation, not a delivery box.

**merge methods** — the three ways a PR lands: merge commit (keeps branch history), squash (one commit per PR — default for messy branches), rebase (linear replay). Pick per repo convention; see `repo-ops.md`.

**GitHub Flow vs Git Flow** — GitHub Flow: branch → PR → merge to `main`, deploy often; the default for web/app projects. Git Flow: long-lived `develop`/`release` branches for versioned, scheduled releases. Choose flow by release rhythm, not fashion.

**Actions: event → job → step** — a workflow YAML in `.github/workflows/` triggers on events (push, PR), runs jobs (parallel VMs), each job runs steps (shell actions). CI failing on your PR usually means one step failed — read `--log-failed`, don't guess.

**Issues + labels + milestones** — issues are the work queue; labels are cheap triage states (`bug`/`feature`/`good first issue`); milestones group issues into a release. An unlabeled backlog is a swamp; see `repo-ops.md` triage flow.

**(reserved slot — grow from real questions)**
