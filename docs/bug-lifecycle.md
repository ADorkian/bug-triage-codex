# Bug Lifecycle

This document describes the end-to-end lifecycle of a bug as it moves through the `bug-triage-codex` pipeline.

---

## State Diagram

```
REPORTED
   │
   ▼
INGESTED ──────────────────────────────────────┐
   │  (jira-reader fetches & normalises)        │
   ▼                                            │
CONTEXT_GATHERED                               │
   │  (repo-explorer finds relevant code)       │
   ▼                                            │
TRIAGED                                        │
   │  (triage-analyst scores severity etc.)     │
   ▼                                            │
   ├─[severity < medium]──────────────────► TRIAGED_LOW_PRIORITY
   │                                          (artifact written; no fix plan)
   │
   ▼
PLANNED
   │  (fix-planner produces fix plan)
   ▼
DRAFTED
   │  (implementer produces proposed diff)
   ▼
VALIDATED
   │  (validator checks plan + diff)
   │
   ├─[verdict=fail]──────────────────────► NEEDS_REVISION
   │                                         (triage-router notifies team)
   │
   ▼
READY_FOR_REVIEW
   │  (human engineer reviews artifacts)
   ▼
   ├─[approved]──────────────────────────► IMPLEMENTATION_IN_PROGRESS
   │                                         (engineer applies diff or creates PR)
   └─[rejected]──────────────────────────► CLOSED_WONT_FIX
```

---

## State Descriptions

| State | Description |
|---|---|
| **REPORTED** | Bug exists in Jira but has not yet entered the pipeline. |
| **INGESTED** | `jira-reader` has fetched and normalised the issue. |
| **CONTEXT_GATHERED** | `repo-explorer` has identified relevant files and tests. |
| **TRIAGED** | `triage-analyst` has produced a severity/priority classification. |
| **TRIAGED_LOW_PRIORITY** | Terminal state for low/trivial bugs — no automated fix plan is generated; issue is placed in backlog. |
| **PLANNED** | `fix-planner` has produced a step-by-step fix plan. |
| **DRAFTED** | `implementer` has produced a proposed diff. |
| **VALIDATED** | `validator` has reviewed the plan and diff. |
| **NEEDS_REVISION** | Validation failed — the pipeline notifies the team for manual intervention. |
| **READY_FOR_REVIEW** | All pipeline stages passed — artifacts are ready for a human engineer. |
| **IMPLEMENTATION_IN_PROGRESS** | An engineer is applying the proposed fix. |
| **CLOSED_WONT_FIX** | Issue closed without a fix (out of scope, not reproducible, etc.). |

---

## Artifacts Produced Per Lifecycle Step

| Step | Agent | Artifact written |
|---|---|---|
| INGESTED → CONTEXT_GATHERED | `jira-reader` | *(in-memory, no file)* |
| CONTEXT_GATHERED → TRIAGED | `repo-explorer` | *(in-memory, no file)* |
| TRIAGED | `triage-analyst` | `artifacts/triage/{KEY}.json` (partial) |
| PLANNED | `fix-planner` | `artifacts/fix-plans/{KEY}-fix-plan.json` |
| VALIDATED | `validator` | `artifacts/validation/{KEY}-validation.json` |
| READY_FOR_REVIEW | `triage-router` | `artifacts/triage/{KEY}.json` (complete) + `artifacts/triage/{KEY}-summary.md` |

---

## Re-Triage

A bug may be re-triaged at any point by running the pipeline again with the same issue key.  The pipeline overwrites existing artifacts (keeping the previous version under a timestamped filename for auditing).
