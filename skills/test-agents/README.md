# test-agents

[![skills.sh](https://skills.sh/b/long6177/skills)](https://skills.sh/long6177/skills)

**Don't benchmark. Just compare.** In 2026 an agent is `LLM + harness`, and the combinations are endless — so the differences blur. `test-agents` scores nothing. It only gives different combinations the same starting line: an isolated git worktree off one base commit, the same task, a kept branch, and your own judgment at the end.

## Why it exists

> Models keep shipping, harnesses keep shipping, combinations keep multiplying.
> You switch every day, yet you can no longer say what actually changed:
> is it the model, or the shell? Is it dumber now, or does it just feel that way?
> Running it casually tells you nothing — different start points, different environments, and they pollute each other.
> A real benchmark is too heavy; casual impressions are too loose.

`test-agents` exists to fix that one fuzzy sentence: *"I feel this one is a bit better."*

## What you can do with it

Invoke it explicitly and it does one thing — set up a fair, isolated run:

```
/test-agents <harness> <llm> [base]
```

- **Same start** — every run branches from the same base commit. `base` is optional (default `HEAD`); describe a commit in plain language and the agent finds it in `git log`.
- **No interference** — each combination gets its own `git worktree` at `./.worktree/<harness>-<llm>`; the main working tree is never modified.
- **Parallel comparison** — run two or more combinations at once; no queueing.
- **Low intrusion** — the skill carries only Git instructions. It does not change your task's approach, scope, style, or judgment.
- **Traceable** — the worktree is removed at the end, but the branch is kept: cut to it and try the result yourself. It never merges, pushes, or deletes the branch.
- **Recorded** — the last commit message records the branch name plus the tool / skill / MCP lists the agent knew of.
- **You judge** — no auto-scoring, no auto-picking. Whether the result is good is your call.

## Use cases

- **Same harness, different LLMs** — `/test-agents claude-code opus-5.5` vs `/test-agents claude-code gpt-5.6-sol`
- **Same LLM, different harnesses** — `/test-agents claude-code opus-5.5` vs `/test-agents codex opus-5.5`
- **Everyday tasks, side by side** — fix a bug, build a small feature, refactor a module, write docs, review code. Real tasks, no mock benchmark set.

## Boundaries

Not a benchmark. Not an auto-eval framework. Not an agent scheduler. Not a "best-combo recommender". It is a Git scaffold for personal A/B experiments.

## Install

```bash
npx skills add long6177/skills --skill test-agents
```

Invoke it explicitly as `/test-agents`; it never auto-triggers.

## FAQ

- **Does it compare agents automatically?** No — it isolates and keeps branches; the judging is yours.
- **Does it interfere with the task itself?** No — only Git instructions; your task's approach, scope, style, and judgment stay untouched.
- **Can I run several combinations at once?** Yes — each gets an independent worktree.
- **Where is the result?** On the branch; cut to it and try it.
- **Is it a serious benchmark tool?** No — it is a Git scaffold for personal comparison, not a replacement for benchmarks.

## License

MIT
