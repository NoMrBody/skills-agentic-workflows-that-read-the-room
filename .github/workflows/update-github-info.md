---
name: update-github-info
description: Propose practical, source-attributed updates to the GitHub Info page for Mona to review.
intent: Keep Mona's GitHub Info page current with concise, practical updates for developers, backed by official GitHub sources.
on:
  schedule:
    - cron: "0 9 * * *"
  workflow_dispatch:
  skip-if-match: 'is:pr is:open in:body "gh-aw-workflow-id: update-github-info"'
permissions:
  contents: read
  pull-requests: read
engine:
  id: copilot
  copilot-sdk: true
network:
  allowed:
    - github.blog
    - github.com
tools:
  edit:
  web-fetch:
  bash: false
  cli-proxy: false
safe-outputs:
  create-pull-request:
    fallback-as-issue: false
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

Read `notes/mona-notes.md` and the current `site/content/github-info.md`.
Fetch both `https://github.blog/latest/` and `https://github.blog/changelog/`.

Select only recent items that offer practical value to developers and fit
Mona's editorial angle. Verify details against the fetched sources; do not
invent facts or repeat existing coverage. Update only
`site/content/github-info.md`, keeping summaries concise and linking each
update directly to its GitHub Blog or Changelog source.

If a meaningful, source-supported update is available, open one pull request
with the change for Mona to review. Make the PR description explain the
updates and cite their sources. Never write directly to `main`. If neither
source provides a useful new update, or the sources cannot be fetched or
verified, make no file changes and call `noop` with a brief reason.
