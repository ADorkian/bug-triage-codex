---
name: jira-normalized-issue
description: Produce a normalized Jira issue in markdown from collected issue evidence. Use when a Jira bug already has sufficient prerequisites and Codex must rewrite the raw Jira content into a stable, reviewable normalized issue that will be consumed by another agent for downstream analysis.
---

# Jira Normalized Issue

Produci una issue markdown normalizzata usando solo evidenze presenti in issue Jira, commenti, allegati o contesto collegato.

## Workflow

1. Raccogli i dati gia estratti dall'issue.
2. Non inventare campi mancanti.
3. Produci solo markdown della issue normalizzata.
4. Mantieni i nomi dei campi esattamente come richiesto.
5. Usa `null`, `[]` oppure `Non specificato` solo se coerente con il tipo e con il contesto ricevuto.

## Output richiesto

Restituisci la issue normalizzata in questo shape:

```md
- issue_key: ...
- title: ...
- description: ...
- priority: ...
- reporter: ...
- assignee: ...
- product_area: ...
- product_version: ...
- steps_to_reproduce:
  - ...
- expected_behavior: ...
- actual_behavior: ...
- logs_and_attachments:
  - ...
- dependencies:
  - ...
- comment_clarifications:
  - ...
- linked_context:
  - ...
- notes:
  - ...
```

## Regole

- Non copiare interi blocchi Jira se basta una sintesi fedele.
- Se un'informazione proviene dai commenti o dal linked context, conservala in forma concisa ma verificabile.
- Se `steps_to_reproduce`, `logs_and_attachments`, `dependencies`, `comment_clarifications`, `linked_context` o `notes` hanno piu elementi, usa una lista markdown.
- Se un valore non e disponibile, scrivi `Non specificato` per i campi testuali e `[]` per gli elenchi, senza dedurre fatti.
- Mantieni invariati identificativi tecnici, nomi file, stack trace, key Jira e path.
- Organizza il testo per essere facilmente consumabile da un altro agente.

## Vincoli

- Non aggiungere analisi di triage.
- Non fare raccomandazioni.
- Non chiedere ulteriori informazioni.
- Non scrivere un commento rivolto a persone.
- Non aggiungere testo prima o dopo il blocco markdown richiesto.
