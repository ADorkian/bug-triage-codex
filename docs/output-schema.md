# Output Schema

All artifacts in this repository are markdown. Filenames are deterministic and keyed by Jira issue key. Artifact prose is written in Italian by default, unless a specific run prompt explicitly requests English.

## Per-Bug Artifacts

| File | Required When | Purpose |
|---|---|---|
| `artifacts/issues/{ISSUE_KEY}-normalized.md` | Final status is `READY` | Canonical normalized bug record |
| `artifacts/triage/{ISSUE_KEY}-triage.md` | Final status is `READY` | Structured triage decision |
| `artifacts/issues/{ISSUE_KEY}-solution-plan.md` | Final status is `READY` | Concrete technical resolution plan |
| `artifacts/prompts/{ISSUE_KEY}-codex-prompt.md` | Final status is `READY` | Reusable implementation prompt for a follow-on Codex run |
| `artifacts/actions/{ISSUE_KEY}-jira-comment.md` | Final status is `WAITING_REPORTER` | Draft Jira comment asking only for missing information |
| `artifacts/actions/{ISSUE_KEY}-tech-db-request.md` | Final status is `WAITING_TECH_DB` | Draft technical request for customer DB availability or download, plus created TECH issue reference when confirmation/write-back succeeded |

## Run-Level Artifact

| File | Required | Purpose |
|---|---|---|
| `artifacts/triage/triage-summary.md` | Every run | Summary of the 8 processed bugs, statuses, severity/readiness ranking, execution order, and blockers |

## Minimum Required Headers

The following level-1 markdown headers are mandatory:

| Artifact | Required H1 |
|---|---|
| Normalized issue | `# {ISSUE_KEY} Issue Normalizzata` |
| Triage | `# {ISSUE_KEY} Triage` |
| Solution plan | `# {ISSUE_KEY} Piano Di Soluzione` |
| Codex prompt | `# Prompt Di Implementazione Codex: {ISSUE_KEY}` |
| Jira comment draft | `# Bozza Commento Jira: {ISSUE_KEY}` |
| Tech DB request draft | `# Bozza Richiesta DB Tecnico: {ISSUE_KEY}` |
| Run summary | `# Sintesi Triage Bug Jira` |

## Section Contracts

- Normalized issue files must match `docs/normalization-template.md` and `templates/normalized-issue.md`.
- Triage files must match `docs/triage-template.md` and `templates/triage-item.md`.
- Solution plans must match `templates/solution-plan.md` and include the definition of done from `docs/definition-of-done.md`.
- Codex prompts must match `docs/prompt-template.md` and `templates/codex-prompt.md`.
- Action drafts must use the matching file under `templates/`.
- `WAITING_TECH_DB` artifacts should also record whether user confirmation is still pending, the proposed Jira TECH payload, and any created Jira key/URL plus the triage-link outcome when write-back occurred.

## Summary Requirements

`artifacts/triage/triage-summary.md` must include:

1. The 8 Jira bug keys in the order fetched from Jira
2. The final status of each bug
3. A severity and readiness ranking
4. The recommended execution order
5. Blocked bugs and the reason each bug is blocked
