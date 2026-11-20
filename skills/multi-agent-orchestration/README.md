# multi-agent-orchestration

[![skills.sh](https://skills.sh/b/long6177/skills)](https://skills.sh/long6177/skills)

**Your context is the budget. Spend it on decisions, not on searching.** When a task splits into several substantial workstreams, this skill makes the agent run them as an orchestrator — splitting the work, briefing each subagent properly, dispatching in parallel, checking what comes back, and integrating one result you can defend.

## Why it exists

Subagents are easy to call and easy to waste. A vague brief comes back vague; a child that pastes its raw material into the parent has spent the context it was hired to save; a result accepted on faith quietly becomes a wrong answer. The skill is the discipline that turns delegation from a gesture into a mechanism.

## What it does

- **Split** — cut the task at its seams: one objective, one owner, one deliverable per workstream, with no two writing the same file.
- **Brief** — objective, context the child cannot discover, scope, deliverable shape, and the check that says done. The brief is the child's whole world, so it carries everything.
- **Dispatch** — independent work in one step, dependent work after its input returns, and small lookups kept in the parent where the brief would cost more than the work.
- **Verify** — a result is a claim until checked against the evidence pointer it came with; the claims that decide the outcome get re-checked by the orchestrator.
- **Integrate** — one coherent answer, seams resolved, and any failed or skipped workstream named rather than left as a silent hole.

## The part worth stealing

> **Conclusions with evidence pointers, not raw material.**

Ask for `path:line`, a command, or a URL, and the parent context stays small enough to hold the whole task. Ask for "the full file contents" and you have re-created the problem the subagent was meant to solve.

## Install

```bash
npx skills add long6177/skills --skill multi-agent-orchestration
```

## Boundaries

- Not a workflow engine, not a scheduler, and not a replacement for doing the work: it governs judgment and discipline, and defers to whatever native delegation mechanism the environment provides.
- Delegation is not free — the skill's own test is whether a workstream is substantial enough to pay for its brief, dispatch, verification, and integration.
- It does not guarantee parallelism: when workstreams share a decision or a file, it sequences them.

## License

MIT
