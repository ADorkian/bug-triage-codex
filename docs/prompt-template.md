# Prompt Template

The generated Codex implementation prompt must be systematic and reusable.

## Required Sections

1. Issue context
2. Normalized bug summary
3. Constraints
4. Implementation goal
5. Files or areas to inspect first
6. Ordered execution steps
7. Test requirements
8. Quality gate
9. Expected output format
10. Explicit instruction not to guess missing facts

## Prompt Standards

- Use imperative language suitable for a follow-on Codex implementation run.
- Anchor the prompt in the solution plan, not in raw Jira text.
- Keep the prompt concrete enough that another engineer or agent can execute it repeatably.
- Preserve unknowns and assumptions as explicit unknowns and assumptions.
- Write the generated prompt in Italian by default. Use English only when the specific run prompt explicitly requests English.

## Canonical Shape

See `templates/codex-prompt.md`.
