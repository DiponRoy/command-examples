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
UPDATE TABLE [dbo].[Users] SET CreatedON = GETUTCDATE();
ALTER TABLE [dbo].[Users] ALTER COLUMN CreatedON DATETIME NOT NULL;
ALTER TABLE [dbo].[Users] ADD CONSTRAINT DF_Users_CreatedON DEFAULT GETUTCDATE() FOR [CreatedON];

ALTER TABLE [dbo].[Users] DROP CONSTRAINT DF_Users_CreatedON;
ALTER TABLE [dbo].[Users] DROP COLUMN CreatedON;
```

## CHECK CONSTRAINT
```
ALTER TABLE Users ADD CONSTRAINT CHK_AccountStatusId CHECK (AccountStatusId IN (0, 1, 2));

ALTER TABLE Users DROP CONSTRAINT CHK_AccountStatusId;
```




## tmpl
**item**
```
cmd     #
cmd     #
cmd     #
```
