# Step Plan Guidance

Use this reference when converting a normalized bug into Playwright MCP actions.

## Minimum Required Data

Do not attempt reproduction unless you have:

- target URL
- credentials
- a navigable UI path
- an expected visible outcome to confirm or disprove

## Step Sources

Build steps only from:

- normalized issue
- triage artifact
- Jira comments with explicit UI actions
- direct user instructions

Do not infer missing clicks, menu names, or hidden navigation.

## Recommended Action Shape

Represent the intended flow in your notes before driving MCP:

```json
{
  "startPath": "/",
  "steps": [
    "open interventi page",
    "search IMA_BOS_25/00000005",
    "open preview PDF",
    "observe error message"
  ],
  "successSignal": "Failed to generate report for intervento IMA_BOS_25/00000005."
}
```

This is a planning artifact only. Execute through Playwright MCP commands, not through a local runner.

## Outcome Rules

- `reproduced`: the expected faulty signal appears
- `not_reproduced`: the flow completes and the faulty signal does not appear
- `login_failed`: authentication cannot complete
- `steps_blocked`: the browser opens but a required UI step cannot be completed
- `inconclusive`: evidence is insufficient to decide
