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

## PRIMARY KEY CONSTRAINT
```
ALTER TABLE [dbo].[Users] ADD CONSTRAINT PK_Users PRIMARY KEY(Id);

ALTER TABLE [dbo].[Users] DROP CONSTRAINT PK_Users;
```

## FOREIGN KEY CONSTRAINT
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
ALTER TABLE Users ADD CONSTRAINT CHK_Users_AccountStatusId CHECK (AccountStatusId IN (0, 1, 2));

ALTER TABLE Users DROP CONSTRAINT CHK_Users_AccountStatusId;
```

## UNIQUE CONSTRAINT
```
ALTER TABLE Users ADD CONSTRAINT UC_Users_UserName UNIQUE (UserName);

ALTER TABLE Users DROP CONSTRAINT UC_Users_UserName;
```
**Composite unique key**
```
ALTER TABLE Users ADD CONSTRAINT UC_Users_UserName_AccountStatusId UNIQUE (UserName, AccountStatusId);

ALTER TABLE Users DROP CONSTRAINT UC_Users_UserName_AccountStatusId;
```

## INDEX
```
CREATE INDEX IX_Users_UserName ON Users (UserName);

DROP INDEX Users.IX_Users_UserName;
ALTER TABLE Users DROP INDEX IX_Users_UserName;   --mysql
```




## tmpl
**item**
```
cmd     #
cmd     #
cmd     #
```
