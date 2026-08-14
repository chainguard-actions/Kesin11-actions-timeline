<!-- markdownlint-disable -->

# Hardening Report: Kesin11--actions-timeline/v2.2.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kesin11--actions-timeline/v2.2.4** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags instead of pinned full-length SHA digests, making them vulnerable to supply-chain attacks if the tag is moved.

.github/workflows/ci.yml: actions/checkout@v4 (lines 21, 29, 51), actions/upload-artifact@v4 (line 36), k1LoW/octocov-action@v1 (line 47), actions/download-artifact@v4 (line 62).

.github/workflows/rebundle.yml: actions/create-github-app-token@v2 (line 12), actions/checkout@v4 (line 14).

.github/workflows/release.yml: release-drafter/release-drafter@v6 (lines 19, 31), actions/github-script@v7 (line 36), actions/checkout@v4 (line 40).

Locations:

- `.github/workflows/ci.yml:21`
- `.github/workflows/ci.yml:29`
- `.github/workflows/ci.yml:36`
- `.github/workflows/ci.yml:47`
- `.github/workflows/ci.yml:51`
- `.github/workflows/ci.yml:62`
- `.github/workflows/rebundle.yml:12`
- `.github/workflows/rebundle.yml:14`
- `.github/workflows/release.yml:19`
- `.github/workflows/release.yml:31`
- `.github/workflows/release.yml:36`
- `.github/workflows/release.yml:40`

### missing-permissions (severity: medium)

rebundle.yml has no top-level `permissions:` key and no job-level `permissions:` block on any of its jobs. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be broader than necessary (especially given that this workflow pushes commits to main).

Locations:

- `.github/workflows/rebundle.yml:1`

### script-injection (severity: high)

release.yml directly interpolates ${{ }} expressions inside run: shell commands and a github-script script: block, allowing template-substituted values to be interpreted as shell/JS code before any quoting or sanitization occurs.

(a) In the 'Update major and minor git tags' run: block (lines 43–44), `${{ steps.release-drafter.outputs.tag_name }}`, `${{ steps.semver.outputs.major }}`, and `${{ steps.semver.outputs.minor }}` are interpolated directly into git push shell commands. If a release-drafter output contains shell metacharacters, they will be executed by the shell.

(b) In the 'Create semver outputs' step (line 38), `${{ steps.release-drafter.outputs.tag_name }}` is interpolated directly inside the github-script `script:` JavaScript block as: `const VERSION = "${{ steps.release-drafter.outputs.tag_name }}"`. This value is template-substituted before the JS engine sees it, enabling JS injection if the tag name contains quote characters or JS syntax.

Locations:

- `.github/workflows/release.yml:38`
- `.github/workflows/release.yml:43`
- `.github/workflows/release.yml:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all 12 unpinned action references across ci.yml, rebundle.yml, and release.yml by resolving each tag to its full commit SHA (format: owner/repo@SHA # tag). Added `permissions: contents: write` to rebundle.yml which was missing any permissions block. Fixed script injection in release.yml: (a) moved `${{ steps.release-drafter.outputs.tag_name }}` out of the github-script JS block into an env var (TAG_NAME) and referenced it via `process.env.TAG_NAME`; (b) moved all three `${{ }}` expressions in the git push run step into the step's env block and referenced them as plain shell variables.

