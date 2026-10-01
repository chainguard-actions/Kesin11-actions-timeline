<!-- markdownlint-disable -->

# Hardening Report: Kesin11--actions-timeline/v3.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Kesin11--actions-timeline/v3.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action '.github/actions/setup-deno-with-cache/action.yml' references two external actions using mutable tag refs instead of pinned full-length SHA digests. This exposes the action to supply-chain attacks where a tag could be silently moved to point to malicious code.

Failing references:
- Line 13: `uses: denoland/setup-deno@v2.0.5` (tag ref, not a SHA)
- Line 16: `uses: actions/setup-node@v7` (tag ref, not a SHA)

These should be pinned to their full 40-character commit SHAs, e.g.:
  `uses: denoland/setup-deno@<40-char-sha> # v2.0.5`
  `uses: actions/setup-node@<40-char-sha> # v7`

Locations:

- `.github/actions/setup-deno-with-cache/action.yml:13`
- `.github/actions/setup-deno-with-cache/action.yml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned two unpinned action references in hardened/action/.github/actions/setup-deno-with-cache/action.yml:
- `denoland/setup-deno@v2.0.5` → `denoland/setup-deno@22d081ff2d3a40755e97629de92e3bcbfa7cf2ed # v2.0.5`
- `actions/setup-node@v7` → `actions/setup-node@820762786026740c76f36085b0efc47a31fe5020 # v7`

