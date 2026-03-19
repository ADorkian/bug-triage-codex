# Routing Rules

This document defines the router behavior for every triage run.

## Issue Selection

1. Read exactly the first 8 Jira issues of type `Bug` from the configured kanban source, restricted to status `To do`.
2. Use the configured priority order from `.codex/config.toml`.
3. Do not skip a bug just because it looks incomplete; the completeness path is part of the workflow.
4. The effective board/filter query must include `status = "To do"`.

## State Routing

Use source routing before starting:

### Product -> Jira project mapping

| Product | Jira project | Issue prefix | Jira board |
|---|---|---|---|
| Vulki | `TP` | `TP-*` | [TP board 73](https://support-akeron.atlassian.net/jira/software/c/projects/TP/boards/73?quickFilter=434) |
| Tarko | `ME` | `ME-*` | [ME board 63](https://support-akeron.atlassian.net/jira/software/c/projects/ME/boards/63) |

### From issue key
- `TP-*` -> Vulki (project TP, board 73 with quick filter 434)
- `ME-*` -> Tarko (project ME, board 63)

### From product name
- "vulki" -> navigate to `https://support-akeron.atlassian.net/jira/software/c/projects/TP/boards/73?quickFilter=434`
- "tarko" -> navigate to `https://support-akeron.atlassian.net/jira/software/c/projects/ME/boards/63`

If I provide an issue key, derive the product from the prefix and select the corresponding Jira project.
If I provide a product name (vulki/tarko), resolve the Jira project key from the table above.
If both are present and they conflict, stop and report the ambiguity.
Do not mix Tarko and Vulki in the same run unless explicitly requested.

Source resolution order:
1. explicit product name in the user request -> mapped Jira project and exact board URL
2. issue key prefix -> mapped product
3. configured default source

Once the source is resolved:
- use only that source for board lookup, issue reading, and triage
- if product = Tarko -> use board 63: `https://support-akeron.atlassian.net/jira/software/c/projects/ME/boards/63`
- if product = Vulki -> use board 73 with quick filter 434: `https://support-akeron.atlassian.net/jira/software/c/projects/TP/boards/73?quickFilter=434`
- do not substitute another TP or ME board unless the user explicitly asks for a different board

1. `jira_intake` extracts the working record.
2. `completeness_check` returns exactly one verdict:
   - `MISSING_INFO`
   - `NEEDS_CUSTOMER_DB`
   - `READY_FOR_NORMALIZATION`
3. `MISSING_INFO` routes to `action_drafter(comment_to_reporter)` and ends as `WAITING_REPORTER`.
4. `NEEDS_CUSTOMER_DB` routes to `action_drafter(tech_db_request)` and ends as `WAITING_TECH_DB`.
5. `READY_FOR_NORMALIZATION` routes to `normalizer -> triage -> solution_planner -> prompt_generator -> critic`.

## Decision Precedence

Apply these checks in order:

1. Missing or unclear version, repro steps, or required logs means `MISSING_INFO`.
2. If prerequisites are complete but realistic local reproduction still depends on customer DB access, return `NEEDS_CUSTOMER_DB` unless Azure already has the DB available.
3. Only then return `READY_FOR_NORMALIZATION`.

## Critic Loop

- Critic failure does not create a new public business status.
- The router may rework the plan or prompt path within `agents.max_depth`.
- If the router cannot achieve a critic pass within the depth budget, fail the run instead of publishing a false `READY`.

## Summary Ordering

The run summary must include:

1. fetched Jira order
2. final status per bug
3. severity and readiness ranking
4. recommended execution order
5. blocked bugs and why

Recommended execution order should prioritize:

1. `READY`
2. higher severity within `READY`
3. Jira priority order as a tie-breaker
4. blocked bugs after actionable bugs

## Routing

Follow the Jira source routing rules in `docs/routing-rules.md`.
