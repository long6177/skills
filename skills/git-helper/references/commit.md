# Commit Playbook

<!-- Staging workflow and safety red lines extracted from github/awesome-copilot@git-commit (MIT); message style principles (why over what, imperative, no noise) distilled from juliusbrussee/caveman@caveman-commit (MIT). Spec text adapted from conventionalcommits.org. -->

## 1. When to commit (hard rules)

A local commit is cheap, reversible, and private — treat it as the save point of your work. Commit:

- **before switching** to a different task or branch
- **before anything experimental or destructive** (put the experiment on its own branch or worktree first — see `worktree.md`)
- **when a logical unit of work is complete** (a feature slice, a fix, a refactor step)

## 2. Granularity (soft decisions)

One logical change per commit. Self-check questions:

- Could this change be reverted alone without breaking unrelated work?
- Would the message need an "and" to describe it? Then split it.
- Formatting mixed with logic? Split: style first, logic second.
- Already committed and found more work? New commit — don't amend history you may have shared.

## 3. Staging

Analyze before staging:

```bash
git status --porcelain   # what changed
git diff                 # unstaged changes
git diff --staged        # what is already staged
```

- Group files into logical commits; avoid one giant commit.
- For partial files: `git add -p` (interactive hunk selection).
- Untracked files piling up? Route to `ignore-governance.md` before staging junk.

**Never stage or commit secrets**: `.env`, `*.pem`, `credentials.*`, anything containing API keys, tokens, or passwords. If found: stop staging, route to `ignore-governance.md`. If such a file is already in history: `danger.md` remediation section.

## 4. Message style (why over what)

- Terse and exact. Explain **why** the change was made, not what the diff shows — the diff already shows what.
- Imperative mood: "add", not "added" or "adds". No trailing period. No first person. No AI attribution footer.
- Subject: ≤50 characters target, 72 hard limit. Body only when the reason isn't obvious; wrap at 72; use `-` for lists.

## 5. Conventional Commits (full spec)

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Types

| Type | Purpose |
| --- | --- |
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no logic change |
| `refactor` | Neither fix nor feat |
| `perf` | Performance improvement |
| `test` | Adding/correcting tests |
| `build` | Build system or dependencies |
| `ci` | CI configuration |
| `chore` | Maintenance, misc |
| `revert` | Reverts a previous commit |

### Breaking changes

Mark with `!` before `:` and/or a `BREAKING CHANGE:` footer describing what changed:

```
feat!: remove deprecated /v1 endpoint

BREAKING CHANGE: the /v1 API is gone; migrate to /v2.
```

### Footers

`token: value` pairs, one per line. Reserved tokens include `BREAKING CHANGE`, `Closes`, `Refs`:

```
fix: prevent race condition in request queue

Closes #231
```

### Worked examples

```
feat(auth): add refresh-token rotation

Sessions no longer outlive 24h; rotated tokens are single-use.
```

```
refactor(queue)!: drop sync dispatch path
```

## 6. Red lines

- Never commit secrets (see §3).
- Never skip hooks with `--no-verify` unless the user explicitly asks.
- Every other safety rule lives in `danger.md` — go there whenever a destructive operation is in play.
