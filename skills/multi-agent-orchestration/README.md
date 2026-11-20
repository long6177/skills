# multi-agent-orchestration

[![skills.sh](https://skills.sh/b/long6177/skills)](https://skills.sh/long6177/skills)

**Don't do everything in one context. Orchestrate.** When a task splits into several substantial workstreams, this skill has the agent act as the central orchestrator: delegate the independent pieces to native subagents, stay accountable for the whole, and synthesize one coherent result.

## Why it exists

A single context degrades as a task grows: the plan, the evidence, and the output all compete for the same window. Subagents solve that — but only if someone stays in charge of the objective instead of spraying tasks at children and hoping the pieces fit. This skill is that discipline.

## What it does

1. **Delegate proactively** — split off independent or loosely coupled workstreams whenever delegation improves focus, parallelism, or context management.
2. **Stay in control** — remain responsible for the objective, priorities, coordination, important decisions, and the final outcome.
3. **Assign focused tasks** — each subagent gets a clear objective, relevant context, constraints, and an expected deliverable; no vague or overlapping assignments.
4. **Coordinate intelligently** — independent work in parallel, dependent work sequenced, no conflicting changes to shared resources.
5. **Synthesize results** — review outputs, verify important findings, resolve disagreements, integrate one coherent outcome.
6. **Use good judgment** — no subagents for trivial tasks, and none when coordination costs more than it saves.

## The stance worth stealing

> **Use the native subagent capabilities of the current environment. Do not assume or prescribe any particular tool, command, or delegation API.**

The skill governs judgment, not plumbing — it works with whatever delegation mechanism your harness ships, and stays valid when that mechanism changes.

## Install

```bash
npx skills add long6177/skills --skill multi-agent-orchestration
```

## Boundaries

- Not a workflow engine or a scheduler: it changes how the agent decides and coordinates, not what tools exist.
- Delegation is not free — the skill's own sixth point is the brake on spawning subagents that cost more than they return.
- It does not promise parallelism: workstreams that share a decision or a resource are sequenced.

## License

MIT
