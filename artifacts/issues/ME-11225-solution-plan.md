# ME-11225 Piano Di Soluzione

## Obiettivo

Ripristinare la generazione del PDF Rapporto Intervento nel flusso Technical Services, evitando che il servizio report Jasper tenti connessioni non valide verso `127.0.0.1:7000` nell'ambiente Bosticco TEST.

## Ipotesi

Il flusso di generazione usa un endpoint Jasper costruito o risolto in modo scorretto. `PublicUrlBase` non sembra essere il parametro che governa questa chiamata, oppure non viene letto nel path effettivamente usato da `JasperPdfService`. Il codice potrebbe ricadere su un default locale o su una configurazione non valorizzata dopo gli aggiornamenti.

## File O Moduli Da Ispezionare Per Primi

- `Tarko.Shared/Services/RT/InterventiJasperReportGenerator.cs`
- `WebApiME/MaintenanceAppApi/Controllers/ReportController.cs`
- `Tarko.Report.Jasper/Services/External/JasperPdfService.cs`
- `Tarko.Report.Jasper/Services/JasperReportGeneratorService.cs`
- configurazioni/app settings del servizio che definiscono host, porta o base URL Jasper

## Approccio Di Fix Suggerito

1. Tracciare il path completo dalla chiamata `report/interventi` fino alla costruzione dell'endpoint HTTP usato da `JasperPdfService`.
2. Verificare da quale configurazione nasce il target `127.0.0.1:7000` e se esiste un fallback implicito quando il valore atteso manca.
3. Implementare il fix minimo difendibile: usare la configurazione corretta per il servizio Jasper nel flusso report intervento oppure fallire in modo esplicito se la configurazione obbligatoria manca, evitando default silenziosi non validi.
4. Controllare che anteprima APP e link di scarico mail condividano la stessa pipeline, cosi' il fix copra entrambi i percorsi.
5. Migliorare logging o messaggio tecnico lato server se la configurazione Jasper non e' valorizzata, per ridurre riaperture simili.

## Casi Limite

- Ambienti legacy in cui Jasper gira davvero in locale e `127.0.0.1` e' un valore valido.
- Configurazioni incomplete o parzialmente migrate fra versioni `25.2.x`, `25.3.x` e `26.0.x`.
- Differenze fra generazione anteprima e download da link mail, anche se oggi sembrano condividere la stessa failure.
- Interventi diversi da `IMA_BOS_25/00000005` che potrebbero passare per template Jasper differenti.

## Strategia Di Test

- Verificare con test unitari o di integrazione il metodo che risolve l'endpoint Jasper, coprendo sia configurazione valida sia configurazione mancante.
- Eseguire uno smoke test del controller `report/interventi` usando un Jasper endpoint stub/mock raggiungibile.
- Verificare manualmente in ambiente di test che l'anteprima PDF del rapporto intervento torni disponibile per il caso `IMA_BOS_25/00000005`.
- Ripetere il controllo anche dal link di scarico usato dal flusso mail, se raggiungibile nello stesso ambiente.

## Strategia Di Rollback

Se il fix introduce regressioni, ripristinare la precedente risoluzione endpoint o la configurazione precedente del servizio Jasper, mantenendo comunque i log aggiuntivi se non alterano il comportamento runtime. Se il cambio e' guidato da config, prevedere rollback tramite restore del valore precedente senza toccare altri flussi report.

## Criteri Di Completamento

- Il controller `report/interventi` non tenta piu' connessioni non valide verso `127.0.0.1:7000` nell'ambiente target.
- L'anteprima PDF del Rapporto Intervento viene generata correttamente sul caso noto.
- Il flusso di download collegato alla mail usa la stessa correzione o e' verificato separatamente.
- Esiste copertura di test almeno sul punto in cui viene risolto l'endpoint Jasper o sul servizio che effettua la chiamata.
- Restano espliciti eventuali prerequisiti di configurazione ambiente.

## Assunzioni Esplicite

- Il caso `IMA_BOS_25/00000005` e' ancora valido come smoke test.
- Il problema non dipende dai dati del singolo intervento ma dalla raggiungibilita/configurazione del servizio Jasper.
- `PublicUrlBase` non governa direttamente la chiamata che oggi fallisce, oppure non viene consumato nel path incriminato.
