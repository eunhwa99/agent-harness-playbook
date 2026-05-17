# GitHub Workflow

## Branch Policy

- Do not commit directly to a primary branch.
- Before edits, check branch, base branch, remotes, and worktree status.
- If the primary branch is checked out, create a focused feature branch or isolated worktree.
- If the worktree is dirty, protect unrelated user or agent changes.
- Never use destructive git commands unless explicitly requested.

Useful checks:

```bash
git status --short --branch
git branch --show-current
git remote -v
```

## Commit Policy

- Commit only intentional files.
- Stage explicit paths.
- Avoid seen-state files, caches, virtualenvs, local scratch files, and generated output unless they are the intended artifact.
- Use small Conventional Commit messages.
- Run verification before commit.

Example:

```bash
git add README.md docs/harness-engineering.md
git diff --cached --check
git commit -m "docs: add harness playbook"
```

## PR Policy

- Confirm the correct remote and base branch.
- Keep one PR focused on one delivery unit.
- Split independent tasks into separate PRs.
- Use stacked PRs only when there is a real ordering dependency.
- Include verification commands and intentionally skipped checks in the PR body.

## CI And Review Feedback

Classify failures before changing code:

- Branch-related: tests, formatting, lint, workflow syntax, state mutation, or behavior tied to the diff.
- Flaky or unrelated: runner outage, network outage, registry outage, external source outage.
- Ambiguous: logs do not prove either category.

Fix branch-related failures. Do not hide flaky or unrelated failures by weakening tests or changing source strategy.
