<!-- markdownlint-disable -->

# Hardening Report: Rebilly--lexi/v2.3.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Rebilly--lexi/v2.3.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable version tags instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if a tag is moved or an action is compromised.

Failing references in deploy-playground.yml: actions/checkout@v4, actions/setup-node@v4, actions/cache@v4, actions/configure-pages@v5, actions/upload-pages-artifact@v3, actions/deploy-pages@v4.

Failing references in pr-checks.yml: actions/checkout@v4, actions/setup-node@v4, actions/cache@v4 (repeated across multiple jobs).

Failing references in update-tags-post-release.yml: actions/checkout@v4, haya14busa/action-update-semver@v1.

Locations:

- `.github/workflows/deploy-playground.yml:22`
- `.github/workflows/deploy-playground.yml:24`
- `.github/workflows/deploy-playground.yml:26`
- `.github/workflows/deploy-playground.yml:33`
- `.github/workflows/deploy-playground.yml:35`
- `.github/workflows/deploy-playground.yml:38`
- `.github/workflows/pr-checks.yml:11`
- `.github/workflows/pr-checks.yml:13`
- `.github/workflows/pr-checks.yml:15`
- `.github/workflows/update-tags-post-release.yml:18`
- `.github/workflows/update-tags-post-release.yml:19`

### missing-permissions (severity: medium)

Two workflow files have no top-level `permissions:` block and no job-level `permissions:` blocks on any of their jobs. Without explicit permissions, workflows inherit the default repository token permissions, which may be overly broad (e.g., write access to contents). Each workflow should declare the minimal permissions required.

- pr-checks.yml: 4 jobs (eslint, tests, build, build-playground, report-readability) — none have permissions declared.
- update-tags-post-release.yml: 1 job (update-major-and-minor-tags) — no permissions declared.

Locations:

- `.github/workflows/pr-checks.yml:1`
- `.github/workflows/update-tags-post-release.yml:6`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 11 unpinned action references across 3 workflow files by replacing mutable version tags with full commit SHAs (preserving tags as comments). Added top-level permissions blocks to pr-checks.yml (contents: read, pull-requests: write) and update-tags-post-release.yml (contents: write). deploy-playground.yml already had a permissions block and only needed the action SHAs pinned.

