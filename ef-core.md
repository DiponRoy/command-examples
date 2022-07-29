
# Entity Framework
 https://docs.microsoft.com/en-us/ef/core/cli/powershell
## Most Used Commands
```
Clear

Drop-Database

Add-Migration MigrationName
Remove-Migration MigrationName

update-database
Get-Migration

Script-Migration
Script-Migration -From 'PreviousMigration' -To 'LastMigration'

Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=AppDb;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Model -Context AppDbContext -Namespace Db.App.Model
```

## Scaffold
**item**
```
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext -ContextNamespace New.Namespace
```
## need to add


## tmpl
**item**
```
cmd     #
cmd     #
cmd     #
```
## need to add
