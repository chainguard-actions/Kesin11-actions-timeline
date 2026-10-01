<!-- markdownlint-disable -->

# Hardening Report: Kesin11--actions-timeline/v2.2.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kesin11--actions-timeline/v2.2.4** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action '.github/actions/setup-deno-with-cache/action.yml' references external actions using mutable version tags instead of full 40-character commit SHA digests. This is a supply-chain risk: if the upstream tag is moved to a different commit (maliciously or accidentally), the action will silently execute different, potentially malicious code. Failing references: 'denoland/setup-deno@v1' and 'actions/setup-node@v4'. These should be pinned to their full commit SHAs, e.g. 'denoland/setup-deno@<40-char-sha> # v1'.

Locations:

- `.github/actions/setup-deno-with-cache/action.yml:13`
- `.github/actions/setup-deno-with-cache/action.yml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned both unpinned action references in `.github/actions/setup-deno-with-cache/action.yml`:
- `denoland/setup-deno@v1` → `denoland/setup-deno@11b63cf76cfcafb4e43f97b6cad24d8e8438f62d # v1`
- `actions/setup-node@v4` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`

SHAs were resolved using lookup_action_sha and the original version tags are preserved as inline comments for readability.

