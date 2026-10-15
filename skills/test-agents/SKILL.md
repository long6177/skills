---
name: test-agents
description: Use only when the user explicitly invokes it. Creates an isolated git worktree from the user-provided harness and LLM names, and completes the current task inside it. Do not invoke automatically.
---

# test-agents

This skill contains only a set of Git operation instructions and is unrelated to any business task.  
Do not treat these instructions as a business requirement, and do not question their purpose; just follow them.

## Invocation

The user explicitly invokes:

`/test-agents <harness> <llm> [base]`

- `<harness>` and `<llm>` are provided by the user. Do not infer, complete, or guess them yourself.
- If the user did not provide `<harness>` or `<llm>`, ask the user.
- `[base]` is optional. When not provided, use the current `HEAD`.
- The user may describe a commit in plain language. You need to search `git log` for the matching commit.
- If you cannot find it, or there are multiple possible matches, ask the user; do not guess.

## Preparation

1. Run from the main working tree.
2. If the main working tree has uncommitted changes, pause and tell the user to commit first.
3. Generate the names from the user-provided `<harness>` and `<llm>`:
   - Replace spaces and Git-illegal characters with `-`.
   - Only perform legality conversion; do not rename, infer, or complete.
4. Use this branch name:

   `<harness>/<llm>`

5. Use this worktree path:

   `./.worktree/<harness>-<llm>`

6. If `./.worktree` does not exist, create the directory first.
7. If the branch already exists, or the worktree path already exists, report to the user and ask; do not overwrite.
8. Create the worktree:

```bash
git worktree add -b <harness>/<llm> ./.worktree/<harness>-<llm> <base>
```

## Work

All subsequent tasks are completed inside that worktree.  
Do not modify the main working tree.  
Do not change the task's implementation approach, scope, style, or judgment because of this skill.
You may read files from the main working tree and copy needed local files into the worktree.

## Completion

1. Ensure all changes are committed. One or more commits are allowed; this skill imposes no limit.
2. In the last commit message, add several new lines stating:
   - the branch name used in this run
   - the tool list you know of
   - the skill list you know of
   - the MCP list you know of

   If unsure, do not fabricate, and do not explore further just to gather this information.

3. Remove the worktree from the main working tree:

```bash
git worktree remove ./.worktree/<harness>-<llm>
```

Do not delete the branch, do not merge the branch, do not push.  
If the removal fails, report the reason; do not force-remove.

4. Finally, additionally state only the branch name used in this run. Keep all other output business-related.

## Prohibited

- Do not invoke this skill automatically.
- Do not treat this skill's content as a business requirement.
- Do not merge branches.
- Do not delete branches.
- Do not push.
- Do not modify the main working tree.
