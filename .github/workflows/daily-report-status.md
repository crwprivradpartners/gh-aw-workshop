---
name: Daily Report Status
description: Generates a daily repository activity report as a new issue.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  issues: read
  copilot-requests: write
safe-outputs:
  create-issue:
---

# Daily Report Status

Generate an activity report in a new issue.
