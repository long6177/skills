# skills

[![skills.sh](https://skills.sh/b/long6177/skills)](https://skills.sh/long6177/skills)

A collection of first-party [agent skills](https://agentskills.io) by [@long6177](https://github.com/long6177), published to [skills.sh](https://skills.sh). Each skill is self-contained under `skills/<name>/` — install any of them on their own, or all together.

## Skills

| Skill | What it does |
| --- | --- |
| [git-helper](skills/git-helper/) | Keeps your Git/GitHub repository state healthy while an AI agent writes code — health check, commit rhythm, worktrees, ignore governance, and a danger playbook. Explain, suggest, let you decide. |
| [test-agents](skills/test-agents/) | Gives different `harness`/LLM combinations the same start — an isolated git worktree off one commit, the same task, a kept branch. Don't benchmark; just compare. Invoke as `/test-agents`. |

## Install

```bash
# install every skill in this repo
npx skills add long6177/skills

# or pick a single skill
npx skills add long6177/skills --skill git-helper
npx skills add long6177/skills --skill test-agents
```

Only the selected `skills/<name>/` folder is installed into your agent's skills directory — this README and other repo-level files never enter the skill payload. Every skill keeps its own showcase page at `skills/<name>/README.md`.

## Structure

```
skills/<name>/
├── SKILL.md          entry point: mode, principles, risk tiers, health check, routing
├── README.md         this skill's own showcase page
└── references/*.md   optional on-demand knowledge, loaded per scene
```

## License

MIT
