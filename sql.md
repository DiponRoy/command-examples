## COLUMN
```
ALTER TABLE [dbo].[Users] ADD CreatedON DATETIME NULL;

ALTER TABLE [dbo].[Users] DROP COLUMN CreatedON;
```

## CONSTRAINT
```
ALTER TABLE [dbo].[Users] ADD CreatedON DATETIME NULL;
ALTER TABLE [dbo].[Users] ADD CONSTRAINT DF_Users_CreatedON DEFAULT GETUTCDATE() FOR [CreatedON];

ALTER TABLE [dbo].[Users] DROP CONSTRAINT DF_Users_CreatedON;
ALTER TABLE [dbo].[Users] DROP COLUMN CreatedON;
```




## tmpl
**item**
```
cmd     #
cmd     #
cmd     #
```
