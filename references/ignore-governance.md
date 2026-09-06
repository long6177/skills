# Ignore Governance

Deciding what never enters the repository — by ownership, not by habit.

## The ownership rule

Decide each file's home with the user, using this rule:

1. **Exists only on this machine** (personal notes, scratch scripts, machine-specific local config) → `.git/info/exclude`. It works exactly like `.gitignore` but lives outside the repo, so it is never committed and never affects collaborators.
2. **The user maintains this repo AND the file is junk every collaborator's machine will also produce** (build output, dependencies, IDE files, OS junk) → `.gitignore`, committed. One decision protects everyone; collaborators stop asking "why is this dirty?"
3. Ambiguous → default: repo-type artifacts go to `.gitignore`, personal artifacts go to exclude. When genuinely unsure whether a file is repo-junk or personal, ask the user which kind it is — this is a decision they should own.

Explain the difference the first time it comes up (see `concepts.md`, `.git/info/exclude` card): same syntax, different audience.

## Three categories

### Security — never commit, any circumstance

`.env`, `*.pem`, `*.key`, `credentials*.json`, service-account files, anything with tokens/passwords. Also project-local agent config with secrets.

- Not yet tracked: ignore (rule 1 or 2 by ownership) and move on.
- Already tracked: `git rm --cached <file>` + ignore rule + commit; if it was ever pushed → `danger.md` remediation (history rewrite).

### Collaboration — repo-level junk

Build output (`dist/`, `build/`, `target/`), dependencies (`node_modules/`, `venv/`), logs, coverage, caches, `.idea/`/`.vscode/` (team-dependent — some teams commit shared editor config; ask if unclear).

### Personal — machine-level only

`notes.md`, `scratch/`, local helper scripts, personal tool state. These are the classic exclude candidates: useful to you, noise to everyone else.

## Built-in starter templates

Append the relevant block to `.gitignore` (trim to what the project actually uses; don't paste blindly):

```gitignore
# --- Node ---
node_modules/
dist/
coverage/
*.log

# --- Python ---
__pycache__/
*.pyc
.venv/
venv/
.pytest_cache/
build/
*.egg-info/

# --- IDE / editor ---
.idea/
.vscode/*
!.vscode/settings.json

# --- OS ---
.DS_Store
Thumbs.db
```

For a fuller starter, github/gitignore's language templates are the reference — but the block above covers most new repos.

## Handling already-tracked files

Ignore rules do **not** untrack anything. To stop tracking a file that is already in the repo:

```bash
git rm --cached <file>        # keep it on disk, drop from index
# then add the ignore rule and commit the removal
```

Committing the removal is an act-and-report action; pushing it follows the push rule.

## Boundary

Secrets that are already committed and pushed need history rewriting, not just ignore rules → `danger.md` remediation section.
