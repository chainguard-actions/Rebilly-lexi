<!-- markdownlint-disable -->

# Hardening Report: Rebilly--lexi/v2.3.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Rebilly--lexi/v2.3.4** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All uses: references in deploy-playground.yml use mutable tag-based refs instead of full 40-character SHA commit hashes, making the workflow vulnerable to supply-chain attacks if the referenced action tags are moved. Unpinned refs: actions/checkout@v4, actions/setup-node@v4, actions/cache@v4, actions/configure-pages@v5, actions/upload-pages-artifact@v3, actions/deploy-pages@v4.

Locations:

- `.github/workflows/deploy-playground.yml:24`
- `.github/workflows/deploy-playground.yml:26`
- `.github/workflows/deploy-playground.yml:29`
- `.github/workflows/deploy-playground.yml:37`
- `.github/workflows/deploy-playground.yml:40`
- `.github/workflows/deploy-playground.yml:45`

### unpinned-uses (severity: high)

All uses: references in pr-checks.yml use mutable tag-based refs instead of full 40-character SHA commit hashes. Unpinned refs (repeated across jobs): actions/checkout@v4, actions/setup-node@v4, actions/cache@v4, and the local ./ self-reference.

Locations:

- `.github/workflows/pr-checks.yml:10`
- `.github/workflows/pr-checks.yml:12`
- `.github/workflows/pr-checks.yml:15`
- `.github/workflows/pr-checks.yml:27`
- `.github/workflows/pr-checks.yml:29`
- `.github/workflows/pr-checks.yml:32`
- `.github/workflows/pr-checks.yml:43`
- `.github/workflows/pr-checks.yml:45`
- `.github/workflows/pr-checks.yml:48`
- `.github/workflows/pr-checks.yml:62`
- `.github/workflows/pr-checks.yml:64`
- `.github/workflows/pr-checks.yml:67`
- `.github/workflows/pr-checks.yml:80`

### unpinned-uses (severity: high)

All uses: references in update-tags-post-release.yml use mutable tag-based refs instead of full 40-character SHA commit hashes. Unpinned refs: actions/checkout@v4, haya14busa/action-update-semver@v1.

Locations:

- `.github/workflows/update-tags-post-release.yml:17`
- `.github/workflows/update-tags-post-release.yml:18`

### missing-permissions (severity: medium)

pr-checks.yml has no top-level permissions: key and none of its jobs (eslint, tests, build, build-playground, report-readability) define a permissions: block. This means the workflow runs with the default broad GITHUB_TOKEN permissions, which include write access to contents and other scopes depending on the repository settings.

Locations:

- `.github/workflows/pr-checks.yml:1`

### missing-permissions (severity: medium)

update-tags-post-release.yml has no top-level permissions: key and its only job (update-major-and-minor-tags) has no permissions: block. This means the workflow runs with the default broad GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/update-tags-post-release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

1. deploy-playground.yml: Pinned 6 action references to full SHA hashes (actions/checkout, actions/setup-node, actions/cache, actions/configure-pages, actions/upload-pages-artifact, actions/deploy-pages). This file already had appropriate permissions.

2. pr-checks.yml: Pinned all action references (actions/checkout, actions/setup-node, actions/cache) across all 5 jobs to full SHAs. Added top-level permissions block with `contents: read` and `pull-requests: write` (needed for the report-readability job that posts PR comments). The local `./` self-reference is not a remote action and requires no pinning.

3. update-tags-post-release.yml: Pinned actions/checkout@v4 and haya14busa/action-update-semver@v1 to full SHAs. Added top-level permissions block with `contents: write` (required to push/update git tags).

