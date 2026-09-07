# Repo Ops Playbook (maintainer view)

You are managing **your own** repository. Goal: a repo a stranger can evaluate in one minute, and a queue (issues + PRs) that reflects reality. (Contributing to other people's repos is `fork-upstream.md`.)

## 1. Repo health checklist

Present findings as a table (item / status ✅⚠️ / note), fold into the ≤3-suggestions output of the health check:

| Item | What good looks like |
| --- | --- |
| README | Install + usage current; screenshot or demo for UI projects |
| LICENSE | Present and matches intent (MIT/Apache-2.0 for openness) |
| `.github/workflows/` | CI exists and recent runs are green |
| Issue/PR templates | Present so contributors file usable reports |
| Branch protection | `main` requires PR + CI; no direct pushes |
| Description/topics | Filled in on GitHub so the repo is findable |

## 2. Issue management

`gh` CLI basics (read-only ones are act-and-report):

```bash
gh issue list --state open
gh issue view 123
gh issue create --title "..." --body "..." --label bug
gh issue close 123 --comment "fixed by #124"
```

**Triage flow:** reproduce (or ask for missing details) → label (`bug` / `feature` / `question` / `good first issue`) → prioritize (milestone or just pin) → assign or leave for contribution. An unlabeled backlog is a swamp; labels are the cheapest triage tool.

<!-- gh CLI usage and triage flow extracted from github/awesome-copilot@gh-cli and @github-issues (MIT). -->

## 3. PR review & merge method

Review flow: read the description first → then the diff → then CI status → comment/approve/request changes.

**Choosing a merge method** (decide with the user; explain trade-offs):

| Method | History looks like | Choose when |
| --- | --- | --- |
| Merge commit | Branch structure preserved, extra merge node | Branch story matters (long feature) |
| Squash | One clean commit per PR | PR branches are messy (default for most teams) |
| Rebase | Linear, each PR commit replayed | Commits were crafted carefully |

Follow repo convention if one exists; otherwise recommend squash for agent-heavy work — AI sessions generate many WIP commits.

## 4. Actions basics (read and operate, don't author)

A workflow is YAML in `.github/workflows/`: **event** (`on: push`, `pull_request`) → **jobs** → **steps**. Concepts card in `concepts.md`.

```bash
gh run list --limit 5
gh run view <id> --log-failed   # only the failing steps
gh run rerun <id>               # retry after fixing or for flaky
```

**Flaky detection:** a run that passes on rerun without any change is flaky — report it as a real bug, don't train the habit of "just rerun".

## 5. Releases & tags

```bash
git tag -a v1.4.0 -m "Release 1.4.0"
git push origin v1.4.0          # or: git push --follow-tags
gh release create v1.4.0 --generate-notes
```

Versioning reminder: breaking → major, feature → minor, fix → patch. Deleting a published tag or release is ask-first (`danger.md` tier) — anyone may reference it.
