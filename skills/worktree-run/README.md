# worktree-run

[![skills.sh](https://skills.sh/b/long6177/skills)](https://skills.sh/long6177/skills)

**Don't benchmark. Just compare.** One command gives every `LLM + harness` combination the same start — its own worktree, one base commit, the same task — and leaves a branch you judge yourself.

## Why

> Models ship weekly, harnesses ship weekly, combinations multiply. You switch all day and still can't say what actually changed — the model, or the shell?

A real benchmark is too heavy; a casual try is too loose. And a repository remembers: an earlier run's branch and commit message are one `git log` away, so a fresh attempt can quietly copy an old answer.

`worktree-run` makes the runs comparable, and keeps them from reading each other.

## What you get

```
/worktree-run <harness> <llm> [base]
```

- **Same start** — every run branches from one pinned SHA; `base` defaults to `HEAD`.
- **Own checkout** — `<repo>/.worktrees/<harness>-<llm>`; run several at once, your main tree untouched.
- **Recorded** — the last commit carries `branch`, `tools`, `skills`, `mcp`, `Base:`.
- **Yours to judge** — nothing scores, merges or pushes. The branch is the result.

## The part worth stealing

```
[worktree-run] add a dark theme toggle to settings [commit 2]
```

The subject of every commit is **your request**, never the change — the change lives in the body. A run that lists branches therefore learns nothing it didn't already know, and the repeated subject gives you one filter: `git log --all --grep='\[worktree-run\]'` lists every run you have ever made.

The rule is backed by git rather than trust: each worktree sets `format.pretty=oneline` locally, so `git log` and `git show` print subjects and never bodies, even when the agent forgets what it was told.

## Use cases

- **Same harness, different models** — `claude-code opus-5.5` vs `claude-code gpt-5.6-sol`
- **Same model, different harnesses** — `claude-code opus-5.5` vs `codex opus-5.5`
- **Everyday tasks, side by side** — a bug, a small feature, a refactor, a review. Real work, no mock set.

## Install

```bash
npx skills add long6177/skills --skill worktree-run
```

Invoke it explicitly as `/worktree-run`; it never auto-triggers.

## Boundaries

Not a benchmark, not an eval framework, not a scheduler — a git scaffold for personal A/B runs. It isolates the working directory and `HEAD`, not the repository; if you need provable independence, clone. The rules last for one run: afterwards the worktree and its config are gone, and only the branch remains.

## FAQ

- **Several combinations at once?** Yes — each has its own worktree and its own git config.
- **Same combination again?** Its old branch is replaced at the new base; a round ends when you merge the winner.
- **Does it find the result for me?** No. The diff is the record, the judging is yours.

## License

MIT
