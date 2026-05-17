# Agent Harness Playbook

A public, sanitized playbook for building agent-based engineering harnesses around real software delivery.

This repository documents the repeatable workflow I use to turn AI-agent work into controlled engineering delivery: scoped planning, branch hygiene, dry-run safety, verification gates, fresh review loops, and clean PR handoff.

The private source workflow was applied to production-code changes in a Spring Boot backend repository and to operational Slack automation systems. This public version removes private project details, credentials, schedules, and state files while keeping the reusable engineering patterns.

## What This Shows

- How to structure an agent workflow before code changes start.
- How to keep implementation, tests, verification, review, and PR handoff traceable.
- How to design dry-runs that do not mutate production state.
- How to persist state only after the external side effect succeeds.
- How to use GitHub Actions as a safe operational runner.
- How to use fresh reviewer loops to catch regressions before handoff.

## Reference Implementation

The first project built with this harness style was [`ai-news-alerts`](https://github.com/eunhwa99/ai-news-alerts), a daily Slack brief for AI infrastructure, agents, databases, model-serving, and developer tooling trends.

It was built with two public pieces:

- [`agent-harness-playbook`](https://github.com/eunhwa99/agent-harness-playbook): the delivery workflow and review gates.
- [`codex-config`](https://github.com/eunhwa99/codex-config): the local Codex skills, hooks, safety checks, and sync automation used to run the workflow.

The project uses the same harness principles documented here:

- Hacker News and official RSS sources are treated as explicit source strategy.
- Dry-runs print the brief without sending Slack or mutating `seen` state.
- Non-dry-runs send Slack first, then persist `seen` state.
- GitHub Actions runs tests before delivery.
- Persisted state is merged back into the branch to reduce duplicate alerts.
- Slack output is designed and tested as an operational contract.

See [`examples/ai-news-alerts/`](examples/ai-news-alerts/) for the sanitized architecture and workflow example.

## Playbook Contents

- [`docs/harness-engineering.md`](docs/harness-engineering.md): phase and gate workflow.
- [`docs/review-lenses.md`](docs/review-lenses.md): reviewer checklist for agent-delivered changes.
- [`docs/github-workflow.md`](docs/github-workflow.md): branch, commit, PR, and CI handling.
- [`examples/ai-news-alerts/`](examples/ai-news-alerts/): sanitized example showing how the playbook maps to a real Slack automation app.

## Core Loop

```text
scope discovery
-> plan and acceptance criteria
-> branch/worktree preflight
-> implementation and tests
-> focused verification
-> integration or dry-run check
-> fresh reviewer
-> fix findings and repeat
-> commit / push / PR handoff
```

## Safety Defaults

- Do not expose secrets, webhooks, tokens, or private user data.
- Do not let dry-runs mutate state or send messages.
- Do not persist state before the external delivery succeeds.
- Do not mix unrelated local changes into commits.
- Do not reuse the same reviewer after a finding is fixed.
- Do not treat generated caches, local scratch files, or seen-state files as source changes.

## When To Use This

This playbook is useful for small operational automation projects, backend delivery workflows, and agent-assisted PR work where reliability matters more than raw speed.

It is intentionally lightweight: the goal is not to build a large framework, but to make agent-assisted delivery repeatable, reviewable, and safe.
