<!-- markdownlint-disable -->

# Hardening Report: vn7n24fzkq--github-profile-summary-cards/v0.12.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **vn7n24fzkq--github-profile-summary-cards/v0.12.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use tag-based or version-based `uses:` references instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks where a tag is moved to point to malicious code. Affected references:
- deploy.yml: `actions/checkout@v6`, `actions/setup-node@v6`
- manual-release.yml: `actions/checkout@v6`, `actions/setup-node@v6`, `softprops/action-gh-release@v3`
- publish-wiki.yml: `actions/checkout@v6`, `Andrew-Chen-Wang/github-wiki-action@v4`
- test-and-lint.yml: `catchpoint/foresight-workflow-kit-action@v1`, `actions/checkout@v6`, `actions/setup-node@v6`, `catchpoint/foresight-test-kit-action@v1`

Locations:

- `.github/workflows/deploy.yml:21`
- `.github/workflows/deploy.yml:25`
- `.github/workflows/manual-release.yml:28`
- `.github/workflows/manual-release.yml:32`
- `.github/workflows/manual-release.yml:60`
- `.github/workflows/publish-wiki.yml:17`
- `.github/workflows/publish-wiki.yml:19`
- `.github/workflows/test-and-lint.yml:12`
- `.github/workflows/test-and-lint.yml:17`
- `.github/workflows/test-and-lint.yml:19`
- `.github/workflows/test-and-lint.yml:27`
- `.github/workflows/test-and-lint.yml:37`
- `.github/workflows/test-and-lint.yml:40`

### missing-permissions (severity: medium)

The workflow file `test-and-lint.yml` has no top-level `permissions:` block and neither of its jobs (`tests`, `lint`) defines a job-level `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be broader than necessary (e.g., write access to contents). Minimal permissions such as `contents: read` should be declared.

Locations:

- `.github/workflows/test-and-lint.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 13 action references across 4 workflow files to full 40-character SHA hashes (with tag comments for readability): actions/checkout@v6→d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6→249970729cb0ef3589644e2896645e5dc5ba9c38, softprops/action-gh-release@v3→3d0d9888cb7fd7b750713d6e236d1fcb99157228, Andrew-Chen-Wang/github-wiki-action@v4→50650fccf3a10f741995523cf9708c53cec8912a, catchpoint/foresight-workflow-kit-action@v1→105a1f2f6eef7e4403adf5592020391ebfc72583, catchpoint/foresight-test-kit-action@v1→16fcb107ba3a54b63d784ebcc4829b80e0ab79c3. Added top-level `permissions: contents: read` to test-and-lint.yml to address missing-permissions finding.

