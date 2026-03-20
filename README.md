# bug-triage-codex

`bug-triage-codex` is a reusable Codex project for Jira bug triage. It processes exactly the first 8 bugs in `To do` from a configured kanban source, checks whether each bug is actionable, drafts the right operational follow-up when it is not, and produces normalized markdown artifacts for every bug that is genuinely ready for engineering work.

## What The Project Does

- Reads the first 8 Jira bugs in `To do` from the target kanban in priority order
- Collects raw Jira evidence first, then verifies the required prerequisites: product version, reproducible steps, and logs when they are needed
- Drafts a Jira reporter comment when information is missing
- Drafts a tech issue for customer DB download when local reproduction depends on customer data that is not already available on Azure, asks for explicit confirmation before Jira creation, and then links the created TECH issue back to the source bug
- Normalizes complete issues, triages them, produces a concrete solution plan, and generates a reusable Codex implementation prompt
- Writes uniform markdown artifacts under `artifacts/`
- Writes result artifacts in Italian by default, unless a specific run prompt explicitly requests English

## Core Workflow

The router follows this state machine:

`START -> jira_intake -> completeness_check -> [WAITING_REPORTER | WAITING_TECH_DB | normalizer -> triage -> solution_planner -> prompt_generator -> critic -> READY]`

Operationally, the router first fetches the first 8 eligible bugs in Jira priority order, then fans out per-bug triage to parallel sub-agents, and finally merges all outcomes back into one summary while preserving the original fetch order.

See `AGENTS.md` and `docs/routing-rules.md` for the full routing contract.

## Running In Codex CLI

Interactive:

```powershell
codex -C .
```

Then paste the run prompt from `docs/prompts/run-bug-triage.md`.

Non-interactive:

```powershell
Get-Content -Raw .\docs\prompts\run-bug-triage.md | codex exec -C . --sandbox workspace-write -a on-request -
```

The bundled run prompt tells Codex to operate as the `router` agent defined in `.codex/config.toml` and `.codex/agents/router.toml`.

## MCP Configuration

Required:

- Atlassian MCP for Jira and optional Confluence context

Optional:

- GitHub or repository metadata MCP for code ownership or linked change context
- Azure MCP for checking whether the customer DB is already available
- SQL Server MCP for verifying in read-only mode whether the records needed for reproduction or E2E validation already exist
- CI/test MCP for existing build or regression signals

Read-only by default:

- Jira reads are expected
- Jira comments and Jira issue creation remain draft-only unless write mode is explicitly enabled
- Azure and CI checks are informational unless your MCP permissions allow broader operations

To switch safely to write-enabled mode:

1. Configure a write-capable Atlassian MCP server
2. Set `allow_external_writes = true` in `.codex/config.toml`
3. Set the write-enable environment guards referenced in `.codex/config.toml`
4. Keep the workflow draft-first and review generated markdown before any external write

## Output Layout

- `artifacts/issues/` stores normalized issues and solution plans
- `artifacts/triage/` stores per-bug triage results and the run summary
- `artifacts/prompts/` stores Codex implementation prompts
- `artifacts/actions/` stores Jira comment drafts and tech DB request drafts

The exact filenames and required sections are documented in `docs/output-schema.md`.

## Using The Router

Start every run from the `router` role. The router:

- selects exactly 8 bugs
- launches one parallel sub-agent per fetched bug, up to the configured concurrency limit
- preserves kanban priority order
- applies the prerequisite checks before any deeper analysis
- stops blocked bugs at the correct waiting state
- only emits `READY` after the critic approves the normalized issue, triage, plan, and implementation prompt

## Extending The System

Useful next steps for this scaffold:

- scheduled intake against a saved Jira filter or board
- webhook-triggered triage for newly created bugs
- team-specific owner routing or code ownership lookups
- automated freshness checks for existing `WAITING_*` drafts
