<!-- markdownlint-disable -->

# Hardening Report: Kesin11--actions-timeline/v2.2.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kesin11--actions-timeline/v2.2.6** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references across workflow files and the composite action use mutable version tags instead of pinned 40-character SHA digests, making them vulnerable to supply-chain attacks if the upstream tag is moved.

Failing references:
- .github/workflows/ci.yml: actions/checkout@v6 (×3), actions/upload-artifact@v6, k1LoW/octocov-action@v1, actions/download-artifact@v7
- .github/workflows/rebundle.yml: actions/create-github-app-token@v2, actions/checkout@v6
- .github/workflows/release.yml: release-drafter/release-drafter@v6 (×2), actions/github-script@v8, actions/checkout@v6
- .github/actions/setup-deno-with-cache/action.yml: denoland/setup-deno@v1, actions/setup-node@v6

Locations:

- `.github/workflows/ci.yml:21`
- `.github/workflows/ci.yml:29`
- `.github/workflows/ci.yml:35`
- `.github/workflows/ci.yml:48`
- `.github/workflows/ci.yml:53`
- `.github/workflows/ci.yml:64`
- `.github/workflows/rebundle.yml:11`
- `.github/workflows/rebundle.yml:15`
- `.github/workflows/release.yml:20`
- `.github/workflows/release.yml:33`
- `.github/workflows/release.yml:39`
- `.github/workflows/release.yml:48`
- `.github/actions/setup-deno-with-cache/action.yml:13`
- `.github/actions/setup-deno-with-cache/action.yml:16`

### script-injection (severity: high)

release.yml contains direct `${{ ... }}` expression interpolation inside `run:` shell commands and a `github-script` script block (sub-rule a). The `run:` block at lines 52-53 interpolates `${{ steps.release-drafter.outputs.tag_name }}`, `${{ steps.semver.outputs.major }}`, and `${{ steps.semver.outputs.minor }}` directly into shell arguments — these are `steps.*.outputs.*` values that flow through YAML template substitution before the shell processes them, allowing injection of shell metacharacters. The `actions/github-script` step at line 42 similarly interpolates `${{ steps.release-drafter.outputs.tag_name }}` directly into the JavaScript string `const VERSION = "${{ steps.release-drafter.outputs.tag_name }}"` before the JS engine evaluates it.

Offending lines:
  Line 42: `const VERSION = "${{ steps.release-drafter.outputs.tag_name }}"`
  Line 52: `git push -f origin "refs/tags/${{ steps.release-drafter.outputs.tag_name }}:refs/tags/${{ steps.semver.outputs.major }}"`
  Line 53: `git push -f origin "refs/tags/${{ steps.release-drafter.outputs.tag_name }}:refs/tags/${{ steps.semver.outputs.minor }}"`

Locations:

- `.github/workflows/release.yml:42`
- `.github/workflows/release.yml:52`
- `.github/workflows/release.yml:53`

### missing-permissions (severity: medium)

rebundle.yml has no top-level `permissions:` key and no job-level `permissions:` block on any of its jobs. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (write-all by default on many repositories). The job `rebundle-dist` performs a `git push` to main and should declare minimal required permissions explicitly.

Locations:

- `.github/workflows/rebundle.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all findings across 4 files:

1. unpinned-uses: Pinned all 14 action references to full SHA digests with tag comments:
   - actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 (×4 across ci.yml, rebundle.yml, release.yml)
   - actions/upload-artifact@v6 → @b7c566a772e6b6bfb58ed0dc250532a479d7789f
   - k1LoW/octocov-action@v1 → @b3b6ee60482a667950f87553abf1df63217235d9
   - actions/download-artifact@v7 → @37930b1c2abaa49bbe596cd826c3c89aef350131
   - actions/create-github-app-token@v2 → @fee1f7d63c2ff003460e3d139729b119787bc349
   - release-drafter/release-drafter@v6 → @6a93d829887aa2e0748befe2e808c66c0ec6e4c7 (×2)
   - actions/github-script@v8 → @ed597411d8f924073f98dfc5c65a23a2325f34cd
   - denoland/setup-deno@v1 → @11b63cf76cfcafb4e43f97b6cad24d8e8438f62d
   - actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38

2. script-injection (release.yml): Moved all ${{ steps.*.outputs.* }} expressions out of run: shell commands and github-script JS into step env: blocks; shell uses plain $VAR references.

3. missing-permissions (rebundle.yml): Added top-level `permissions: contents: write` (minimal permission needed for git push to main).

