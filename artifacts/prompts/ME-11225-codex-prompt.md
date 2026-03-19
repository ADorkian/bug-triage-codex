# Prompt Di Implementazione Codex: ME-11225

## Contesto Del Problema

Analizza il bug `ME-11225` relativo alla mancata generazione del PDF Rapporto Intervento nel flusso Technical Services del cliente Bosticco. Il problema e' stato riprodotto su ambiente `Bosticco TEST` anche dopo aggiornamenti di versione e dopo un tentativo di configurazione con `PublicUrlBase`.

## Sintesi Normalizzata Del Bug

- L'endpoint `https://bosticco-me-test.akeroncloud.com/WEBAPI-MANUTENTORI/api/maintenanceApp/report/interventi?id=IMA_BOS_25/00000005` fallisce.
- L'errore applicativo e' `Failed to generate report for intervento IMA_BOS_25/00000005`.
- La catena di eccezioni arriva a un rifiuto di connessione verso `127.0.0.1:7000`.
- I moduli citati nello stack trace sono `InterventiJasperReportGenerator`, `ReportController`, `JasperPdfService` e `JasperReportGeneratorService`.

## Vincoli

- Non inventare configurazioni ambiente mancanti.
- Mantieni il fix il piu' piccolo e difendibile possibile.
- Non alterare altri flussi di reportistica se non emerge una dipendenza reale.
- Se scopri che il problema dipende solo da configurazione deploy e non da codice, fermati e documenta con precisione il punto esatto.

## Obiettivo Di Implementazione

Correggere il flusso di generazione PDF del rapporto intervento in modo che il servizio non usi un endpoint Jasper non valido o un fallback locale scorretto nell'ambiente target.

## File O Aree Da Ispezionare Per Prime

- `Tarko.Shared/Services/RT/InterventiJasperReportGenerator.cs`
- `WebApiME/MaintenanceAppApi/Controllers/ReportController.cs`
- `Tarko.Report.Jasper/Services/External/JasperPdfService.cs`
- `Tarko.Report.Jasper/Services/JasperReportGeneratorService.cs`
- configurazioni/app settings relative a host, porta o base URL del servizio Jasper

## Passi Di Esecuzione Ordinati

1. Ricostruisci il path dal controller `report/interventi` fino alla chiamata HTTP verso Jasper.
2. Identifica dove viene risolto l'host/porta del servizio e perche' la chiamata finisce su `127.0.0.1:7000`.
3. Verifica se esiste un fallback implicito o una configurazione non letta correttamente nel flusso report intervento.
4. Implementa il fix minimo: usare la configurazione corretta o rimuovere il fallback non valido, mantenendo compatibilita con gli ambienti che usano davvero un endpoint locale.
5. Aggiungi una validazione o logging tecnico utile quando la configurazione Jasper e' assente o inconsistente.
6. Verifica che il fix copra sia anteprima APP sia link di scarico, se condividono lo stesso servizio.
7. Aggiungi o aggiorna test mirati sul punto di risoluzione endpoint o sul servizio che effettua la chiamata.

## Requisiti Di Test

- Testare il path con configurazione valida e configurazione mancante.
- Eseguire almeno uno smoke test del flusso `report/interventi` con endpoint Jasper controllato o mockato.
- Documentare se la validazione finale sul caso `IMA_BOS_25/00000005` richiede ambiente reale.

## Criterio Di Qualita

Il lavoro e' accettabile solo se:

- viene identificata con precisione la causa del target `127.0.0.1:7000`
- il fix non amplia inutilmente la superficie della modifica
- i test o le verifiche coprono il path interessato
- eventuali dipendenze di configurazione restano esplicite nella consegna finale

## Formato Di Output Atteso

Fornisci:

- breve root cause confermata o ipotesi finale piu' solida
- elenco dei file modificati
- sintesi del fix applicato
- test eseguiti o non eseguiti
- eventuali rischi residui o controlli da fare in ambiente

## Regola Di Non Inferenza

Non inventare fatti mancanti. Se un'evidenza manca, dichiara esplicitamente il gap e fermati.
