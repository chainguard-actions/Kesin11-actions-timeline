<!-- markdownlint-disable -->

# Hardening Report: Kesin11--actions-timeline/v3.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Kesin11--actions-timeline/v3.1.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action at .github/actions/setup-deno-with-cache/action.yml references two actions using mutable version tags instead of full 40-character commit SHA digests. This exposes the action to supply-chain attacks where a tag could be moved to point to malicious code. Failing references: `denoland/setup-deno@v2.0.4` and `actions/setup-node@v6`.

Locations:

- `.github/actions/setup-deno-with-cache/action.yml:13`
- `.github/actions/setup-deno-with-cache/action.yml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned both unpinned action references in .github/actions/setup-deno-with-cache/action.yml:
- `denoland/setup-deno@v2.0.4` → `denoland/setup-deno@667a34cdef165d8d2b2e98dde39547c9daac7282 # v2.0.4`
- `actions/setup-node@v6` → `actions/setup-node@48b55a011bda9f5d6aeb4c2d9c7362e8dae4041e # v6`

Original version tags are preserved as inline comments for readability.

