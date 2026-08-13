<!-- markdownlint-disable -->

# Hardening Report: EndBug--label-sync/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **EndBug--label-sync/v2.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow files use mutable version tags instead of pinned 40-character commit SHAs. This exposes the action to supply-chain attacks if the referenced action is compromised or the tag is moved. Failing references:
- `actions/checkout@v3` (.github/workflows/labels.yml, .github/workflows/test.yml ×2)
- `EndBug/label-sync@v2` (.github/workflows/labels.yml)
- `Actions-R-Us/actions-tagger@v2` (.github/workflows/versioning.yml)
Each should be replaced with a full 40-character SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/labels.yml:9`
- `.github/workflows/labels.yml:10`
- `.github/workflows/test.yml:9`
- `.github/workflows/test.yml:17`
- `.github/workflows/versioning.yml:8`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and no individual job within them defines job-level permissions either. Without explicit permissions, workflows run with the default (often broad) token permissions, violating the principle of least privilege. A minimal `permissions:` block (e.g. `contents: read`) should be added to each workflow.

Locations:

- `.github/workflows/labels.yml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned `uses:` references by replacing mutable tags with full 40-character commit SHAs (verified via lookup_action_sha), preserving the original tag in a trailing comment. Added top-level `permissions:` blocks to all three workflow files with minimal required permissions: labels.yml gets `contents: read` + `issues: write` (for label management), test.yml gets `contents: read` (checkout only), and versioning.yml gets `contents: write` (for creating/updating git tags).

