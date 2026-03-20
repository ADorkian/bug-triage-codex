---
name: tarko-solution-planner-codebase
description: Ground a Tarko solution plan in the real Tarko codebase and in the curated `.AiInfo` architecture notes. Use when the `solution-planner` agent, or any bug-analysis workflow on Tarko, must turn a normalized issue into concrete modules to inspect, likely file paths, relevant test projects, architectural constraints, and the smallest defensible fix based on the repository under `C:\\BtoWebNet\\MENSTD\\STANDARD` and the structure docs under `C:\\BtoWebNet\\MENSTD\\.AiInfo`.
---

# Tarko Solution Planner Codebase

Usa questa skill per passare dal bug al codice Tarko reale senza inventare moduli o percorsi.

## Workflow

1. Parti da `C:\BtoWebNet\MENSTD\.AiInfo\INDEX.md`.
2. Leggi solo i file `.AiInfo` coerenti con il bug:
   - `ProjectOverview/SOLUZIONE.md` per architettura, layer e convenzioni generali.
   - `ProjectOverview/ProgettiSoluzione.md` per mappare nomi progetto -> path `.csproj`.
   - `Services/READ_SERVICE.md` o `Services/WRITE_SERVICE.md` se il bug tocca servizi.
   - `Queries/QueryDomain.md` o `Queries/QueryInternalDomain.md` se il bug tocca query.
   - `Commands/*.md` se il bug richiede scritture o mutazioni.
   - `Navigation/MenuPrincipale.md` se il bug nasce da menu, form o navigazione.
   - `References/TarkoCommon.md` se servono enum condivisi, helper o convenzioni cross-layer.
   - `References/Seeders.md` se il piano richiede seed o integrazione test.
3. Cerca poi nella codebase reale sotto `C:\BtoWebNet\MENSTD\STANDARD`.
4. Usa `rg` per trovare namespace, classi, servizi, controller, query, command e test correlati al bug.
5. Riporta nel piano solo file e moduli sostenuti da evidenze del repository o da `.AiInfo`.

## Regole

- Considera `C:\BtoWebNet\MENSTD\STANDARD` come path canonico della codebase. Se un prompt cita `.STANDARD`, trattalo come refuso e usa `STANDARD`.
- Tratta `.AiInfo` come mappa di orientamento, non come verita' assoluta sul fix.
- Se `.AiInfo` e codebase divergono, privilegia la codebase reale e annota il disallineamento.
- Non limitarti a nominare macro-progetti come `Tarko.Services`: prova a scendere almeno a cartella, namespace o classe plausibile.
- Se non trovi il file esatto, indica il modulo piu' vicino e spiega perche'.
- Per Tarko preferisci fix minimi e localizzati; evita refactor trasversali salvo evidenze forti.

## Heuristiche Tarko

- UI web e menu: guarda `STANDARD/Btoweb_Forms/BtowebNet`, `Navigation/MenuPrincipale.md`, eventuali servizi e query collegati.
- Web API e app esterne: guarda `STANDARD/WebApiME/*`, inclusi `WebApiME`, `MaintenanceAppApi`, `TechnicalServiceAppApi`.
- Dominio applicativo: parti da `Tarko.Services`, `Tarko.CoreServices`, `Tarko.InternalServices`, `Tarko.Domain`, `Tarko.InternalDomain`, `Tarko.Persistence`, `Tarko.Common`, `Tarko.Shared`.
- Reportistica: guarda `Tarko.Report.Jasper` e i chiamanti in `Tarko.Shared` o nelle Web API.
- Contabilita': guarda i progetti `AklContabilita.*` e `NubContabilita*`.
- Test: cerca prima in `STANDARD/Btoweb_Forms/Tarko.Services.Tests`, `STANDARD/Btoweb_Forms/Tarko.Services.IntegrationTests` e nei test del modulo specifico.

## Query Di Ricerca Utili

Usa query mirate come queste e adattale al bug:

```powershell
rg -n "NomeEntita|NomeFunzione|MessaggioErrore" C:\BtoWebNet\MENSTD\STANDARD
rg --files C:\BtoWebNet\MENSTD\STANDARD | rg "Tarko\\.(Services|CoreServices|InternalServices|Persistence|Shared|Common|Domain|InternalDomain)"
rg -n "Controller|Report|Query|Command|Service" C:\BtoWebNet\MENSTD\STANDARD\WebApiME C:\BtoWebNet\MENSTD\STANDARD\Btoweb_Forms
rg -n "namespace .*<Dominio>|class .*<Classe>|interface I.*<Servizio>" C:\BtoWebNet\MENSTD\STANDARD
```

## Output Atteso Per Il Solution Planner

Quando contribuisci al `solution-planner`, produci soprattutto:

- progetti e moduli da ispezionare per primi
- file o namespace plausibili, con motivazione breve
- test gia' esistenti da estendere o aree test da coprire
- vincoli architetturali emersi da `.AiInfo`
- ipotesi esplicite quando un collegamento e' probabile ma non ancora confermato

## Riferimenti Locali

Leggi [tarko-map.md](references/tarko-map.md) per i percorsi canonici, i documenti `.AiInfo` principali e una mappa rapida dei progetti Tarko piu' utili al planning.
