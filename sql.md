## DROP
```
DROP TABLE [dbo].[Users];
DROP TABLE IF EXISTS [dbo].[Users];
```

## COLUMN
```
ALTER TABLE [dbo].[Users] ADD CreatedON DATETIME NULL;

ALTER TABLE [dbo].[Users] DROP COLUMN CreatedON;
```

## PRIMARY KEY
```
ALTER TABLE [dbo].[Users] ADD CONSTRAINT PK_Users PRIMARY KEY(Id);

ALTER TABLE [dbo].[Users] DROP CONSTRAINT PK_Users;
```

## FOREIGN KEY
```
ALTER TABLE [dbo].[Users] ADD CONSTRAINT FK_Users_UserTypes FOREIGN KEY (UserTypeId) REFERENCES UserTypes (Id);

ALTER TABLE [dbo].[Users] DROP CONSTRAINT FK_Users_UserTypes;
```

## DEFAULT CONSTRAINT
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
