---
name: jira-tech-db-request-issue
description: Produce il contenuto di una issue TECH Jira per richiedere disponibilita' o download del database cliente necessario a riprodurre un bug. Usare quando il bug e' completo ma la riproduzione locale dipende da un customer DB non disponibile su Azure e serve preparare una richiesta tecnica draft-first senza inventare dettagli.
---

# Jira Tech DB Request Issue

Produci una bozza markdown di issue TECH Jira per richiedere il database cliente necessario al triage di un bug.

## Workflow

1. Leggi i dati del bug di origine e il motivo per cui il DB cliente e' necessario.
2. Riusa solo evidenze presenti nel working record e nella decisione di completezza.
3. Organizza la richiesta in modo operativo per un team tecnico interno.
4. Produci solo il markdown della bozza issue TECH.

## Output richiesto

Usa questa struttura:

```md
# Bozza Richiesta DB Tecnico: {ISSUE_KEY}

## Sintesi Della Richiesta

Richiesta DB cliente per {ISSUE_KEY} - {TITLE}

## Bug Di Origine

- Bug Jira: {ISSUE_KEY}
- Reporter: {REPORTER}
- Priorita: {PRIORITY}

## Perche Sono Necessari I Dati Del Cliente

{DB_REASON}

## Verifica Disponibilita Su Azure

{AZURE_STATUS}

## Azione Richiesta

Confermare se il DB cliente e gia disponibile su Azure. Se non e disponibile, predisporre il download del DB o un accesso equivalente necessario per riprodurre e analizzare questo bug in sicurezza.

## Evidenze

{EVIDENCE_SUMMARY}
```

## Regole

- Non inventare progetto TECH, assignee, priority interna o altri metadati Jira non ricevuti in input.
- Mantieni invariati issue key, titoli, nomi file, riferimenti tecnici e identificativi.
- Se un dettaglio non e' disponibile, scrivilo in forma esplicita e neutra.
- La richiesta deve essere utilizzabile come base per creare una issue Jira interna dopo conferma umana.

## Vincoli

- Non creare direttamente la issue Jira.
- Non aggiungere testo prima o dopo il markdown richiesto.
- Non chiedere conferma nel corpo della issue: la conferma utente e' gestita fuori dalla skill.
