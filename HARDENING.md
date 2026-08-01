<!-- markdownlint-disable -->

# Hardening Report: Kesin11--actions-timeline/v3.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kesin11--actions-timeline/v3.2.0** was hardened automatically. 3 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions by mutable tags instead of full 40-character SHA digests, making them vulnerable to supply-chain attacks if the tag is moved.

- ci.yml: `actions/checkout@v7`, `actions/upload-artifact@v7`, `k1LoW/octocov-action@v1`, `actions/download-artifact@v8`
- release.yml: `release-drafter/release-drafter@v7` (×2), `actions/github-script@v9`, `actions/checkout@v7`
- rebundle.yml: `actions/create-github-app-token@v3`, `actions/checkout@v7`
- copilot-setup-steps.yml: `actions/checkout@v7`

Locations:

- `.github/workflows/ci.yml:19`
- `.github/workflows/ci.yml:27`
- `.github/workflows/ci.yml:43`
- `.github/workflows/ci.yml:57`
- `.github/workflows/ci.yml:62`
- `.github/workflows/ci.yml:75`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:27`
- `.github/workflows/release.yml:33`
- `.github/workflows/release.yml:40`
- `.github/workflows/rebundle.yml:12`
- `.github/workflows/rebundle.yml:18`
- `.github/workflows/copilot-setup-steps.yml:30`

### missing-permissions (severity: medium)

rebundle.yml has no top-level `permissions:` key and no job-level `permissions:` key on any of its jobs. Without explicit permissions, the workflow inherits the repository default (typically `contents: write` for push-triggered workflows), granting broader access than necessary.

Locations:

- `.github/workflows/rebundle.yml:1`

### script-injection (severity: high)

In release.yml, the 'Update major and minor git tags' run: block directly interpolates GitHub Actions expressions into shell commands (sub-rule a). The values `${{ steps.release-drafter.outputs.tag_name }}`, `${{ steps.semver.outputs.major }}`, and `${{ steps.semver.outputs.minor }}` are expanded by the Actions template engine before the shell sees them, allowing an attacker who can influence these step outputs to inject arbitrary shell commands.

Offending lines:
  git push -f origin "refs/tags/${{ steps.release-drafter.outputs.tag_name }}:refs/tags/${{ steps.semver.outputs.major }}"
  git push -f origin "refs/tags/${{ steps.release-drafter.outputs.tag_name }}:refs/tags/${{ steps.semver.outputs.minor }}"

Fix: store the values in env vars and reference them as quoted shell variables, e.g.:
  env:
    TAG_NAME: ${{ steps.release-drafter.outputs.tag_name }}
    MAJOR: ${{ steps.semver.outputs.major }}
    MINOR: ${{ steps.semver.outputs.minor }}
  run: |
    git push -f origin "refs/tags/${TAG_NAME}:refs/tags/${MAJOR}"
    git push -f origin "refs/tags/${TAG_NAME}:refs/tags/${MINOR}"

Locations:

- `.github/workflows/release.yml:46`
- `.github/workflows/release.yml:47`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all three findings across four workflow files:

1. unpinned-uses: Pinned all mutable tag references to full 40-char SHAs with tag comments preserved:
   - actions/checkout@v7 → @3d3c42e5aac5ba805825da76410c181273ba90b1 (used in ci.yml ×3, release.yml, rebundle.yml, copilot-setup-steps.yml)
   - actions/upload-artifact@v7 → @043fb46d1a93c77aae656e7c1c64a875d1fc6a0a (ci.yml)
   - k1LoW/octocov-action@v1 → @a167dc0dee441b7ffc45e1b862ab55ec0d87f278 (ci.yml)
   - actions/download-artifact@v8 → @3e5f45b2cfb9172054b4087a40e8e0b5a5461e7c (ci.yml)
   - release-drafter/release-drafter@v7 → @34d80673e067bdc0c24568d3af899c216adcfaa9 (release.yml ×2)
   - actions/github-script@v9 → @3a2844b7e9c422d3c10d287c895573f7108da1b3 (release.yml)
   - actions/create-github-app-token@v3 → @bcd2ba49218906704ab6c1aa796996da409d3eb1 (rebundle.yml)

2. missing-permissions: Added `permissions: contents: write` to rebundle.yml (needed for git push).

3. script-injection: In release.yml 'Update major and minor git tags' step, moved TAG_NAME, MAJOR, and MINOR expressions into an env: block and referenced them as shell variables in the run: block.

### Iteration 2

**Fixes applied:** unpinned-uses

**Notes:**

Pinned both unpinned action references in hardened/action/.github/actions/setup-deno-with-cache/action.yml:
- `denoland/setup-deno@v2.0.5` → `denoland/setup-deno@22d081ff2d3a40755e97629de92e3bcbfa7cf2ed # v2.0.5`
- `actions/setup-node@v7` → `actions/setup-node@820762786026740c76f36085b0efc47a31fe5020 # v7`
SHAs were resolved via lookup_action_sha.

