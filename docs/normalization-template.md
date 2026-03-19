# Normalization Template

Use this template whenever `completeness_check` returns `READY_FOR_NORMALIZATION`.

## Purpose

The normalized issue is the canonical internal bug record. It turns raw Jira content into one clear markdown artifact without losing source evidence.

## Fill Rules

- Use only evidence from Jira fields, comments, attachments metadata, and linked context.
- Prefer concise internal wording over copy-pasting raw Jira text.
- If a field is missing, write `Unknown` or `Not provided`.
- Repro steps can be assembled from comments only when the sequence is explicit.
- For logs and attachments, record what exists and why it matters. Do not infer file contents.

## Required Sections

1. Issue key
2. Title
3. Product area
4. Product version
5. Business context
6. Steps to reproduce
7. Expected behavior
8. Actual behavior
9. Comments-derived clarifications
10. Logs and attachments
11. Dependencies
12. Current completeness verdict

## Canonical Shape

See `templates/normalized-issue.md`.
