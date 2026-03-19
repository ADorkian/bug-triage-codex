---
name: jira-reporter-mention-comment
description: Draft a Jira comment that tags the reporter and asks only for the missing prerequisites needed to continue bug triage. Use when a Jira bug lacks explicit product version, reproducible steps, required logs, or other mandatory evidence and Codex must prepare a concise reporter-facing request without inventing facts or asking for data already present.
---

# Jira Reporter Mention Comment

Scrivi un commento Jira conciso in italiano che tagga il reporter e chiede solo i requisiti mancanti per proseguire con il triage.

## Workflow

1. Identifica il reporter da taggare.
2. Leggi l'elenco dei requisiti mancanti.
3. Formula una richiesta chiara, cortese e operativa.
4. Chiedi solo ciò che manca davvero.
5. Produci solo il testo del commento.

## Output richiesto

Usa questa struttura:

```md
@reporter

Grazie. Per completare il triage del bug ci servono ancora queste informazioni:

- requisito mancante 1
- requisito mancante 2

Quando avremo questi dettagli potremo procedere con l'analisi tecnica.
```

## Regole

- Usa il tag Jira nel formato `@{reporter}` o nel formato già fornito in input.
- Se il reporter non è disponibile, ometti il tag e mantieni il messaggio impersonale.
- Se manca un solo requisito, chiedi solo quello.
- Se i requisiti mancanti sono più di uno, usa una lista puntata.
- Non attribuire colpe e non inserire assunzioni tecniche non provate.

## Vincoli

- Non chiedere informazioni già presenti nella issue.
- Non includere dettagli di triage interno o valutazioni di severità.
- Non aggiungere introduzioni o chiusure extra oltre al commento richiesto.
