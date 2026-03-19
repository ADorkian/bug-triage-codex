# ME-11225 Issue Normalizzata

## Metadati

- Chiave issue: ME-11225
- Titolo: Bosticco - Mancata produzione PDF Rapporto Intervento su Anteprima e su link di scarico (mail da Destinatari)
- Area prodotto: Technical Services / Maintenance App / Reportistica Jasper
- Versione prodotto: Riprodotto ancora su `ME 25.3.8 Service Pack`; gia' segnalato su `ME 25.2.10 Service Pack` e verificato ancora dopo aggiornamento a `ME 25.3.4`
- Priorita Jira: Medium
- Reporter: Mirco Bracaloni

## Contesto Di Business

Nel flusso Technical Services il cliente Bosticco non riesce a visualizzare o scaricare il PDF del Rapporto Intervento dall'anteprima APP e dal link di scarico collegato alle mail dei destinatari. Il problema rimane aperto nonostante aggiornamenti di versione e un tentativo di correzione via configurazione ambiente.

## Passi Per Riprodurre

1. Usare l'ambiente `Bosticco TEST` aggiornato almeno a `ME 25.3.8 Service Pack`.
2. Tentare la generazione dell'anteprima PDF del rapporto intervento dall'APP oppure invocare direttamente l'endpoint:
   `https://bosticco-me-test.akeroncloud.com/WEBAPI-MANUTENTORI/api/maintenanceApp/report/interventi?id=IMA_BOS_25/00000005`
3. Verificare l'esito della chiamata di report Jasper per l'intervento `IMA_BOS_25/00000005`.

## Comportamento Atteso

Il sistema deve generare e restituire correttamente il PDF del Rapporto Intervento, sia in anteprima APP sia dal link di scarico associato alla mail.

## Comportamento Attuale

La chiamata fallisce con `System.InvalidOperationException` e messaggio `Failed to generate report for intervento IMA_BOS_25/00000005.`.
Dalla catena di eccezioni emerge una `HttpRequestException`, poi una `WebException`, fino a una `SocketException` con rifiuto di connessione verso `127.0.0.1:7000`.

## Chiarimenti Derivati Dai Commenti

- Il problema era gia' stato osservato dopo aggiornamento a `ME 25.3.4`.
- E' stato proposto di configurare `PublicUrlBase=https://bosticco-me-test.akeroncloud.com/BTOWEBNET/`.
- Il reporter ha confermato che il problema persiste anche dopo l'aggiunta di `PublicUrlBase`.
- Il ticket e' stato riaperto dopo aggiornamento a `ME 25.3.8`, con nuova riproduzione e stack trace completo.

## Log E Allegati

Non risultano allegati sul ticket, ma i commenti contengono stack trace sufficienti a localizzare il punto di failure:

- `Tarko.Shared.Services.RT.InterventiJasperReportGenerator.cs:line 37`
- `MaintenanceAppApi.Controllers.ReportController.cs:line 28`
- `Tarko.Jasper.Services.External.JasperPdfService.cs:line 41`
- `Tarko.Jasper.Services.JasperReportGeneratorService.cs:line 22`
- errore finale di connessione: `No connection could be made because the target machine actively refused it 127.0.0.1:7000`

## Dipendenze

- Configurazione ambiente Bosticco TEST
- Raggiungibilita del servizio Jasper o del relativo endpoint HTTP
- Configurazioni applicative usate dal flusso `WEBAPI-MANUTENTORI` per la generazione PDF

## Verdetto Attuale Di Completezza

`READY_FOR_NORMALIZATION`

Il ticket contiene versione, scenario, endpoint riproducibile, messaggio di errore e stack trace utili per passare alla fase di triage tecnico e piano di soluzione.
