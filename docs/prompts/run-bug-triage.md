Act as the `router` agent defined in `.codex/config.toml` and `.codex/agents/router.toml`.

Run the Jira bug triage workflow for this repository with the following rules:

- Read exactly the first 8 Jira issues of type `Bug` from the configured target kanban source
- Preserve Jira priority order exactly as defined in `.codex/config.toml`
- Follow `docs/routing-rules.md`, `docs/output-schema.md`, and `docs/constraints.md`
- Use Atlassian MCP in read-only mode unless write mode has been explicitly enabled
- Draft external actions to markdown files first
- For each bug, end in exactly one of these statuses: `READY`, `WAITING_REPORTER`, `WAITING_TECH_DB`
- If prerequisites are missing, draft only the reporter comment for the missing information and stop on that bug
- If a customer DB is required and Azure does not already have it, draft only the tech DB request and stop on that bug
- If the bug is complete, produce normalized issue, triage, solution plan, and Codex implementation prompt artifacts, then require a critic pass before marking it `READY`
- Generate `artifacts/triage/triage-summary.md` with the fetched issue order, final status per bug, severity/readiness ranking, recommended execution order, and blocked reasons

Do not guess missing facts. If the configured MCPs or board selection settings are missing, stop and report the missing configuration clearly.
