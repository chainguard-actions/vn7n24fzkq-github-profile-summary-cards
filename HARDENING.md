<!-- markdownlint-disable -->

# Hardening Report: vn7n24fzkq--github-profile-summary-cards/v0.6.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **vn7n24fzkq--github-profile-summary-cards/v0.6.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file .github/workflows/test-and-lint.yml has no top-level `permissions:` key, and neither the `tests` job nor the `lint` job defines its own `permissions:` block. This means the workflow runs with the default (potentially broad) GitHub token permissions. A top-level or per-job `permissions:` block with minimal specific scopes should be added.

Locations:

- `.github/workflows/test-and-lint.yml:1`

### unpinned-uses (severity: high)

Multiple `uses:` references in .github/workflows/test-and-lint.yml are pinned to mutable tags or version strings rather than immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced action tags are moved or compromised. Failing references:
- `runforesight/foresight-workflow-kit-action@v1` (line ~19)
- `actions/checkout@v3` (line ~23)
- `actions/setup-node@v3` (line ~25)
- `runforesight/foresight-test-kit-action@v1` (line ~33)
- `actions/checkout@v3` (line ~50)
- `actions/setup-node@v3` (line ~52)
Each should be replaced with a full SHA pin, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/test-and-lint.yml:19`
- `.github/workflows/test-and-lint.yml:23`
- `.github/workflows/test-and-lint.yml:25`
- `.github/workflows/test-and-lint.yml:33`
- `.github/workflows/test-and-lint.yml:50`
- `.github/workflows/test-and-lint.yml:52`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

Fixed .github/workflows/test-and-lint.yml: (1) Added top-level `permissions: {}` and per-job `permissions: contents: read` blocks for both `tests` and `lint` jobs. (2) Pinned all 6 unpinned action references to full 40-character commit SHAs: runforesight/foresight-workflow-kit-action@v1→105a1f2f..., actions/checkout@v3→a37ce912... (×2), actions/setup-node@v3→3235b876... (×2), runforesight/foresight-test-kit-action@v1→16fcb107.... Original tags preserved as inline comments.

