# Severity Rubric

Use this rubric for the `Severity` field in the triage artifact.

| Severity | Meaning | Typical Signals |
|---|---|---|
| `Sev-1 Critical` | Work cannot continue, production is materially down, or there is serious data loss or security exposure. | System outage, unrecoverable corruption, blocked core revenue path, severe regulatory or security risk. |
| `Sev-2 High` | Major user capability is broken with no acceptable workaround. | Frequent production failures, broken key workflow, widespread customer impact, urgent support burden. |
| `Sev-3 Medium` | Important defect with a workaround or limited blast radius. | Single module failure, narrow customer segment, manageable but real operational pain. |
| `Sev-4 Low` | Minor defect, cosmetic issue, or low-impact behavior mismatch. | Edge-case UI defect, wording issue, small annoyance, non-critical workflow friction. |

## Triage Notes

- Severity is not the same as Jira priority. Use business impact first, then breadth, then recoverability.
- If evidence is weak, prefer a lower-confidence triage rather than inflating severity.
- The run summary should still recommend execution order across READY bugs by readiness first and severity second.
