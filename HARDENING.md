<!-- markdownlint-disable -->

# Hardening Report: simple-elf--allure-report-action/v1.15

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **simple-elf--allure-report-action/v1.15** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable version tags (@v4) instead of pinned full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references: `actions/checkout@v4` (used in both files), `peaceiris/actions-gh-pages@v4` (in allure-report.yml). Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/allure-report.yml:18`
- `.github/workflows/allure-report.yml:22`
- `.github/workflows/allure-report.yml:33`
- `.github/workflows/allure-report-pr.yml:11`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no individual job within them declares job-level permissions either. Without explicit permissions, workflows inherit the repository's default token permissions (often `write-all`), violating the principle of least privilege. Both files should declare minimal required permissions (e.g. `contents: read` for checkout, `pages: write` for gh-pages deployment).

Locations:

- `.github/workflows/allure-report.yml:1`
- `.github/workflows/allure-report-pr.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all mutable action tags to full 40-character commit SHAs — actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 (used in both files), peaceiris/actions-gh-pages@v4 → @84c30a85c19949d7eee79c4ff27748b70285e453. (2) Added top-level permissions blocks to both files — allure-report.yml gets 'contents: write' and 'pages: write' (required for gh-pages deployment), allure-report-pr.yml gets 'contents: read' (only needs checkout).

