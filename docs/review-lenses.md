# Review Lenses

Use these lenses when reviewing an agent-delivered change. Select only the lenses that match the changed behavior.

## Slack Or Notification Contract

- Message format is readable and action-oriented.
- Links use the target platform's expected syntax.
- Labels are escaped or sanitized.
- Empty results render intentionally.
- External delivery errors do not leak secrets.

## Scheduler And State

- Dry-run or preview mode does not send messages.
- Dry-run or preview mode does not mutate seen, tracker, cache, or export state.
- Production mode persists state only after successful external delivery.
- No-op dates exit successfully without posting unexpectedly.
- Dedupe avoids duplicate alerts without hiding new items.

## Source And Collector

- Source strategy is explicit and documented.
- Public source failures become visible warnings where possible.
- Authenticated or fragile scraping is not added accidentally.
- Date, role, keyword, or score filters do not silently hide expected results.
- Fixture coverage reflects the source shape being parsed.

## GitHub Actions

- Manual dispatch is harmless by default.
- Secrets are passed through environment variables and never printed.
- Tests run before delivery.
- Persisted state is committed only for production paths.
- Concurrent runs do not overwrite each other's state.

## Security And Secrets

- No webhook URL, token, OAuth credential, `.env` value, or private user data is committed.
- Error messages name the missing secret without echoing its value.
- Logs are useful enough to debug without exposing sensitive data.

## Test Quality

- Tests cover the changed layer.
- Slack formatting changes include escaping and link rendering.
- Collector changes cover dedupe and malformed input.
- State changes prove dry-run safety and post-success persistence.

## Change Size And Staging

- Unrelated files are not staged.
- Scratch files, caches, virtualenvs, and generated state are not included.
- Large unrelated tasks are split into separate delivery units.

## Reviewer Output

Reviewers should report findings first. Every actionable finding should include:

- Severity.
- File and line reference.
- Why it matters.
- Concrete fix recommendation.

If there are no actionable findings, say that clearly.
