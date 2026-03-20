# Definition Of Done

A bug can only be considered fully ready for implementation when all of the following are true.

## READY Bug Done Criteria

- The issue passed `completeness_check` with `READY_FOR_NORMALIZATION`
- `artifacts/issues/{ISSUE_KEY}-normalized.md` exists
- `artifacts/triage/{ISSUE_KEY}-triage.md` exists
- `artifacts/issues/{ISSUE_KEY}-solution-plan.md` exists
- `artifacts/prompts/{ISSUE_KEY}-codex-prompt.md` exists
- The solution plan names likely files or modules to inspect first
- The solution plan contains a smallest defensible fix, edge cases, E2E data readiness, test strategy, rollback strategy, assumptions, and explicit definition of done
- E2E data readiness explicitly states one of: required data already present, verification not possible from the available context/MCPs, or minimum data setup required to execute an E2E validation
- The prompt matches the plan and does not guess missing facts
- The critic passes with no blocking findings

## Waiting-State Done Criteria

- `WAITING_REPORTER` bugs have a single focused Jira comment draft that asks only for missing information
- `WAITING_TECH_DB` bugs have a DB request draft that explains why customer data is required and whether Azure availability was checked
