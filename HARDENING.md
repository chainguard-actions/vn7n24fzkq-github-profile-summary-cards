<!-- markdownlint-disable -->

# Hardening Report: vn7n24fzkq--github-profile-summary-cards/v0.11.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **vn7n24fzkq--github-profile-summary-cards/v0.11.5** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference GitHub Actions using mutable version tags instead of pinned full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks where a compromised or malicious tag update could execute arbitrary code in the runner.

Failing references:
- manual-release.yml: actions/checkout@v6, actions/setup-node@v6, softprops/action-gh-release@v3
- publish-wiki.yml: actions/checkout@v6, Andrew-Chen-Wang/github-wiki-action@v4
- test-and-lint.yml: catchpoint/foresight-workflow-kit-action@v1, actions/checkout@v6, actions/setup-node@v6, catchpoint/foresight-test-kit-action@v1

All uses: references must be pinned to a full SHA (e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4).

Locations:

- `.github/workflows/manual-release.yml:25`
- `.github/workflows/manual-release.yml:30`
- `.github/workflows/manual-release.yml:67`
- `.github/workflows/publish-wiki.yml:21`
- `.github/workflows/publish-wiki.yml:25`
- `.github/workflows/test-and-lint.yml:11`
- `.github/workflows/test-and-lint.yml:15`
- `.github/workflows/test-and-lint.yml:17`
- `.github/workflows/test-and-lint.yml:24`
- `.github/workflows/test-and-lint.yml:33`
- `.github/workflows/test-and-lint.yml:35`

### missing-permissions (severity: medium)

The workflow file test-and-lint.yml has no top-level `permissions:` key and neither of its two jobs (`tests` and `lint`) defines a `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents). A `permissions:` block with minimal required scopes should be added at the top level or on each job.

Locations:

- `.github/workflows/test-and-lint.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full SHA hashes across three workflow files:
- manual-release.yml: actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38, softprops/action-gh-release@v3 → @3d0d9888cb7fd7b750713d6e236d1fcb99157228
- publish-wiki.yml: actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803, Andrew-Chen-Wang/github-wiki-action@v4 → @50650fccf3a10f741995523cf9708c53cec8912a
- test-and-lint.yml: catchpoint/foresight-workflow-kit-action@v1 → @105a1f2f6eef7e4403adf5592020391ebfc72583, actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 (×2), actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38 (×2), catchpoint/foresight-test-kit-action@v1 → @16fcb107ba3a54b63d784ebcc4829b80e0ab79c3

Added `permissions: {}` at the top level of test-and-lint.yml to enforce least-privilege (the workflow only runs tests and linting and requires no GitHub token permissions). Original tags preserved as inline comments for readability.

