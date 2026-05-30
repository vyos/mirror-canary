# AGENTS.md — `vyos/mirror-canary`

## Project purpose

Canary / test-target repository for the VyOS mirror pipeline (Rollout 1a and beyond). It is the **source side** of the `vyos` → `VyOS-Networks` PR-mirror pair. Its sister repo `VyOS-Networks/mirror-canary` is the mirror destination.

The repo exists solely to validate mirror-pipeline mechanics end-to-end: PR creation, App-token authentication, Mergify coupling, multi-branch backport, conflict detection, and mirror-refusal semantics. It is not a software product.

## Sibling — not a mirror — pair

`vyos/mirror-canary` (source) and `VyOS-Networks/mirror-canary` (destination) form a canary pair. Changes pushed to `vyos/mirror-canary` trigger the central reusable workflow (`vyos/.github/.github/workflows/pr-mirror-repo-sync.yml`) which opens a mirror PR on `VyOS-Networks/mirror-canary`. They are deliberately kept structurally symmetric so the pipeline can be tested without touching production repos.

## Repository layout

```
README.md          — human notes on canary test scenarios
tests/             — small text files used as test payloads for each scenario
```

## Tech stack

- No build system, no language runtime required.
- Test payloads are plain text files committed to `tests/`.
- Mirror pipeline: GitHub Actions reusable workflow; Mergify central config (`extends: mergify`) on both sides.

## Default branch and branch conventions

Default branch: `rolling` (plus long-lived `sagitta` and `circinus` for backport testing).

Commit title format: `T<num>: <description>` or `test: <scenario>` for canary test commits.

Do not commit credentials or secrets.

## When to touch this repo

- During mirror-pipeline rollout verification (Rollout 1a / 1b / 2 / 3 / 4 etc.) to smoke-test new pipeline behaviour.
- To add a new test scenario: create a small file under `tests/`, open a PR, and observe the mirror pipeline's response on `VyOS-Networks/mirror-canary`.
- Safe to discard test branches after verification; the default branch `rolling` is the durable baseline.
