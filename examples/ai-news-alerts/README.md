# AI News Alerts Example

This is a sanitized example of how the playbook maps to a real Slack automation project.

The original project, [`ai-news-alerts`](https://github.com/eunhwa99/ai-news-alerts), sends a daily English Slack brief about AI infrastructure, agents, databases, model-serving, and developer tooling trends. This example documents the architecture and workflow shape without exposing private webhooks, state files, or personal configuration.

## Harness Mapping

| Harness principle | AI news implementation |
| --- | --- |
| Explicit source strategy | Hacker News first, official RSS second, GitHub metadata only as enrichment |
| Dry-run safety | `--dry-run` prints the brief without Slack delivery or seen-state writes |
| External side-effect ordering | Non-dry-run sends Slack first, then persists seen state |
| Verification gate | Unit tests run before delivery in GitHub Actions |
| Operational recovery | Persisted seen state is merged back into the branch to reduce duplicate alerts |
| Reviewability | Slack format, source warnings, dedupe, and state behavior are testable contracts |

## Sanitized Workflow Shape

```yaml
name: Daily AI Trend Brief

on:
  schedule:
    - cron: "0 21 * * *"
  workflow_dispatch:
    inputs:
      dry_run:
        description: Run without sending Slack alerts or persisting seen news
        type: choice
        required: true
        default: "true"
        options: ["true", "false"]

permissions:
  contents: write

concurrency:
  group: ai-news-alerts-${{ github.ref }}
  cancel-in-progress: false

jobs:
  daily-brief:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: python -m pip install -e ".[dev]"
      - run: python -m pytest -q -p no:cacheprovider
      - name: Run brief
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
        run: |
          if [ "${{ github.event_name }}" = "workflow_dispatch" ] && [ "${{ inputs.dry_run }}" = "true" ]; then
            ai-news-alerts --dry-run
          else
            ai-news-alerts
          fi
```

## What To Show In A Portfolio

- The app is not just a scraper; it has an operational contract.
- Dry-run behavior is safe by design.
- State persistence happens after Slack succeeds.
- Source strategy is explicit, so noise and outages are explainable.
- Tests protect selection, Slack rendering, seen-state behavior, and collector parsing.
