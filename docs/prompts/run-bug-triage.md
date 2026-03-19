# Run Bug Triage

Use this prompt to trigger the full bug-triage pipeline on a single Jira issue.

---

## Prerequisites

Before running, ensure the following environment variables are set:

| Variable | Description |
|---|---|
| `JIRA_BASE_URL` | Base URL of the Jira instance, e.g. `https://akeron.atlassian.net` |
| `JIRA_API_TOKEN` | Jira API token (generated in Atlassian account settings) |
| `JIRA_USER_EMAIL` | Email address associated with the API token |

---

## Prompt

Copy and paste the following into a Codex session, replacing `{ISSUE_KEY}` with the real Jira issue key (e.g. `AKERON-456`):

```
Triage the Jira issue {ISSUE_KEY}.

Follow the pipeline defined in .codex/config.toml:

1. Use the jira-reader agent to fetch and normalise the issue.
2. Use the repo-explorer agent to gather relevant code context.
3. Use the triage-analyst agent to classify severity, priority, affected
   component, estimated effort, and root-cause hypothesis. Apply the rubric in
   docs/triage-rubric.md.
4. If severity is medium or above:
   a. Use the fix-planner agent to produce a step-by-step fix plan.
   b. Use the implementer agent to draft a proposed diff.
   c. Use the validator agent to validate the plan and diff.
5. Write all artifacts to the correct locations as defined in
   docs/output-schema.md.
6. Write a human-readable triage summary to
   artifacts/triage/{ISSUE_KEY}-summary.md using the template at
   templates/triage-summary.md.

Respect all constraints in docs/constraints.md throughout.
```

---

## Expected Outputs

After a successful run you should find the following files:

| File | Description |
|---|---|
| `artifacts/triage/{ISSUE_KEY}.json` | Complete triage artifact |
| `artifacts/triage/{ISSUE_KEY}-summary.md` | Human-readable summary |
| `artifacts/fix-plans/{ISSUE_KEY}-fix-plan.json` | Fix plan (if severity ≥ medium) |
| `artifacts/validation/{ISSUE_KEY}-validation.json` | Validation report (if severity ≥ medium) |

---

## Tips

- If the pipeline fails at the `jira-reader` step, verify that all three Jira environment variables are set correctly and that the issue key exists.
- If `triage-analyst` returns `confidence: low`, consider adding more detail to the Jira issue (stack trace, reproduction steps, affected version) before re-running.
- Artifacts are overwritten on re-run.  Previous versions are retained with a timestamp suffix, e.g. `AKERON-456-20250115T103000Z.json`.
