<!-- markdownlint-disable -->

# Hardening Report: simple-elf--allure-report-action/v1.14

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **simple-elf--allure-report-action/v1.14** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable version tags instead of pinned 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced action tags are moved or compromised. Failing references: `actions/checkout@v4` (in both workflows), `peaceiris/actions-gh-pages@v4` (in allure-report.yml). These should be pinned to their full SHA digests, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/allure-report-pr.yml:12`
- `.github/workflows/allure-report.yml:15`
- `.github/workflows/allure-report.yml:20`
- `.github/workflows/allure-report.yml:32`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` block, and no individual job within either file defines its own `permissions:` block. Without explicit permissions, the default token permissions (which can be broad, including write access) are granted to every job. This is especially risky for `allure-report-pr.yml`, which is triggered on `pull_request` events and could be exploited by a malicious PR. A minimal `permissions:` block (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/allure-report.yml:1`
- `.github/workflows/allure-report-pr.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all action references to full SHA digests — actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 (3 occurrences across both files), peaceiris/actions-gh-pages@v4 → @84c30a85c19949d7eee79c4ff27748b70285e453. (2) Added top-level permissions blocks — allure-report-pr.yml gets 'contents: read' (minimal for a PR-triggered workflow), allure-report.yml gets 'contents: write' (required for the gh-pages deployment step).

