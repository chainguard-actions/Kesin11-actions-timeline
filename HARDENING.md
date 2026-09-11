<!-- markdownlint-disable -->

# Hardening Report: Kesin11--actions-timeline/v3.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kesin11--actions-timeline/v3.1.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action at .github/actions/setup-deno-with-cache/action.yml references two actions pinned to mutable version tags rather than immutable full SHA commits. This exposes the action to supply-chain attacks if the upstream tag is moved or the repository is compromised.

Failing references:
- `uses: denoland/setup-deno@v2.0.4` (line 13) — pinned to a version tag, not a SHA
- `uses: actions/setup-node@v6` (line 16) — pinned to a major version tag, not a SHA

Each should be replaced with the full 40-character commit SHA, e.g.:
  uses: denoland/setup-deno@<40-char-sha> # v2.0.4
  uses: actions/setup-node@<40-char-sha> # v6

Locations:

- `.github/actions/setup-deno-with-cache/action.yml:13`
- `.github/actions/setup-deno-with-cache/action.yml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned both mutable tag references in hardened/action/.github/actions/setup-deno-with-cache/action.yml:
- `denoland/setup-deno@v2.0.4` → `denoland/setup-deno@667a34cdef165d8d2b2e98dde39547c9daac7282 # v2.0.4`
- `actions/setup-node@v6` → `actions/setup-node@249970729cb0ef3589644e2896645e5dc5ba9c38 # v6`

