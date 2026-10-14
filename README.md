# skills

[![skills.sh](https://skills.sh/b/long6177/skills)](https://skills.sh/long6177/skills)

A collection of first-party [agent skills](https://agentskills.io) by [@long6177](https://github.com/long6177), published to [skills.sh](https://skills.sh). Each skill is self-contained under `skills/<name>/`, so any of them can be installed on its own or together.

## Skills

| Skill | What it does |
| --- | --- |
| [git-helper](skills/git-helper/) | Helps developers keep their Git/GitHub repository state healthy while coding with AI agents — explain, suggest, let the user decide. |

## Install

```bash
# install every skill in this repo
npx skills add long6177/skills

# or pick a single skill
npx skills add long6177/skills --skill git-helper
```

Only the selected `skills/<name>/` folder is installed into your agent's skills directory — this README and other repo-level files never enter the skill payload.

## Structure

```
skills/<name>/
├── SKILL.md          entry point: mode, principles, risk tiers, health check, routing
└── references/*.md   on-demand knowledge, loaded per scene
```

## git-helper

A single-file-plus-references skill that helps developers with git basics maintain their Git/GitHub repository state while coding with AI agents.

**Design stance:** explain, suggest, let the user decide. The skill runs only when explicitly invoked (passive by design — it never hijacks your coding session), stays in "maintenance mode" for the session, and routes deep knowledge through `references/` on demand.

Covers:

- **Repo health check** — layered quick check (read-only, ~2s) + one-line status report, ≤3 suggestions with why + risk level
- **Commit rhythm & messages** — when to commit, granularity, smart staging, full Conventional Commits spec
- **Worktree lifecycle** — isolation detection, consent, creation, baseline tests, four-option branch finishing, cleanup
- **Ignore governance** — `.gitignore` vs `.git/info/exclude` by an ownership rule; secrets handling; starter templates
- **Fork & upstream** — remote layout, sync strategy, PR etiquette (contributor view)
- **Repo ops** — issues/labels triage, PR review & merge methods, Actions basics, releases (maintainer view)
- **Danger playbook** — destructive commands with impact, safer alternatives, and confirmation scripts; committed-secrets remediation
- **Concept cards** — 16 git/GitHub cards, answered on request only

## Credits

Workflows and norms distilled (not delegated) from, all MIT:
[obra/superpowers](https://github.com/obra/superpowers) (using-git-worktrees, finishing-a-development-branch), [github/awesome-copilot](https://github.com/github/awesome-copilot) (git-commit, gh-cli, github-issues, create-github-pull-request), [mattpocock/skills](https://github.com/mattpocock/skills) (git-guardrails blocked list), [juliusbrussee/caveman](https://github.com/juliusbrussee/caveman) (message style principles). Spec text adapted from [conventionalcommits.org](https://www.conventionalcommits.org).

Pairing suggestion: for always-on hard interception of destructive git commands, combine with a PreToolUse hook (e.g. git-guardrails) — this skill's danger layer only runs while loaded.

## License

MIT
