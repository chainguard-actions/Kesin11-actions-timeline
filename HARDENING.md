<!-- markdownlint-disable -->

# Hardening Report: Kesin11--actions-timeline/v3.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kesin11--actions-timeline/v3.1.0** was hardened automatically. 3 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable version tags instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks if a tag is moved or a repository is compromised.

- ci.yml: actions/checkout@v6 (lines 21, 30, 51), actions/upload-artifact@v7 (line 37), k1LoW/octocov-action@v1 (line 48), actions/download-artifact@v8 (line 63)
- rebundle.yml: actions/create-github-app-token@v3 (line 12), actions/checkout@v6 (line 17)
- release.yml: release-drafter/release-drafter@v7 (lines 20, 31), actions/github-script@v9 (line 36), actions/checkout@v6 (line 44)
- copilot-setup-steps.yml: actions/checkout@v6 (line 33)

Locations:

- `.github/workflows/ci.yml:21`
- `.github/workflows/ci.yml:30`
- `.github/workflows/ci.yml:37`
- `.github/workflows/ci.yml:48`
- `.github/workflows/ci.yml:51`
- `.github/workflows/ci.yml:63`
- `.github/workflows/rebundle.yml:12`
- `.github/workflows/rebundle.yml:17`
- `.github/workflows/release.yml:20`
- `.github/workflows/release.yml:31`
- `.github/workflows/release.yml:36`
- `.github/workflows/release.yml:44`
- `.github/workflows/copilot-setup-steps.yml:33`

### missing-permissions (severity: medium)

The workflow file rebundle.yml has no top-level `permissions:` key and no job-level `permissions:` key on any of its jobs. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents). All permissions should be explicitly declared using the principle of least privilege.

Locations:

- `.github/workflows/rebundle.yml:1`

### script-injection (severity: high)

In release.yml, the 'Update major and minor git tags' run: block directly interpolates GitHub Actions expressions into shell commands without going through an env: variable, violating rule (a). The values `${{ steps.release-drafter.outputs.tag_name }}`, `${{ steps.semver.outputs.major }}`, and `${{ steps.semver.outputs.minor }}` are substituted by the Actions template engine before the shell ever sees them, allowing an attacker who can influence the release-drafter output to inject arbitrary shell commands.

Offending lines:
  `git push -f origin "refs/tags/${{ steps.release-drafter.outputs.tag_name }}:refs/tags/${{ steps.semver.outputs.major }}"`
  `git push -f origin "refs/tags/${{ steps.release-drafter.outputs.tag_name }}:refs/tags/${{ steps.semver.outputs.minor }}"`

Additionally, the 'Create semver outputs' github-script step interpolates `${{ steps.release-drafter.outputs.tag_name }}` directly into a JavaScript string literal inside the `script:` block, which is also a template-injection risk.

Locations:

- `.github/workflows/release.yml:46`
- `.github/workflows/release.yml:47`
- `.github/workflows/release.yml:39`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all 13 unpinned action references across ci.yml, rebundle.yml, release.yml, and copilot-setup-steps.yml by pinning each to its full 40-character commit SHA (with the original tag preserved as a comment). Added `permissions: contents: write` to rebundle.yml which had no permissions block. Fixed script injection in release.yml by moving `${{ steps.release-drafter.outputs.tag_name }}` out of the github-script JavaScript string literal into an env var (TAG_NAME) accessed via process.env.TAG_NAME, and moved TAG_NAME, MAJOR, and MINOR expressions out of the shell run block into the step's env: block so they are never interpolated directly into shell commands.

### Iteration 2

**Fixes applied:** unpinned-uses

**Notes:**

Pinned both unpinned action references in hardened/action/.github/actions/setup-deno-with-cache/action.yml: (1) denoland/setup-deno@v1 → @11b63cf76cfcafb4e43f97b6cad24d8e8438f62d # v1; (2) actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38 # v6. Original tags preserved as comments for readability.

