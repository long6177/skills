# Danger Playbook

<!-- Blocked-command skeleton from mattpocock/skills@git-guardrails (MIT); safety protocol from github/awesome-copilot@git-commit (MIT). This file educates and scripts confirmations for the loaded session; always-on interception is hook territory (see Hard Block note). -->

Per item: what it destroys → safer alternative → confirmation script (use verbatim, adapted to context).

## The list

**1. `git push --force` / `git push -f`** — replaces remote history; commits others built on vanish.
Safer: `--force-with-lease` (fails if someone pushed meanwhile). Script:
> "This force-push rewrites `<remote/branch>` on the server. Anyone who pulled it will hit conflicts, and commits may be lost. Push anyway? (danger)"

**2. `git reset --hard <ref>`** — discards uncommitted changes and moves the branch; uncommitted work is gone.
Safer: `git stash push` first, or `--soft/--mixed` to keep changes staged/in-tree. Script:
> "This wipes all uncommitted changes in the working tree. Stash them first, or proceed? (danger)"

**3. `git clean -f` / `-fd`** — deletes untracked files permanently (`-x` also kills ignored files).
Safer: `git clean -n` dry run first, review the list. Script:
> "`git clean` permanently deletes untracked files. Dry run first, then delete? (danger)"

**4. `git branch -D <branch>`** — force-deletes a branch with unmerged commits.
Safer: `git branch -d` (refuses unmerged); tag the tip first if unsure. Script:
> "`<branch>` has unmerged commits. Tag its tip as backup, or delete for real? (danger)"

**5. `git checkout .` / `git restore .`** — discards ALL unstaged changes in the tree.
Safer: `git restore -p` (hunk by hunk) or stash. Script:
> "This discards every uncommitted change — there is no undo. Selective restore instead? (danger)"

**6. `git commit --amend` on a pushed commit** — rewrites shared history.
Safer: a new commit. Script:
> "That commit was already pushed. Amending rewrites remote history — new commit instead? (danger)"

**7. Rebase on a shared branch** — replays everyone's commits onto a new base.
Safer: merge; rebase only your own unpushed/PR branches. Script:
> "This rewrites `<branch>`, which others may have based work on. Merge instead, or rebase for real? (danger)"

**8. `--no-verify` (commit or push)** — skips pre-commit/pre-push hooks.
Safer: fix the hook failure. Script:
> "This skips the safety hooks. Only proceed if you've seen the failure and decided it's spurious. (danger)"

**9. Deleting remote branches / tags / releases** — collaborators and tooling may reference them.
Safer: archive/retag; check references first. Script:
> "Deleting `<ref>` on the remote is visible to everyone using this repo. Confirm? (danger)"

## Remediation: secrets already committed

1. **Rotate/revoke the credential first.** History rewriting is secondary — any clone or fork may already hold the secret; rotation is the only real fix.
2. **Assess exposure:** was it pushed? public repo? how long ago?
3. **Rewrite history:** `git filter-repo` (preferred) or BFG to purge the file across all commits.
4. **Force-push the rewritten branches** (ask-first) and **notify collaborators to re-clone**.
5. Accept that public forks/caches may retain it forever — step 1 is the fix; steps 3-5 limit blast radius.

## Hard block note

These checks only run while this skill is loaded. For always-on protection, recommend installing a PreToolUse hook that blocks `push --force`, `reset --hard`, `clean -f`, `branch -D`, `checkout .`, and `restore .` (see mattpocock/skills@git-guardrails for a ready-made one).
