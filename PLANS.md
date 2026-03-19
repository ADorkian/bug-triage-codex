# Plans

This document tracks the current rollout plan for the workshop-ready, Codex-native Jira triage scaffold.

## Phase 1 - Repository Initialization

Goal: establish the multi-agent workflow, templates, safety rules, and artifact structure for 8-bug kanban triage.

| Task | Status |
|---|---|
| Replace legacy pipeline with 9-agent state-based workflow | Done |
| Create project-scoped `.codex/config.toml` with MCP placeholders | Done |
| Create router, intake, completeness, drafting, normalization, triage, planning, prompt, and critic agent configs | Done |
| Add normalization, triage, severity, prompt, routing, and definition-of-done docs | Done |
| Add markdown templates for normalized issue, triage, solution plan, prompt, and operational drafts | Done |
| Align README, AGENTS, lifecycle, constraints, and output schema docs | Done |
| Create artifact directory skeleton for issues, triage, prompts, and actions | Done |

## Phase 2 - Environment Wiring

Goal: connect the scaffold to real Jira and optional support systems without changing the draft-first default.

| Task | Status |
|---|---|
| Configure Atlassian MCP server name and authentication | Pending |
| Set target kanban JQL or board configuration | Pending |
| Configure optional Azure availability check for customer DBs | Pending |
| Configure optional GitHub/repo and CI MCPs | Pending |
| Run first live 8-bug dry run against a safe Jira source | Pending |

## Phase 3 - Hardening

Goal: make repeated workshop/demo runs deterministic and easy to audit.

| Task | Status |
|---|---|
| Add regression checks for markdown artifact completeness | Pending |
| Add retry strategy for critic rework loop | Pending |
| Add archival/versioning strategy for repeated triage runs | Pending |
| Add smoke validation for MCP availability before a run starts | Pending |

## Phase 4 - Extensions

| Idea | Notes |
|---|---|
| Scheduled morning intake | Run the router against the configured kanban automatically |
| New-bug trigger | Kick off triage when a Jira bug enters a chosen board column |
| Ownership enrichment | Pull owner suggestions from repo metadata or CodeScene |
| Auto-post mode | Optional future step once write-enabled governance is accepted |
