# Harness Engineering

## Purpose

This document defines a repeatable engineering harness for agent-assisted software changes.

The harness turns agent activity into an auditable delivery loop: scope discovery, planning, branch preflight, implementation, verification, integration checks, fresh review, and PR handoff.

Use it when a change touches code, workflows, schedules, state persistence, external delivery, or repository instructions.

## Phase And Gate Order

1. Scope discovery: inspect the current project files before proposing changes.
2. Work planning: write acceptance criteria, non-goals, expected files, verification, and rollback point.
3. Branch or worktree preflight: confirm the current branch, base branch, worktree status, and unrelated changes.
4. Implementation: keep changes narrow and aligned with existing project patterns.
5. Test update: cover the changed behavior at the right layer.
6. Focused verification: run the smallest command set that proves the change.
7. Integration check: run a dry-run, preview, smoke check, or workflow check when behavior crosses a system boundary.
8. Fresh review: ask a new reviewer for findings first.
9. Fix every actionable finding, rerun verification, and repeat with a new reviewer until no actionable findings remain.

## Planning Checklist

Capture this before changing implementation files:

- User request and acceptance criteria.
- Scope and non-goals.
- Files or modules expected to change.
- External side effects that must not happen during dry-run.
- State files that must not be mutated by dry-run.
- Verification commands.
- Risks, blockers, and rollback point.

For documentation-only changes, keep the plan lightweight but verify referenced paths, commands, and project facts against current files.

## Implementation Rules

- Prefer existing helper APIs and local patterns over new abstractions.
- Keep schedule timing, source strategy, and production state behavior unchanged unless explicitly requested.
- Treat dry-run behavior as a contract.
- Never print or commit secrets.
- Stage explicit paths only.
- Keep unrelated local changes out of commits and PRs.

## Verification

Choose verification by change type:

- Parser, scoring, or matching changes: unit tests plus relevant fixture coverage.
- Slack formatting changes: formatting tests plus a rendered preview or dry-run.
- Scheduler changes: at least one normal run date and one no-op date.
- State-persistence changes: prove dry-run does not write state and production mode writes only after successful external delivery.
- GitHub Actions changes: workflow syntax review plus manual-dispatch safety check when available.
- Documentation-only changes: path checks, trailing-whitespace checks, and `git diff --check`.

## Failure Classification

Classify before fixing:

- `implementation bug`: production code does not satisfy intended behavior.
- `test bug`: the test setup or expectation is wrong.
- `environment blocker`: a local service, secret, permission, network, or runner dependency is missing.
- `dependency issue`: package install, Python or Java version, or CLI entrypoint mismatch.
- `source outage`: an upstream public source is unavailable, rate-limited, or changed shape.
- `unclear requirement`: safe behavior cannot be inferred.

Fix local bugs directly. For blockers, report the exact command, error, and safest next step.

## Review Gate

Every completed change should pass a fresh review.

- Spawn or assign a new reviewer every time.
- Give the reviewer requirements, changed files, verification output, skipped checks, and relevant review lenses.
- Ask for findings first, ordered by severity.
- Fix every actionable finding.
- Rerun relevant verification.
- Ask a new reviewer for the next pass.
- Stop only when the newest reviewer reports no actionable findings.

If fresh review is unavailable, say so explicitly and perform a self-review only as a documented fallback.
