<!-- markdownlint-disable -->

# Hardening Report: vn7n24fzkq--github-profile-summary-cards/v0.6.0-hotfix.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **vn7n24fzkq--github-profile-summary-cards/v0.6.0-hotfix.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file references GitHub Actions using mutable version tags instead of pinned full-length commit SHAs. This exposes the workflow to supply-chain attacks where a tag could be silently moved to point to malicious code. Unpinned references found:
- `runforesight/foresight-workflow-kit-action@v1` (line 21)
- `actions/checkout@v3` (lines 25, 55)
- `actions/setup-node@v3` (lines 27, 57)
- `runforesight/foresight-test-kit-action@v1` (line 38)
All should be pinned to their full 40-character commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`).

Locations:

- `.github/workflows/test-and-lint.yml:21`
- `.github/workflows/test-and-lint.yml:25`
- `.github/workflows/test-and-lint.yml:27`
- `.github/workflows/test-and-lint.yml:38`
- `.github/workflows/test-and-lint.yml:55`
- `.github/workflows/test-and-lint.yml:57`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key, and neither the `tests` job nor the `lint` job defines a job-level `permissions:` block. Without explicit permissions, the workflow inherits the default repository token permissions, which may be overly broad (write access to contents, packages, etc.). A minimal `permissions:` block should be added at the top level or per job (e.g. `permissions: read-all` or specific scopes like `contents: read`).

Locations:

- `.github/workflows/test-and-lint.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed .github/workflows/test-and-lint.yml: (1) Pinned all 4 action references to full commit SHAs — runforesight/foresight-workflow-kit-action@v1 → @105a1f2f6eef7e4403adf5592020391ebfc72583, actions/checkout@v3 → @f43a0e5ff2bd294095638e18286ca9a3d1956744, actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610, runforesight/foresight-test-kit-action@v1 → @16fcb107ba3a54b63d784ebcc4829b80e0ab79c3. Original tags preserved as inline comments. (2) Added top-level `permissions: contents: read` block to restrict the GITHUB_TOKEN to the minimum needed for checkout and test/lint operations.

