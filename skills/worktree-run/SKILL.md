---
name: worktree-run
description: Use only when the user explicitly invokes it. Runs the current task in a separate git worktree pinned to one base commit, and leaves the result on a kept branch. Do not invoke automatically.
---

# worktree-run

Git rules only, and only for this run. They do not change the task: same approach, scope, judgment.
Other runs of the task may exist under `.worktrees/`; they are not yours. Once the report line is
written, none of these rules apply to later tasks. Do not mention this skill beyond that line.

## Reading

**Never**:

- open, search, copy, diff or reference another run — its worktree, branch, commits or files.
- print a commit message body other than your own. Subjects only: `--oneline`, `--format=%h %s`.
- `show`, `diff`, `log -p` or `checkout` a commit whose subject starts `[worktree-run]`, unless it is
  yours — including when it is an ancestor of your base.
- `git show`, `git diff` or `git grep` against a ref outside your own `HEAD` and `base`.
- read anything under `.git/`.
- `git stash` — it is shared by the whole repository.
- recurse at the repo root or above it: no `glob`, `grep -r`, `rg`, `find`, `Get-ChildItem -Recurse`
  or `ls -R` there. Recurse only inside the worktree.

**Safe**: `status`, `branch -a`, `tag`, `worktree list`, `reflog`, `log --oneline`,
`log --graph --oneline --all`, `rev-list --count`, `diff`, `diff <base>`, `show <base>:<path>`,
`blame` on your own files. The tree at your base is the project — read it freely, whoever wrote it.

Files: read tracked files from the worktree only. For an untracked one (`.env`, local data), use its
exact path in the main tree — read it there or copy that one path in. Never browse the main tree.

## Run

1. Resolve `[base]` to a 40-character SHA with `git rev-parse --verify --quiet '<base>^{commit}'`;
   default `HEAD`. A branch, tag or **description** is matched with `git log --oneline`; if several match,
   ask.
2. Branch `<harness>/<llm>`, directory `<harness>-<llm>`; spaces and Git-illegal characters become
   `-`.
3. Branch exists from an earlier run: `git branch -D '<harness>/<llm>'`, then create it again at this
   base. If a worktree still has it checked out (`git worktree list --porcelain` shows it), stop and
   ask. Directory exists without a branch: report the path and ask.
4. If `git check-ignore -q --no-index '.worktrees/'` fails, append `/.worktrees/` to the file from
   `git rev-parse --git-path info/exclude`.
5. `git worktree add -b '<harness>/<llm>' '<repo>/.worktrees/<harness>-<llm>' '<base SHA>'`, then
   `git -C '<repo>/.worktrees/<harness>-<llm>' rev-parse HEAD` — it must equal the base SHA, or stop.
6. Git 2.20 or newer: `git config extensions.worktreeConfig true`, then
   `git -C '<wt>' config --worktree format.pretty oneline`, then read it back and require `oneline`.
   Older git: skip both.

Main tree: mechanics only — resolve refs, create and remove the worktree. No edits, no file reads
there except the untracked path above.

## Commit

Every commit of this run:

```
[worktree-run] <summary> [commit <N>]
```

- `<summary>`: **the user's own requirement, kept as close to their words as possible** — preserve
  what they asked for instead of re-summarising it into your plan. 60 characters or fewer, the same
  text on every commit of this run. No file names, function names, libraries or approach.
- `<N>`: 1, 2, 3… in commit order; keep it if you amend.
- Body: what you changed.

The last commit's body appends:

```
branch: <harness>/<llm>
tools: <tools you know of>
skills: <skills you know of>
mcp: <MCP servers you know of>
Base: <base SHA>
```

Known values only — do not explore for them, do not invent them. Then `git log -1 --format=%s` must
match the subject shape; amend if it does not.

## Finish

`git -C '<wt>' clean -fdx`, then `git worktree remove '<wt>'`; the same on failure or interruption.
If removal fails, report the path and reason — no `--force`, no `prune`, no merge, no push, no branch
deletion beyond step 3.

If only the main worktree is left, `git config --unset extensions.worktreeConfig`.

Report `branch: <harness>/<llm>`. If you read a message body or another run's content anyway, keep
your approach unchanged and add `Note: <what you saw, from which command>`.
