# Triage Template

The triage artifact translates the normalized issue into an engineering decision record.

## Required Fields

1. Severity
2. Customer/business impact
3. Reproducibility
4. Confidence
5. Suspected subsystem
6. Probable cause
7. Missing information
8. Blockers
9. Recommended owner
10. Implementation readiness

## Guidance

- Severity must follow `docs/severity-rubric.md`.
- Reproducibility should be one of `Always`, `Intermittent`, `Unknown`, or a clearly equivalent term.
- Confidence should be `High`, `Medium`, or `Low`.
- Missing information should usually be `None` for a `READY` bug; if not, the critic should challenge the output.
- Implementation readiness should state whether the bug is ready for coding now and why.

## Canonical Shape

See `templates/triage-item.md`.
