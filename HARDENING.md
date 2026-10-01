<!-- markdownlint-disable -->

# Hardening Report: Kesin11--actions-timeline/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kesin11--actions-timeline/v3.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action '.github/actions/setup-deno-with-cache/action.yml' references two actions using mutable tag-based refs instead of pinned 40-character commit SHAs. This exposes the action to supply-chain attacks if the upstream tags are moved or compromised. Failing references: `denoland/setup-deno@v1` and `actions/setup-node@v6`.

Locations:

- `.github/actions/setup-deno-with-cache/action.yml:13`
- `.github/actions/setup-deno-with-cache/action.yml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned both unpinned action references in `.github/actions/setup-deno-with-cache/action.yml`:
- `denoland/setup-deno@v1` → `denoland/setup-deno@11b63cf76cfcafb4e43f97b6cad24d8e8438f62d # v1`
- `actions/setup-node@v6` → `actions/setup-node@249970729cb0ef3589644e2896645e5dc5ba9c38 # v6`

Original tags preserved as inline comments for readability.

