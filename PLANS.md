# Plans

This document tracks the implementation roadmap for `bug-triage-codex`.

---

## Phase 1 – Foundation (current)

**Goal:** Establish the repository structure, agent definitions, documentation, and templates so the pipeline can be exercised end-to-end on a single Jira issue.

| Task | Status |
|---|---|
| Repository skeleton (dirs, placeholder files) | ✅ Done |
| `.codex/config.toml` | ✅ Done |
| Agent TOML definitions (all 7 agents) | ✅ Done |
| Triage rubric (`docs/triage-rubric.md`) | ✅ Done |
| Output schema (`docs/output-schema.md`) | ✅ Done |
| Bug lifecycle doc (`docs/bug-lifecycle.md`) | ✅ Done |
| Constraints doc (`docs/constraints.md`) | ✅ Done |
| Run prompt (`docs/prompts/run-bug-triage.md`) | ✅ Done |
| JSON template (`templates/triage-item.json`) | ✅ Done |
| Markdown template (`templates/triage-summary.md`) | ✅ Done |

---

## Phase 2 – Integration

**Goal:** Wire up Jira API authentication and validate the pipeline against real issues.

| Task | Status |
|---|---|
| Jira OAuth / PAT credential handling | ⬜ Pending |
| End-to-end test against a sandbox Jira project | ⬜ Pending |
| CI workflow to run triage on new Jira webhook events | ⬜ Pending |

---

## Phase 3 – Hardening

**Goal:** Make the system production-ready.

| Task | Status |
|---|---|
| Rate-limit handling for Jira API calls | ⬜ Pending |
| Retry / fallback logic in triage-router | ⬜ Pending |
| Metrics and observability hooks | ⬜ Pending |
| Human-in-the-loop review step before implementer runs | ⬜ Pending |
| Automated tests for each agent in isolation | ⬜ Pending |

---

## Phase 4 – Enhancements

| Idea | Notes |
|---|---|
| Slack / Teams notifications for high-severity bugs | Notify on-call team immediately |
| Historical trend analysis | Use past triage artifacts to refine severity scoring |
| Auto-label Jira issues after triage | Write labels back via Jira API |
| Support GitHub Issues as an additional source | Extend jira-reader to a generic issue-reader |
