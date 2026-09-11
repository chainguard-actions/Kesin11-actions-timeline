<!-- markdownlint-disable -->

# Hardening Report: Kesin11--actions-timeline/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kesin11--actions-timeline/v3.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action at `.github/actions/setup-deno-with-cache/action.yml` references two external actions using mutable tag refs instead of pinned full-length SHA digests. This exposes the action to supply-chain attacks if the upstream tag is moved or the repository is compromised. Failing references: `denoland/setup-deno@v1` and `actions/setup-node@v6`. These should be pinned to their full 40-character commit SHAs (e.g. `denoland/setup-deno@<sha> # v1`).

Locations:

- `.github/actions/setup-deno-with-cache/action.yml:13`
- `.github/actions/setup-deno-with-cache/action.yml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned two unpinned action references in `.github/actions/setup-deno-with-cache/action.yml`:
- `denoland/setup-deno@v1` → `denoland/setup-deno@11b63cf76cfcafb4e43f97b6cad24d8e8438f62d # v1`
- `actions/setup-node@v6` → `actions/setup-node@249970729cb0ef3589644e2896645e5dc5ba9c38 # v6`

Original tag names preserved as inline comments for readability.

