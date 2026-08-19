<!-- markdownlint-disable -->

# Hardening Report: vn7n24fzkq--github-profile-summary-cards/v0.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **vn7n24fzkq--github-profile-summary-cards/v0.7.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in publish-wiki.yml use mutable tag refs instead of pinned 40-character SHA digests, making the workflow vulnerable to supply-chain attacks if the referenced action tags are moved or compromised. Failing references: `actions/checkout@v4` and `Andrew-Chen-Wang/github-wiki-action@v4`.

Locations:

- `.github/workflows/publish-wiki.yml:17`
- `.github/workflows/publish-wiki.yml:18`

### unpinned-uses (severity: high)

All `uses:` references in test-and-lint.yml use mutable tag refs instead of pinned 40-character SHA digests, making the workflow vulnerable to supply-chain attacks. Failing references: `catchpoint/foresight-workflow-kit-action@v1`, `actions/checkout@v4` (×2), `actions/setup-node@v4` (×2), and `catchpoint/foresight-test-kit-action@v1`.

Locations:

- `.github/workflows/test-and-lint.yml:11`
- `.github/workflows/test-and-lint.yml:15`
- `.github/workflows/test-and-lint.yml:17`
- `.github/workflows/test-and-lint.yml:27`
- `.github/workflows/test-and-lint.yml:35`
- `.github/workflows/test-and-lint.yml:37`

### missing-permissions (severity: medium)

The workflow file test-and-lint.yml has no top-level `permissions:` key and neither of its jobs (`tests`, `lint`) defines a job-level `permissions:` block. Without explicit permissions, the workflow inherits the default repository token permissions, which may be overly broad (e.g., write access to contents). A minimal permissions block such as `permissions: contents: read` should be added.

Locations:

- `.github/workflows/test-and-lint.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings:
1. publish-wiki.yml: Pinned `actions/checkout@v4` → SHA `11d5960a326750d5838078e36cf38b85af677262` and `Andrew-Chen-Wang/github-wiki-action@v4` → SHA `50650fccf3a10f741995523cf9708c53cec8912a`. Original `permissions: contents: write` was already present.
2. test-and-lint.yml: Pinned all 6 unpinned action references to full 40-character SHA digests with `# v1` or `# v4` comments for readability. Added top-level `permissions: contents: read` block to satisfy the missing-permissions finding.

