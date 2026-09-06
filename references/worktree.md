# Worktree Playbook — the Isolated Dev Lifecycle

<!-- Opening workflow extracted from obra/superpowers@using-git-worktrees (MIT); closing workflow from obra/superpowers@finishing-a-development-branch (MIT). Platform-specific parts removed. -->

## Part 1 — Opening: creating isolation

**Why worktrees:** work in a separate directory on its own branch while the main checkout stays untouched — the user can keep reading/building there, or an experiment can run without risking the main tree.

### Step 0 — Detect existing isolation first

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
git branch --show-current
git rev-parse --show-superproject-working-tree 2>/dev/null   # submodule guard
```

If `GIT_DIR != GIT_COMMON` and the submodule guard is empty: you are already in a linked worktree — do **not** create another. Report where you are and continue.

### Step 1 — Ask before creating

Creation is an ask-first action. Suggested script:

> "Want me to set up an isolated worktree? It keeps your current checkout untouched while I work. (safe)"

Honor any declared preference (path, naming, always/never) without re-asking.

### Step 2 — Create

If the harness offers a native worktree tool, prefer it — one line, then stop. Otherwise:

```bash
git worktree add "$PATH" -b "$BRANCH"
```

Directory selection, in priority order: user's stated preference → existing `.worktrees/` → existing `worktrees/` → default `.worktrees/` at repo root.

**Verify the directory is ignored before creating anything:**

```bash
git check-ignore -q .worktrees || echo "NOT IGNORED"
```

If not ignored: add it to `.gitignore`, commit that change, then create the worktree. An unignored worktree directory is how whole trees get committed by accident.

### Step 3 — Setup + baseline

Install dependencies per project type (`package.json` → `npm install`, `requirements.txt`/`pyproject.toml` → pip/poetry, `Cargo.toml` → `cargo build`, `go.mod` → `go mod download`), then run the test suite once. Dirty baseline → report failures, let the user decide whether to proceed; a red baseline makes every later failure ambiguous.

## Part 2 — Closing: finishing a development branch

### Verify before offering options

Tests must pass first. Failures → report and ask (proceed / investigate / keep the branch open). Never present finish options for a red branch without saying so.

### The four options (present all; user decides)

1. **Merge** into the base branch — for local-only work with no remote flow.
2. **Open a PR** — default for shared/remote flow; route to `fork-upstream.md` if this is a fork.
3. **Keep** the branch — work in progress; park it and clean up the worktree only.
4. **Discard** — delete the branch and its work; `branch -D` is ask-first (see `danger.md`); suggest tagging the tip first if there's any doubt.

### Cleanup

```bash
git worktree remove "$PATH"   # refuse/skip if dirty without asking
git worktree prune
git branch -d "$BRANCH"       # -d only; -D is ask-first
```

## Quick reference

| Situation | Action |
| --- | --- |
| Already in a linked worktree | Skip creation entirely |
| Inside a submodule | Treat as normal repo (Step 0 guard) |
| Native worktree tool exists | Use it, one line |
| Target dir not ignored | Add ignore rule + commit, then create |
| Dirty baseline tests | Report; user decides |
| Tests red at closing | Say so before offering options |
| Worktree dirty at removal | Ask; never auto-discard |
