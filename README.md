# git-helper

[![skills.sh](https://skills.sh/b/long6177/skill-git-helper)](https://skills.sh/long6177/skill-git-helper)

A single-file-plus-references [agent skill](https://agentskills.io) that helps developers with git basics maintain their Git/GitHub repository state while coding with AI agents.

**Design stance:** explain, suggest, let the user decide. The skill runs only when explicitly invoked (passive by design — it never hijacks your coding session), stays in "maintenance mode" for the session, and routes deep knowledge through `references/` on demand.

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
npx skills add long6177/skill-git-helper
```

Only the `skills/git-helper/` folder is installed into your agent's skills directory — this README and repo-level files never enter the skill payload.

## Structure

```
skills/git-helper/
├── SKILL.md          flow skeleton: mode, principles, risk tiers, health check, routing
└── references/*.md   on-demand knowledge, loaded per scene
```

## Credits

Workflows and norms distilled (not delegated) from, all MIT:
[obra/superpowers](https://github.com/obra/superpowers) (using-git-worktrees, finishing-a-development-branch), [github/awesome-copilot](https://github.com/github/awesome-copilot) (git-commit, gh-cli, github-issues, create-github-pull-request), [mattpocock/skills](https://github.com/mattpocock/skills) (git-guardrails blocked list), [juliusbrussee/caveman](https://github.com/juliusbrussee/caveman) (message style principles). Spec text adapted from [conventionalcommits.org](https://www.conventionalcommits.org).

Pairing suggestion: for always-on hard interception of destructive git commands, combine with a PreToolUse hook (e.g. git-guardrails) — this skill's danger layer only runs while loaded.

## License

MIT
