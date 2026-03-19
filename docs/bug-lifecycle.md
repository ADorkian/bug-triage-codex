# Bug Lifecycle

This project uses a small set of business-visible states and an internal readiness loop.

## State Diagram

```
START
  -> jira_intake
  -> completeness_check

  -> MISSING_INFO       -> action_drafter(comment_to_reporter) -> WAITING_REPORTER
  -> NEEDS_CUSTOMER_DB  -> action_drafter(tech_db_request -> user_confirmation -> jira_create_and_link_if_allowed) -> WAITING_TECH_DB
  -> READY_FOR_NORMALIZATION
       -> normalizer
       -> triage
       -> solution_planner
       -> prompt_generator
       -> critic
       -> READY
```

## Business States

| State | Meaning |
|---|---|
| `WAITING_REPORTER` | The issue is blocked on missing or unclear version, repro steps, or logs. |
| `WAITING_TECH_DB` | The issue is blocked on customer DB availability because realistic reproduction is not feasible locally. A TECH DB request draft always exists; the Jira TECH issue is created and linked only after explicit user confirmation and with write permissions enabled. |
| `READY` | The issue is actionable and all required artifacts exist with a passing critic verdict. |

## Internal States

| State | Meaning |
|---|---|
| `READY_FOR_NORMALIZATION` | `completeness_check` confirmed the bug is complete enough for internal normalization. |
| `critic rework` | Internal only. If the critic finds weak reasoning, the router may re-run planner and prompt generation within the configured depth budget. |

## Re-Runs

The router is intended to be rerun against the same board or filter. Because filenames are deterministic, the latest run overwrites the prior logical output unless your team adds an archival layer.
