# Agents

This document describes every AI agent in the `bug-triage-codex` pipeline and how they collaborate to triage Jira bugs.

## Pipeline Overview

```
triage-router
    ├── jira-reader       (fetch & normalise the Jira issue)
    ├── repo-explorer     (gather relevant code context)
    ├── triage-analyst    (score severity, priority, component)
    ├── fix-planner       (produce a structured fix plan)
    ├── implementer       (draft code changes)
    └── validator         (verify the fix plan / diff is sound)
```

The **triage-router** is the entry point.  It decides which downstream agents to invoke and in what order based on the nature of the incoming issue.

---

## Agent Definitions

### triage-router
| Field | Value |
|---|---|
| Config | `.codex/agents/triage-router.toml` |
| Role | Orchestrator – parses the trigger, selects the appropriate sub-agents, and aggregates their outputs into a final triage artifact. |
| Inputs | Jira issue key (e.g. `AKERON-123`) |
| Outputs | Triage artifact written to `artifacts/triage/` |

### jira-reader
| Field | Value |
|---|---|
| Config | `.codex/agents/jira-reader.toml` |
| Role | Fetches the Jira issue via the Jira REST API, normalises fields, and returns a structured JSON representation. |
| Inputs | Jira issue key |
| Outputs | Normalised issue object |

### repo-explorer
| Field | Value |
|---|---|
| Config | `.codex/agents/repo-explorer.toml` |
| Role | Searches the target repository for files, symbols, and test cases related to the bug report.  Provides code snippets and file paths as context. |
| Inputs | Normalised issue object + repository root |
| Outputs | Code context bundle |

### triage-analyst
| Field | Value |
|---|---|
| Config | `.codex/agents/triage-analyst.toml` |
| Role | Applies the triage rubric (`docs/triage-rubric.md`) to assign severity, priority, affected component, and estimated effort. |
| Inputs | Normalised issue object + code context bundle |
| Outputs | Triage scores and classification |

### fix-planner
| Field | Value |
|---|---|
| Config | `.codex/agents/fix-planner.toml` |
| Role | Produces a step-by-step fix plan describing what needs to change, in which files, and why.  Written to `artifacts/fix-plans/`. |
| Inputs | Triage scores + code context bundle |
| Outputs | Fix plan document |

### implementer
| Field | Value |
|---|---|
| Config | `.codex/agents/implementer.toml` |
| Role | Drafts code changes (unified diff or file contents) based on the fix plan.  Does **not** push changes — it produces a diff for human review. |
| Inputs | Fix plan + code context bundle |
| Outputs | Proposed diff / code changes |

### validator
| Field | Value |
|---|---|
| Config | `.codex/agents/validator.toml` |
| Role | Reviews the fix plan and proposed diff for correctness, completeness, and conformance to project constraints (`docs/constraints.md`).  Writes a validation report to `artifacts/validation/`. |
| Inputs | Fix plan + proposed diff + code context bundle |
| Outputs | Validation report |

---

## Adding a New Agent

1. Create `.codex/agents/<name>.toml` following the schema of an existing agent file.
2. Register the agent in `.codex/config.toml` under `[agents]`.
3. Update `triage-router.toml` to route to the new agent where appropriate.
4. Document the agent in this file.
