<!-- markdownlint-disable -->

# Hardening Report: simple-elf--allure-report-action/v1.13

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **simple-elf--allure-report-action/v1.13** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference external actions using mutable version tags (@v4) instead of full 40-character commit SHA digests. This exposes the workflow to supply-chain attacks if the tag is moved to a different (potentially malicious) commit. Affected references:
- .github/workflows/allure-report-pr.yml: `actions/checkout@v4`
- .github/workflows/allure-report.yml: `actions/checkout@v4` (×2), `peaceiris/actions-gh-pages@v4`

Locations:

- `.github/workflows/allure-report-pr.yml:10`
- `.github/workflows/allure-report.yml:15`
- `.github/workflows/allure-report.yml:20`
- `.github/workflows/allure-report.yml:33`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no job in either file has its own `permissions:` block. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions, which may include write access to repository contents, pull requests, and packages. Explicit minimal permissions should be declared.

Locations:

- `.github/workflows/allure-report.yml:1`
- `.github/workflows/allure-report-pr.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four unpinned action references by replacing mutable @v4 tags with full 40-character commit SHAs (actions/checkout → 11d5960a326750d5838078e36cf38b85af677262, peaceiris/actions-gh-pages → 84c30a85c19949d7eee79c4ff27748b70285e453). Added top-level permissions blocks to both workflow files: allure-report-pr.yml gets 'permissions: {}' (no write access needed), and allure-report.yml gets 'permissions: contents: write' (required for the peaceiris/actions-gh-pages step to push to the gh-pages branch).

