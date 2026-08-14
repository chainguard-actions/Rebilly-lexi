<!-- markdownlint-disable -->

# Hardening Report: Rebilly--lexi/v2.3.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Rebilly--lexi/v2.3.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across all three workflow files use mutable tag-based refs instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks where a compromised or updated tag could silently execute malicious code.

Failing references in deploy-playground.yml: actions/checkout@v4, actions/setup-node@v4, actions/cache@v4, actions/configure-pages@v5, actions/upload-pages-artifact@v3, actions/deploy-pages@v4.

Failing references in pr-checks.yml: actions/checkout@v4, actions/setup-node@v4, actions/cache@v4 (used in 4 jobs).

Failing references in update-tags-post-release.yml: actions/checkout@v4, haya14busa/action-update-semver@v1.

Locations:

- `.github/workflows/deploy-playground.yml:24`
- `.github/workflows/deploy-playground.yml:27`
- `.github/workflows/deploy-playground.yml:31`
- `.github/workflows/deploy-playground.yml:38`
- `.github/workflows/deploy-playground.yml:41`
- `.github/workflows/deploy-playground.yml:45`
- `.github/workflows/pr-checks.yml:9`
- `.github/workflows/pr-checks.yml:11`
- `.github/workflows/pr-checks.yml:14`
- `.github/workflows/update-tags-post-release.yml:16`
- `.github/workflows/update-tags-post-release.yml:17`

### missing-permissions (severity: medium)

pr-checks.yml and update-tags-post-release.yml have no top-level `permissions:` block and no job-level `permissions:` blocks on any of their jobs. Without explicit permissions, workflows inherit the default repository permissions (which may include write access to contents, pull-requests, etc.), violating the principle of least privilege.

Locations:

- `.github/workflows/pr-checks.yml:1`
- `.github/workflows/update-tags-post-release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned `uses:` references across all three workflow files by replacing mutable tag refs with full 40-character SHA commit hashes (preserving the original tag as a comment). Added top-level `permissions:` blocks to pr-checks.yml (contents: read, pull-requests: read) and update-tags-post-release.yml (contents: write, required to push updated semver tags). deploy-playground.yml already had a permissions block and only needed the SHA pinning.

