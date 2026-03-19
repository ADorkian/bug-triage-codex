---
name: jira-playwright-mcp-repro
description: Reproduce Jira bugs through Playwright MCP in this repository. Use when Codex must try a browser E2E reproduction for a Tarko or Vulki bug using the repo-local product target mapping and any URL, username, and password overrides provided by the user at runtime. Resolve the product from issue key or explicit product name, choose the matching Tarko or Vulki target, drive the UI with Playwright MCP only, and save local evidence under this repo.
---

# Jira Playwright MCP Repro

## Overview

Use this skill when a Jira bug has enough UI detail to attempt a browser reproduction. Keep the reproduction outcome separate from the triage verdict unless the run clearly confirms or disproves the observed behavior.

The dedicated agent `e2e_reproducer` should use this skill. Its first task is to decide whether the issue is actually suitable for browser E2E, not to force a browser run on every bug.

## Product Routing

Resolve the target before opening the browser:

- `ME-*` or `tarko` -> Tarko
- `TP-*` or `vulki` -> Vulki

If issue key and product name conflict, stop and ask for clarification.

## Inputs Required At Runtime

Prefer the repo-local target mapping in `references/product-targets.json`.

If the selected product still has placeholder values there, require these values from the user:

- base URL
- username
- password
- optional start path after login
- optional browser steps not already present in the normalized issue or triage

If the user provides runtime values, use them as one-run overrides for the chosen product.

## Tool Requirement

Use Playwright MCP only. Do not fall back to local Playwright scripts, Selenium, or ad-hoc browser automation.

If Playwright MCP is not available in the current tool list, stop and report that the skill cannot run in this session.

## Workflow

1. Read the bug context from the repo artifacts or Jira evidence.
2. Decide whether the issue is:
   - `APPLICABILE`
   - `NON_APPLICABILE`
   - `BLOCCATA`
3. Mark the issue `NON_APPLICABILE` when the defect is not meaningfully observable through browser E2E.
4. Mark the issue `BLOCCATA` when E2E would make sense but credentials, environment, data, or UI steps are missing.
5. Only if the issue is `APPLICABILE`, continue with browser execution.
6. Resolve product and load the matching Tarko or Vulki target from `references/product-targets.json`.
7. If the mapping still contains placeholders, ask the user only for the missing values.
8. If the user provides runtime overrides, use them for the current run only.
9. Open the target with Playwright MCP.
10. Perform login with the resolved username and password.
11. Execute only explicit UI steps from the bug evidence or user instructions.
12. Capture evidence:
   - screenshot before login if relevant
   - screenshot after login
   - screenshots at key checkpoints
   - final screenshot
   - final page URL
   - visible error text or confirmation text
13. Save the main report under `docs/e2e-repro/{ISSUE_KEY-or-session}-playwright-mcp.md`.
14. Save supporting machine-readable notes under `artifacts/repro/{ISSUE_KEY-or-session}/notes.json`.
15. Classify the outcome as:
   - `reproduced`
   - `not_reproduced`
   - `login_failed`
   - `steps_blocked`
   - `inconclusive`

## References

- Read `references/product-targets.json` before asking the user for target URL or credentials.
- Read `references/step-plan.md` before generating Playwright MCP actions.
- Use only selectors and assertions that come from explicit UI evidence or a stable local DOM inspection.

## DevExtreme Note

If the target UI uses DevExtreme `dx-*` controls, also apply the interaction rules from the existing `devextreme-playwrite-mcp` skill. Use its selection and snapshot patterns instead of inventing new ones.

## Output Files

Write only inside this repo:

- `docs/e2e-repro/{RUN_ID}-playwright-mcp.md`
- `artifacts/repro/{RUN_ID}/notes.json`

`RUN_ID` should be the issue key when available, otherwise a timestamped session id.

## Report Content

The markdown report should include:

- applicability verdict
- why the issue is or is not suitable for browser E2E
- product resolved
- base URL used
- issue key if available
- login outcome
- steps attempted
- final verdict
- evidence files saved
- blocker if the run was inconclusive

`notes.json` should include machine-readable fields:

- `issueKey`
- `product`
- `baseUrl`
- `status`
- `stepsAttempted`
- `finalUrl`
- `observedText`
- `savedScreenshots`
- `blocker`

## Agent Choice

Use the dedicated `e2e_reproducer` agent for this skill.

The router may schedule it in parallel with `normalizer`, `triage`, and `solution_planner` when the issue is plausibly browser-reproducible or the user explicitly requests browser verification.
