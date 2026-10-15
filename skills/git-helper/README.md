# git-helper

[![skills.sh](https://skills.sh/b/long6177/skills)](https://skills.sh/long6177/skills)

A single-file-plus-references [agent skill](https://agentskills.io) that helps developers with git basics maintain their Git/GitHub repository state while coding with AI agents.

**Design stance:** explain, suggest, let the user decide. The skill runs only when explicitly invoked (passive by design — it never hijacks your coding session), stays in "maintenance mode" for the session, and routes deep knowledge through `references/` on demand.

## Why it exists

Writing code has never been easier, which has quietly made Git and GitHub the infrastructure of AI-assisted development. But many people driving an AI agent only have shallow git knowledge — so when an advanced situation comes up, they just accept whatever the agent recommends, because they can't judge it. git-helper closes that gap: it gives you a repository health check, timely "should you commit / branch / worktree" suggestions, concept cards for the terms you're missing, and a danger playbook for the commands worth slowing down on. It keeps you inside the decision loop instead of watching your repository get managed for you.

## Covers

- **Repo health check** — layered quick check (read-only, ~2s) + one-line status report, ≤3 suggestions with why + risk level
- **Commit rhythm & messages** — when to commit, granularity, smart staging, full Conventional Commits spec
- **Worktree lifecycle** — isolation detection, consent, creation, baseline tests, four-option branch finishing, cleanup
- **Ignore governance** — `.gitignore` vs `.git/info/exclude` by an ownership rule; secrets handling; starter templates
- **Fork & upstream** — remote layout, sync strategy, PR etiquette (contributor view)
- **Repo ops** — issues/labels triage, PR review & merge methods, Actions basics, releases (maintainer view)
- **Danger playbook** — destructive commands with impact, safer alternatives, and confirmation scripts; committed-secrets remediation
- **Concept cards** — 16 git/GitHub cards, answered on request only

## Install

```bash
npx skills add long6177/skills --skill git-helper
```

Only this skill folder is installed into your agent's skills directory. Invoke it explicitly (e.g. `$git-helper`); it never auto-triggers during ordinary coding.

## Structure

```
SKILL.md          flow skeleton: mode, principles, risk tiers, health check, routing
references/*.md   on-demand knowledge, loaded per scene
```

## Credits

Workflows and norms distilled (not delegated) from, all MIT:
[obra/superpowers](https://github.com/obra/superpowers) (using-git-worktrees, finishing-a-development-branch), [github/awesome-copilot](https://github.com/github/awesome-copilot) (git-commit, gh-cli, github-issues, create-github-pull-request), [mattpocock/skills](https://github.com/mattpocock/skills) (git-guardrails blocked list), [juliusbrussee/caveman](https://github.com/juliusbrussee/caveman) (message style principles). Spec text adapted from [conventionalcommits.org](https://www.conventionalcommits.org).

Pairing suggestion: for always-on hard interception of destructive git commands, combine with a PreToolUse hook (e.g. git-guardrails) — this skill's danger layer only runs while loaded.

## License

MIT
