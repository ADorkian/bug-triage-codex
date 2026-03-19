# E2E Repro Template

Use this template when `e2e_reproducer` evaluates whether a bug can be reproduced via browser E2E with Playwright MCP.

## Purpose

The E2E repro document is a per-issue engineering note stored under `docs/e2e-repro/`. It answers two separate questions:

1. Is this bug actually suitable for browser E2E validation?
2. If yes, what happened when Playwright MCP attempted the reproduction?

## Applicability Rules

Use one of these values:

- `APPLICABILE`: the issue exposes an observable browser flow and enough explicit UI evidence exists to try it
- `NON_APPLICABILE`: the issue is not meaningfully verifiable through browser E2E
- `BLOCCATA`: browser E2E might be useful, but the run is blocked by missing data, environment, credentials, or Playwright MCP availability

## Execution Result Values

Use one of these values when an execution attempt happens:

- `reproduced`
- `not_reproduced`
- `login_failed`
- `steps_blocked`
- `inconclusive`

## Required Sections

1. Applicability verdict
2. Why the issue is or is not suitable for browser E2E
3. Product and target URL
4. Whether Playwright MCP execution was attempted
5. Execution result
6. Steps attempted
7. Observed evidence
8. Saved evidence paths
9. Remaining blockers

## Canonical Shape

See `templates/e2e-repro.md`.
