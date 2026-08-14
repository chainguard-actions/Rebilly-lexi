<!-- markdownlint-disable -->

# Hardening Report: Rebilly--lexi/v2.3.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Rebilly--lexi/v2.3.5** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files use mutable tag-based refs instead of pinned 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the referenced action tags are moved or compromised.

Failing references in deploy-playground.yml:
- uses: actions/checkout@v6
- uses: actions/setup-node@v6
- uses: actions/cache@v5
- uses: actions/configure-pages@v5
- uses: actions/upload-pages-artifact@v4
- uses: actions/deploy-pages@v4

Failing references in pr-checks.yml:
- uses: actions/checkout@v6
- uses: actions/setup-node@v6
- uses: actions/cache@v5

Failing references in update-tags-post-release.yml:
- uses: actions/checkout@v6
- uses: haya14busa/action-update-semver@v1

Locations:

- `.github/workflows/deploy-playground.yml:24`
- `.github/workflows/deploy-playground.yml:26`
- `.github/workflows/deploy-playground.yml:30`
- `.github/workflows/deploy-playground.yml:38`
- `.github/workflows/deploy-playground.yml:41`
- `.github/workflows/deploy-playground.yml:46`
- `.github/workflows/pr-checks.yml:9`
- `.github/workflows/pr-checks.yml:11`
- `.github/workflows/pr-checks.yml:15`
- `.github/workflows/update-tags-post-release.yml:17`
- `.github/workflows/update-tags-post-release.yml:18`

### missing-permissions (severity: medium)

pr-checks.yml has no top-level `permissions:` block and none of its jobs (eslint, tests, build, build-playground, report-readability) define job-level permissions. This means the workflow runs with the default broad GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/pr-checks.yml:1`

### missing-permissions (severity: medium)

update-tags-post-release.yml has no top-level `permissions:` block and its only job (update-major-and-minor-tags) has no job-level permissions. This means the workflow runs with the default broad GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/update-tags-post-release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files:

1. deploy-playground.yml: Pinned all 6 action references to full SHA hashes (actions/checkout, actions/setup-node, actions/cache, actions/configure-pages, actions/upload-pages-artifact, actions/deploy-pages). This file already had a permissions block.

2. pr-checks.yml: Pinned all action references (actions/checkout, actions/setup-node, actions/cache — used across 5 jobs) to full SHA hashes. Added top-level permissions block with `contents: read` and `pull-requests: write` (the report-readability job posts PR comments).

3. update-tags-post-release.yml: Pinned actions/checkout@v6 and haya14busa/action-update-semver@v1 to full SHA hashes. Added top-level permissions block with `contents: write` (required to push updated semver tags).

