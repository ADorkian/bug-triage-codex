# Output Schema

This document defines the canonical JSON schemas for every artifact produced by the `bug-triage-codex` pipeline.

All artifacts are written to the `artifacts/` directory tree.  File naming follows the pattern `{ISSUE_KEY}-{artifact-type}.json`.

---

## 1. Triage Artifact (`artifacts/triage/{ISSUE_KEY}.json`)

Produced by `triage-router` after all sub-agents have completed.

```jsonc
{
  // Metadata
  "schema_version": "1.0",
  "issue_key":      "AKERON-123",
  "triaged_at":     "2025-01-15T10:30:00Z",
  "pipeline_run_id":"<UUID>",

  // Raw issue data (from jira-reader)
  "issue": {
    "summary":     "<string>",
    "description": "<string>",
    "issue_type":  "<Bug|Task|Story|...>",
    "priority":    "<Blocker|Critical|Major|Minor|Trivial>",
    "status":      "<string>",
    "reporter":    "<string>",
    "assignee":    "<string | null>",
    "components":  ["<string>"],
    "labels":      ["<string>"],
    "created_at":  "<ISO-8601>",
    "updated_at":  "<ISO-8601>"
  },

  // Triage classification (from triage-analyst)
  "triage": {
    "severity":              "<critical|high|medium|low|trivial>",
    "priority":              "<P0|P1|P2|P3|P4>",
    "affected_component":    "<string>",
    "estimated_effort":      "<XS|S|M|L|XL>",
    "root_cause_hypothesis": "<string>",
    "confidence":            "<high|medium|low>",
    "reasoning":             "<string>"
  },

  // Code context summary (from repo-explorer)
  "code_context": {
    "relevant_files":  [{"path": "<string>", "reason": "<string>"}],
    "relevant_tests":  [{"path": "<string>", "reason": "<string>"}],
    "recent_commits":  [{"sha": "<string>", "message": "<string>"}]
  },

  // Validation result (from validator, omitted if not run)
  "validation": {
    "verdict":         "<pass|pass_with_warnings|fail>",
    "recommendation":  "<approve|revise|reject>",
    "blocking_issues": ["<string>"],
    "warnings":        ["<string>"]
  }
}
```

---

## 2. Fix Plan (`artifacts/fix-plans/{ISSUE_KEY}-fix-plan.json`)

Produced by `fix-planner`.

```jsonc
{
  "schema_version": "1.0",
  "issue_key":      "AKERON-123",
  "plan_id":        "<UUID>",
  "created_at":     "<ISO-8601>",
  "summary":        "<one-sentence description of the fix>",
  "steps": [
    {
      "step":        1,
      "file":        "<repo-relative path>",
      "action":      "<modify|create|delete|rename>",
      "description": "<what to change and why>",
      "test_impact": "<describe tests to add or update, or 'none'>"
    }
  ],
  "risks":            ["<string>"],
  "alternatives":     ["<string>"],
  "estimated_effort": "<XS|S|M|L|XL>"
}
```

---

## 3. Validation Report (`artifacts/validation/{ISSUE_KEY}-validation.json`)

Produced by `validator`.

```jsonc
{
  "schema_version": "1.0",
  "issue_key":      "AKERON-123",
  "plan_id":        "<UUID>",
  "validated_at":   "<ISO-8601>",
  "verdict":        "<pass|pass_with_warnings|fail>",
  "checks": [
    {
      "name":    "<check name>",
      "result":  "<pass|warn|fail>",
      "details": "<string>"
    }
  ],
  "blocking_issues": ["<string>"],
  "warnings":        ["<string>"],
  "recommendation":  "<approve|revise|reject>"
}
```

---

## 4. Enumerated Values

### severity
`critical` | `high` | `medium` | `low` | `trivial`

### priority
`P0` | `P1` | `P2` | `P3` | `P4`

### estimated_effort
`XS` | `S` | `M` | `L` | `XL`

### confidence
`high` | `medium` | `low`

### verdict
`pass` | `pass_with_warnings` | `fail`

### recommendation
`approve` | `revise` | `reject`

### action (fix plan step)
`modify` | `create` | `delete` | `rename`
