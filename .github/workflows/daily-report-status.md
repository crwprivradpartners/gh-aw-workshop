---
name: Daily Report Status
description: Generates a daily repository activity report as a new issue.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  issues: read
  pull-requests: read
  copilot-requests: write
safe-outputs:
  create-issue:
---

# Daily Report Status

Generate a daily report of repository activity from the last 24 hours and publish it as a new issue.

## Report format

Start with a one-line caption and fenced ASCII art of a cool animal (under 10 lines). Then include:
- **TL;DR:** Up to 3 bullets on overall activity and anything needing attention.
- **Issues:** Opened and closed, with links.
- **Pull requests:** Opened, merged, and awaiting review, with links.
- **Commits:** Notable commits and their authors.
- **Needs attention:** Stale, blocked, or unanswered items.
- **Stats:** A small table counting issues opened/closed, PRs opened/merged, and commits.

Use `###` headings for each section. Omit empty sections or mark them "Nothing to report". Keep bullets to one line, link items by number, and avoid repeating details.
