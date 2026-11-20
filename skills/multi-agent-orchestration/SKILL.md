---
name: multi-agent-orchestration
description: Use when a task splits into multiple substantial or independent workstreams — parallel research, a wide file sweep, work that would exhaust one context, or several deliverables — and native subagents should carry them. Covers splitting, briefing, dispatching, verifying, and integrating.
license: MIT
---

# Multi-Agent Orchestration

You are the orchestrator. Your context is the scarce resource: it holds the objective, the decisions you made, and the evidence you verified. Subagents buy more of it, at the price of coordination.

## Split

Find the seams before you delegate. A workstream is one objective, one owner, one deliverable.

- Independent or loosely coupled work → separate workstreams.
- Work coupled by a shared decision → settle the decision first, then split.
- Two workstreams that would edit the same file → one workstream, or sequence them.

**Done when** every workstream has a single owner, a deliverable, and no overlap in the files it writes.

## Brief

A subagent starts with none of your conversation. Its brief is the whole world it gets, and a vague brief returns vague work.

Every brief carries:

- **Objective** — one sentence, in the imperative.
- **Context** — what the child cannot discover by looking: prior decisions, constraints, exact paths it needs, what you already ruled out.
- **Scope** — what to touch, what to leave alone.
- **Deliverable** — the shape of the answer: a conclusion, a file, a diff, or findings with evidence.
- **Done** — the check the child applies before it returns.

Ask for **conclusions with evidence pointers** — file paths, line numbers, commands, URLs — rather than raw material. A child that pastes five hundred lines back into your context has spent what it was hired to save.

**Done when** a subagent with no access to this conversation could execute the brief and know when it is finished.

## Dispatch

- Independent workstreams → dispatch together, in one step.
- A workstream that consumes another's output → dispatch after that one returns.
- Work that is really a lookup or a single edit → do it yourself; the brief costs more than the work.

**Done when** every dispatched workstream is running or returned, and nothing is dispatched twice.

## Verify

A subagent's result is a claim, not a fact.

- Check each load-bearing claim against the evidence pointer it supplied.
- Re-run the check yourself when the claim decides the outcome.
- Treat conflicting results as a signal to investigate, not to average.

**Done when** every claim the final answer depends on has been checked against its evidence.

## Integrate

You own the outcome. Read the final diff, resolve the seams between workstreams, and state what changed in one coherent answer. When a workstream failed or was skipped, say so and cover the gap — a silent hole must not ship as a finished result.

**Done when** the delivered result is one answer you can defend end to end, with no workstream unaccounted for.

## Judgment

Coordination has a price: a brief, a dispatch, a verification, an integration. Delegate when a workstream is substantial enough that its context and time outweigh that price, and keep the small, tightly coupled, or decision-heavy work in your own hands.

Use the native subagent capabilities of the current environment. The mechanics differ between harnesses; this skill governs what you do with them, not which tool you call.
