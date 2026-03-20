# Tarko Map

## Percorsi Canonici

- Root repository: `C:\BtoWebNet\MENSTD`
- Codebase principale: `C:\BtoWebNet\MENSTD\STANDARD`
- Documentazione strutturale AI: `C:\BtoWebNet\MENSTD\.AiInfo`

## Punto Di Ingresso `.AiInfo`

- `C:\BtoWebNet\MENSTD\.AiInfo\INDEX.md`

## Documenti `.AiInfo` Da Caricare In Base Al Caso

- Architettura generale:
  - `C:\BtoWebNet\MENSTD\.AiInfo\ProjectOverview\SOLUZIONE.md`
  - `C:\BtoWebNet\MENSTD\.AiInfo\ProjectOverview\ProgettiSoluzione.md`
- Servizi:
  - `C:\BtoWebNet\MENSTD\.AiInfo\Services\READ_SERVICE.md`
  - `C:\BtoWebNet\MENSTD\.AiInfo\Services\WRITE_SERVICE.md`
- Query:
  - `C:\BtoWebNet\MENSTD\.AiInfo\Queries\QueryDomain.md`
  - `C:\BtoWebNet\MENSTD\.AiInfo\Queries\QueryInternalDomain.md`
- Command:
  - `C:\BtoWebNet\MENSTD\.AiInfo\Commands\InsertCommandDomain.md`
  - `C:\BtoWebNet\MENSTD\.AiInfo\Commands\UpdateCommandDomain.md`
  - `C:\BtoWebNet\MENSTD\.AiInfo\Commands\DeleteCommandDomain.md`
- Navigazione:
  - `C:\BtoWebNet\MENSTD\.AiInfo\Navigation\MenuPrincipale.md`
- Riferimenti:
  - `C:\BtoWebNet\MENSTD\.AiInfo\References\TarkoCommon.md`
  - `C:\BtoWebNet\MENSTD\.AiInfo\References\Seeders.md`

## Progetti Tarko Più Frequenti Nel Planning

- `STANDARD/Btoweb_Forms/Tarko.Services/Tarko.Services.csproj`
- `STANDARD/Btoweb_Forms/Tarko.CoreServices/Tarko.CoreServices.csproj`
- `STANDARD/Btoweb_Forms/Tarko.InternalServices/Tarko.InternalServices.csproj`
- `STANDARD/Btoweb_Forms/Tarko.Persistence/Tarko.Persistence.csproj`
- `STANDARD/Btoweb_Forms/Tarko.Shared/Tarko.Shared.csproj`
- `STANDARD/Btoweb_Forms/Tarko.Common/Tarko.Common.csproj`
- `STANDARD/Btoweb_Forms/Tarko.Services.Tests/Tarko.Services.Tests.csproj`
- `STANDARD/Btoweb_Forms/Tarko.Services.IntegrationTests/Tarko.Services.IntegrationTests.csproj`
- `STANDARD/WebApiME/WebApiME/WebApiME.csproj`
- `STANDARD/WebApiME/MaintenanceAppApi/MaintenanceAppApi.csproj`
- `STANDARD/WebApiME/TechnicalServiceAppApi/TechnicalServiceAppApi.csproj`
- `STANDARD/Btoweb_Forms/Tarko.Report.Jasper/Tarko.Jasper.csproj`

## Convenzioni Utili

- `Affects versions` Jira non indica automaticamente un modulo: usa il bug per trovare area funzionale e poi mappa l'area al progetto.
- Gli enum condivisi stanno in `Tarko.Common`.
- I mapping si distribuiscono tipicamente tra `Tarko.Persistence`, `Tarko.Services` e `Tarko.InternalServices`.
- Le issue di reportistica spesso attraversano `WebApiME/*`, `Tarko.Shared` e `Tarko.Report.Jasper`.
- Per i test, cerca prima test unitari o integration test gia' vicini al dominio prima di proporre nuovi harness.
