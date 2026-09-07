# Fork & Upstream Playbook (contributor view)

You are changing someone else's repository. Everything here protects two things: your fork staying current, and the maintainers' review time. (Maintainer-side repo management is `repo-ops.md`.)

## Before forking: three questions

1. **Is a PR even wanted?** Check `CONTRIBUTING.md` and the issue tracker. Many projects want an approved issue before any PR; big features usually need discussion first.
2. **Is it an issue, not a PR?** Bug reports and "have you considered…" often help more than code.
3. **License check** — does the project's license permit your kind of contribution?

## Remote layout (standard convention)

```bash
git clone git@github.com:<you>/<REPO>.git && cd <REPO>
git remote add upstream git@github.com:<original>/<REPO>.git
git fetch upstream
```

- `origin` = your fork — push here. `upstream` = the source repo — fetch only, never push.
- When explaining to the user, this is the `origin vs upstream` card in `concepts.md`.

## Keeping your fork current

```bash
git checkout main
git fetch upstream
git merge --ff-only upstream/main
git push origin main
```

`--ff-only` keeps main linear and refuses to create surprise merge commits. If it fails, main has local commits — surface that to the user rather than force-fixing.

## PR branch workflow

- Branch from a freshly updated `upstream/main`, not from a stale local main.
- One branch = one logical change; name it for the change (`fix/queue-race`), not for you (`patch-1` is GitHub's useless default).
- **Rebasing your own PR branch** onto `upstream/main` is normal and keeps review diffs clean.
- **Never rebase a branch others build on** — if a maintainer pushed commits to your PR branch, rebase is off the table.

## PR etiquette

- One PR = one logical change. Small PRs get reviewed; 40-file PRs rot.
- Description: what changed, why, how it was tested. Use the repo's `PULL_REQUEST_TEMPLATE` if present. Link issues with `Closes #123` / `Refs #456`.
- Get CI green before requesting review; if CI fails, fix or explain — don't leave it red.
- Respond to review comments with new commits on the same branch (reviewers see the incremental diff). Don't force-push mid-review unless the maintainer asks.
- If the PR goes stale or the direction changed: say so and close it politely. Silence is worse.

## Sync check before "it's done"

Before declaring a PR ready: `git fetch upstream` and confirm the branch still merges cleanly; upstream may have moved during your work.
