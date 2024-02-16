







- Basic https://www.w3schools.com/sql/sql_constraints.asp
- Unique Indexes vs Unique Constraints https://www.mssqltips.com/sqlservertip/4270/difference-between-sql-server-unique-indexes-and-unique-constraints/
- DateTime conversion number https://www.mssqltips.com/sqlservertip/1145/date-and-time-conversions-using-sql-server/
- Functions
    - [w3schools](https://www.w3schools.com/sql/sql_ref_sqlserver.asp)

```
SELECT @@VERSION
```


## DROP
**Table**
```
DROP TABLE [dbo].[Users];
DROP TABLE IF EXISTS [dbo].[Users];
IF OBJECT_ID(N'[dbo].[Users]', N'U') IS NOT NULL
	DROP TABLE [dbo].[Users]
```
**Temp Table**
```
IF object_id('tempdb..#tblRelation') IS NOT NULL
    DROP TABLE #tblRelation;
IF object_id('tempdb..##tblDetails') IS NOT NULL
    DROP TABLE ##tblDetails;
```

## COLUMN
```
ALTER TABLE [dbo].[Users] ADD CreatedON DATETIME NULL;
ALTER TABLE [dbo].[Users] ALTER COLUMN CreatedON DATETIME NOT NULL;

ALTER TABLE [dbo].[Users] DROP COLUMN CreatedON;
```

**Conditional**
```
IF EXISTS(SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_SCHEMA = 'dbo' AND TABLE_NAME = 'Users' AND COLUMN_NAME = 'Zone')
BEGIN
	ALTER TABLE [dbo].[Users] ALTER COLUMN [Zone] NVARCHAR(50) NULL;
END
ELSE
BEGIN
	ALTER TABLE [dbo].[Users] ADD [Zone] NVARCHAR(50) NULL;
END
```

## PRIMARY KEY CONSTRAINT
```
ALTER TABLE [dbo].[Users] ADD CONSTRAINT PK_Users PRIMARY KEY(Id);
ALTER TABLE [dbo].[Users] ADD CONSTRAINT PK_Users PRIMARY KEY CLUSTERED (Id ASC);

ALTER TABLE [dbo].[Users] DROP CONSTRAINT PK_Users;
```
**Composite key**
```
ALTER TABLE [dbo].[Group_User] ADD CONSTRAINT PK_Group_User PRIMARY KEY(GroupId, UserId);
ALTER TABLE [dbo].[Group_User] ADD CONSTRAINT PK_Group_User PRIMARY KEY(GroupId ASC, UserId ASC);

ALTER TABLE [dbo].[Group_User] DROP CONSTRAINT PK_Group_User;
```

## FOREIGN KEY CONSTRAINT
If you really want to create a foreign key to a non-primary key, it MUST be a column that has a unique constraint on it.
```
ALTER TABLE [dbo].[Users] ADD CONSTRAINT FK_Users_UserTypes FOREIGN KEY (UserTypeId) REFERENCES UserTypes (Id);

ALTER TABLE [dbo].[Users] DROP CONSTRAINT FK_Users_UserTypes;
```
**Composite key**
```
ALTER TABLE [dbo].[GroupUserImage] ADD CONSTRAINT FK_GroupUserImage_GroupUser FOREIGN KEY (GroupId, UserId) REFERENCES Group_User(GroupId, UserId);

ALTER TABLE [dbo].[GroupUserImage] DROP CONSTRAINT FK_GroupUserImage_GroupUser;
```

## DEFAULT CONSTRAINT
**Not Nullable**
```
ALTER TABLE [dbo].[Users] ALL CreatedON DATETIME NULL;
UPDATE TABLE [dbo].[Users] SET CreatedON = GETUTCDATE();
ALTER TABLE [dbo].[Users] ALTER COLUMN CreatedON DATETIME NOT NULL;
ALTER TABLE [dbo].[Users] ADD CONSTRAINT DF_Users_CreatedON DEFAULT GETUTCDATE() FOR [CreatedON];

ALTER TABLE [dbo].[Users] DROP CONSTRAINT DF_Users_CreatedON;
ALTER TABLE [dbo].[Users] DROP COLUMN CreatedON;
```
**Nullable**
```
ALTER TABLE [dbo].[Users] ADD VisibleOnId INT NULL CONSTRAINT DF_Users_VisibleOn DEFAULT 1 WITH VALUES;
```

## CHECK CONSTRAINT
```
ALTER TABLE Users ADD CONSTRAINT CK_Users_AccountStatusId CHECK (AccountStatusId IN (0, 1, 2));

ALTER TABLE Users DROP CONSTRAINT CK_Users_AccountStatusId;
```
```
SELECT * FROM sys.objects WHERE sys.objects.type = 'C'

SELECT 
	SCHEMA_NAME(schema_id) AS SchemaName, --QUOTENAME()
	OBJECT_NAME(parent_object_id) AS TableName,
	* 
FROM sys.check_constraints dc

SELECT 
    TableName = t.Name,
    ColumnName = c.Name,
    dc.Name,
    dc.definition
FROM sys.tables t
INNER JOIN sys.check_constraints dc ON t.object_id = dc.parent_object_id
INNER JOIN sys.columns c ON dc.parent_object_id = c.object_id AND c.column_id = dc.parent_column_id
```
## UNIQUE KEY CONSTRAINT
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
```
**Descending**
```
CREATE INDEX IX_Users_CreatedON ON Users (CreatedON DESC);
```


## UNIQUE INDEX
```
ALTER TABLE Users ADD CONSTRAINT UC_Users_UserName UNIQUE (UserName);
CREATE UNIQUE INDEX UIX_Users_UserName ON Users (UserName);

DROP INDEX Users.UIX_Users_UserName;
ALTER TABLE Users DROP CONSTRAINT UC_Users_UserName;
```

## Find Objects
```
--table
SELECT TABLE_NAME
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_TYPE = 'BASE TABLE'
AND TABLE_NAME LIKE '%role%'
ORDER BY TABLE_NAME

--view
SELECT v.name
FROM INFORMATION_SCHEMA.VIEWS iv
JOIN sys.views v on v.name = iv.Table_Name
WHERE v.name LIKE '%role%'
ORDER BY v.name

--sp
SELECT ROUTINE_NAME 
FROM INFORMATION_SCHEMA.ROUTINES
WHERE ROUTINE_TYPE = 'PROCEDURE'
AND ROUTINE_NAME LIKE '%role%'

--tr
SELECT  name as trigger_name
, object_name(parent_obj) as tableName
, object_schema_name(parent_obj) as schemaName 
,OBJECTPROPERTY( id, 'ExecIsUpdateTrigger') AS isupdate 
,OBJECTPROPERTY( id, 'ExecIsDeleteTrigger') AS isdelete 
,OBJECTPROPERTY( id, 'ExecIsInsertTrigger') AS isinsert 
,OBJECTPROPERTY( id, 'ExecIsAfterTrigger') AS isafter 
,OBJECTPROPERTY( id, 'ExecIsInsteadOfTrigger') AS isinsteadof 
,OBJECTPROPERTY(id, 'ExecIsTriggerDisabled') AS [disabled] 
FROM    sysobjects s
WHERE s.type = 'TR' 
```

## ROWVERSION
```
DROP TABLE IF EXISTS TestUser;
GO
CREATE TABLE TestUser (
	Id INT IDENTITY(1, 1) NOT NULL,
	FirstName VARCHAR(MAX) NULL,
	LastName VARCHAR(MAX) NULL,
	[RowVersion] ROWVERSION,			--Can use TIMESTAMP
);

GO
TRUNCATE TABLE TestUser;
INSERT INTO TestUser (FirstName, LastName) VALUES (1, 2);				--0x0000000000011173
UPDATE TestUser SET FirstName = '11', LastName = '22' WHERE Id = 1;		--0x0000000000011174
UPDATE TestUser SET FirstName = '111', LastName = '222' WHERE Id = 1;	--0x0000000000011175

SELECT 
	*
	,CONVERT(BIGINT, [RowVersion]) AS RowVersionNo
FROM TestUser;

SELECT @@DBTS AS LastUsedRowVersion;
```

**Add RowVersion**
```
ALTER TABLE TestUser ADD [RowVersion] ROWVERSION;		--Will add column, value will be auto populated
```

**Check RowVersion in Update/Delete**
```
TRUNCATE TABLE TestUser;
INSERT INTO TestUser (FirstName, LastName) VALUES (1, 2);																			--0x000000000001117A
UPDATE TestUser SET FirstName = '11', LastName = '22' WHERE Id = 1 AND [RowVersion] =  CONVERT(BINARY(8), '0x000000000001117A', 1);	--0x000000000001117B
DELETE FROM TestUser  WHERE Id = 1 AND [RowVersion] =  CONVERT(BINARY(8), '0x000000000001117B', 1);	

SELECT * FROM TestUser WHERE Id = 1;
```

## Trigger
https://stackoverflow.com/questions/20205639/insert-delete-update-trigger-in-sql-server
https://www.red-gate.com/simple-talk/databases/sql-server/database-administration-sql-server/sql-server-triggers-good-scary/#:~:text=A%20trigger%20is%20a%20set,operation%20within%20the%20MERGE%20statement.
```
DROP TABLE IF EXISTS TestUser;
DROP TABLE IF EXISTS TestUser_Audit;
GO
CREATE TABLE TestUser (
	Id INT IDENTITY(1, 1) NOT NULL,
	FirstName VARCHAR(MAX) NULL,
	LastName VARCHAR(MAX) NULL,
	[RowVersion] VARCHAR(MAX) NULL,
);
CREATE TABLE TestUser_Audit (
	AuditId INT IDENTITY(1, 1) NOT NULL,
	AuditType VARCHAR(MAX)  NOT NULL,
	AuditAt DATETIME NOT NULL DEFAULT(GETUTCDATE()),

	Id INT NOT NULL,
	FirstName VARCHAR(MAX) NULL,
	LastName VARCHAR(MAX) NULL,
	[RowVersion] VARCHAR(MAX) NULL,
	[OldRowVersion] VARCHAR(MAX) NULL
);
```
```
GO
CREATE TRIGGER TR_TestUser_Audit
ON [dbo].[TestUser]
AFTER UPDATE, INSERT, DELETE 
AS
BEGIN
SET NOCOUNT ON;

DECLARE @auditType VARCHAR (50) = '';
DECLARE @oldRowVersion VARCHAR (MAX) = '';

-- update
IF EXISTS (SELECT * FROM Inserted) AND EXISTS (SELECT * FROM Deleted)
BEGIN
    SET @auditType = 'UPDATE';
	SELECT @oldRowVersion = [RowVersion] FROM Deleted;
END

-- insert
IF EXISTS (SELECT * FROM Inserted) AND NOT EXISTS(SELECT * FROM Deleted)
BEGIN
    SET @auditType = 'INSERT';
END

-- delete
IF EXISTS (SELECT * FROM Deleted) AND NOT EXISTS(SELECT * FROM Inserted)
BEGIN
    SET @auditType = 'DELETE';
END

-- log
IF OBJECT_ID('tempdb..#tmpTbl') IS NOT NULL 
	DROP TABLE #tmpTbl;
WITH Logs
AS
(
	SELECT *, 1 AS PriorityNo FROM Inserted
	UNION 
	SELECT *, 2 AS PriorityNo FROM Deleted
)
SELECT TOP(1) *
INTO #tmpTbl
FROM Logs
ORDER BY PriorityNo;

-- add log
INSERT INTO TestUser_Audit  
(
	AuditType
	,Id
	,FirstName
	,LastName
	,[RowVersion]
	,[OldRowVersion]
)
SELECT 
	@auditType
    ,Id
    ,FirstName
    ,LastName
    ,[RowVersion]
    ,@oldRowVersion
FROM #tmpTbl
SET NOCOUNT OFF;
END
```
```
TRUNCATE TABLE TestUser;
TRUNCATE TABLE TestUser_Audit;
INSERT INTO TestUser (FirstName, LastName, [RowVersion])
VALUES (1, 2, 3);
UPDATE TestUser SET FirstName = '11', LastName = '22', [RowVersion] = '33' WHERE Id = 1;
UPDATE TestUser SET FirstName = '111', LastName = '222', [RowVersion] = '333' WHERE Id = 1;
DELETE FROM TestUser WHERE Id = 1
SELECT * FROM TestUser;
SELECT * FROM TestUser_Audit;
```

## String Split
```
DECLARE @FilePath VARCHAR(50) = 'My\Super\Long\String\With\Long\Words'
DECLARE @FindChar VARCHAR(1) = '\'
-- text before last slash
SELECT LEFT(@FilePath, LEN(@FilePath) - CHARINDEX(@FindChar,REVERSE(@FilePath))) AS Before
-- text after last slash
SELECT RIGHT(@FilePath, CHARINDEX(@FindChar,REVERSE(@FilePath))-1) AS After
-- the position of the last slash
SELECT LEN(@FilePath) - CHARINDEX(@FindChar,REVERSE(@FilePath)) + 1 AS LastOccuredAt
```

## All In One
```
SELECT 
	UserName, COUNT(Id) AS Total
FROM Users
WHERE UserName IS NOT NULL
GROUP BY UserName
HAVING COUNT(Id) > 2
ORDER BY UserName
```

## LOOP
```
DECLARE @orderId INT = 0;
WHILE @orderId < 5
BEGIN
	--IF @orderId > 5
	--	BREAK;
	--IF @orderId <= 0
	--	CONTINUE;
	PRINT @orderId;
	SET @orderId = @orderId +1;
END
```

## CURSOR
```
DECLARE @id INT, @name VARCHAR(250);
DECLARE tblCoursor CURSOR FOR
	SELECT Id, UserName
	FROM [Users]  o
	WHERE o.AccountStatusId = 2;
OPEN tblCoursor
	FETCH NEXT FROM tblCoursor INTO @id, @name
	WHILE(@@FETCH_STATUS = 0)
	BEGIN
		PRINT CONCAT(@id, ' ', @name);
		FETCH NEXT FROM tblCoursor INTO @id, @name
	END
CLOSE tblCoursor
DEALLOCATE tblCoursor
```

## TRY CATCH
```
BEGIN TRY
	/*Do something*/
	PRINT 'Hello';
END TRY
BEGIN CATCH
	DECLARE @error VARCHAR = 'Error message.';
	THROW 50000, @error, 1;  
END CATCH
```
There is no FINALLY

## TRANSACTION
```
DECLARE @mainTran VARCHAR(250) = 'TRANSACTION_NAME' ;
BEGIN TRANSACTION @mainTran;
BEGIN TRY
	/*Do something*/
	PRINT 'Hello';

	COMMIT TRANSACTION @mainTran
END TRY
BEGIN CATCH
	ROLLBACK TRANSACTION @mainTran;

	DECLARE @error VARCHAR = 'Error message.';
	THROW 50000, @error, 1;   
END CATCH
```

## IF ELSE
```
DECLARE @value INT = 3;
IF @value=1
BEGIN
	PRINT '1';
END
ELSE IF @value=2
BEGIN
	PRINT '2';
END
ELSE
BEGIN
	PRINT 'Unknown';
END
```

## CASE 
**Simple CASE**
```
DECLARE @userTypeId INT = 2;
DECLARE @userType VARCHAR(100);
SET @userType = CASE @userTypeId
                    WHEN 1 THEN 'Admin'
                    WHEN 2 THEN 'User'
                    ELSE 'Unknown'
                END;
SELECT @userType;
```
**Searched CASE**
```
DECLARE @userTypeId INT = 2;
DECLARE @userType VARCHAR(100);
SET @userType = CASE 
                    WHEN @userTypeId = 1 THEN 'Admin'
                    WHEN @userTypeId = 2 THEN 'User'
                    ELSE 'Unknown'
                END;
SELECT @userType; 
```


## IDENTITY ON OF
```
SET IDENTITY_INSERT [dbo].[AccountStatus] ON 
/*do something*/
SET IDENTITY_INSERT [dbo].[AccountStatus] OFF
```
## CONSTRAINT CHECK NOCHECK
```
ALTER TABLE [dbo].[AccountStatus] NOCHECK CONSTRAINT ALL;
/*do something*/
ALTER TABLE [dbo].[AccountStatus] CHECK CONSTRAINT ALL;
```

## RESET IDENTITY
```
TRUNCATE TABLE tblName;
--or
DELETE FROM tblName;

DBCC CHECKIDENT('tblName', RESEED, 0);
```

## MERGE
```
DROP TABLE IF EXISTS AccountStatus;
CREATE TABLE [dbo].[AccountStatus] (
    [ID]                   INT NOT NULL,
    [AccountStatusName]    NVARCHAR (200) NOT NULL,
    [AccountStatusName_FR] NVARCHAR (200) NULL
);
INSERT INTO AccountStatus VALUES (5, N'x', N'x');


MERGE [dbo].[AccountStatus] AS T
USING ( VALUES 
	(1, N'Activated', N'Activé'),
	(2, N'Inactive', N'Inactif'),
	(3, N'Disabled', N'Désactivé'),
	(4, N'None', N'Aucun') /*will delete after db migrations*/
) AS S ([ID], [AccountStatusName], [AccountStatusName_FR])
ON T.[ID] = S.[ID]
WHEN MATCHED
    THEN UPDATE 
        SET 
            T.[AccountStatusName] = S.[AccountStatusName],
            T.[AccountStatusName_FR] =  S.[AccountStatusName_FR]
WHEN NOT MATCHED BY TARGET
    THEN INSERT ([ID], [AccountStatusName], [AccountStatusName_FR])
        VALUES ([ID], [AccountStatusName], [AccountStatusName_FR])
WHEN NOT MATCHED BY SOURCE
    THEN DELETE;
```


## DATETIME
```
SELECT
	[Now] = GETDATE(),
	[UtcNow] = GETUTCDATE(),
	[YesterdayNow] = DATEADD(DD, -1, GETDATE()),
	[TomorrowNow] = DATEADD(DD, 1, GETDATE()),

	[Date] = CONVERT(DATE, GETDATE()),

	[DateTimeFromString] = CONVERT(DATETIME, '2021.08.16', 102),
	[DateTimeToString] = CONVERT(VARCHAR(100), GETDATE(), 102),

	[DayNo] = DATEPART(DAY, GETDATE()),
	[WeekDayNo] = DATEPART(WEEKDAY, GETDATE()),
	[MonthNo] = DATEPART(MONTH, GETDATE()),
	[YearNo] = DATEPART(YEAR, GETDATE()),

	[WeekDayName] = DATENAME(WEEKDAY, GETDATE()),
	[MonthName] = DATENAME(MONTH, GETDATE())
	;
```
**Day Range**
```
SELECT
	DATEADD(DAY, DATEDIFF(DAY, 0, GETDATE()), 0) AS StartDateTime,
	DATEADD(SECOND, -1, DATEADD(DAY, DATEDIFF(DAY, 0, GETDATE()) + 1, 0)) AS EndDateTime	--MILLISECOND
```

**Compare**
```
SELECT * FROM Users WHERE CreatedON BETWEEN DATEADD(MINUTE, -6, GETUTCDATE()) AND GETUTCDATE();       --data added in last 5 mint
SELECT * FROM Users WHERE CAST(CreatedON AS DATE) = '2021-12-20' ;                                    --date equal
SELECT * FROM Users WHERE CreatedON = '2021-12-20 10:34:03' ;                                         --date time equal
SELECT * FROM Users WHERE CreatedON BETWEEN '2021-12-20 00:00:00.000' AND '2022-06-02 23:59:59.000';  --date time range
SELECT * FROM Users WHERE '2021-12-20 10:34:00' < CreatedON AND CreatedON < '2021-12-21 00:00:00';    --date time greater less
SELECT * FROM Users WHERE CAST(CreatedON AS TIME) <> '00:00:00.0000000';							  --dates with time
SELECT * FROM Users WHERE CAST(CreatedON AS TIME) = '00:00:00.0000000';							  	  --dates without time
```

**DateTime to String**
```
SELECT CONVERT(DATETIME, '2023-10-24', 102) --yyyy-mm-dd
```

**DateTime Overlapping**
```
DECLARE @spiffRanges TABLE (
	Id VARCHAR(MAX),
	RangeStartDateTime INT,
	RangeEndDateTime INT
)

INSERT INTO @spiffRanges 
VALUES 
('Spiff0', 1, 4),
('Spiff1', 5, 15),
('Spiff2', 20, 30),
--overlaping
('Spiff3', 45, 55),
('Spiff4', 50, 60)

SELECT 
	sr.*
FROM @spiffRanges sr
WHERE EXISTS (
	SELECT *
	FROM @spiffRanges rr
	WHERE rr.Id <> sr.Id
	 AND (sr.RangeStartDateTime <= rr.RangeEndDateTime) AND (sr.RangeEndDateTime >= rr.RangeStartDateTime)
);
```

## Anonymous 
**table**
```
SELECT *
FROM
(
	VALUES
	(1, 'A', 2, GETUTCDATE()),
	(2, 'B', 2, DATEADD(MINUTE, -2, GETUTCDATE())),
	(3, 'B', 2, '2021-12-20 14:34:03'),
	(3, 'B', 2, '2021-12-20 10:34:03'),
	(3, 'B', 2, '2021-12-21 00:00:00'),
	(4, 'B', 2, NULL)
) AS Users (Id, UserName, AccountStatusId, CreatedON)
```

## INSERT 
**Multi Row**
```
INSERT INTO 
	Users (Id, UserName, AccountStatusId, CreatedON)
VALUES 
(1, 'A', 2, GETUTCDATE()),
(2, 'B', 2, DATEADD(MINUTE, -2, GETUTCDATE())),
(3, 'B', 2, '2021-12-20 14:34:03'),
(3, 'B', 2, '2021-12-20 10:34:03'),
(3, 'B', 2, '2021-12-21 00:00:00'),
(4, 'B', 2, NULL);
```
```
INSERT INTO 
	Users (Id, UserName, AccountStatusId, CreatedON)
SELECT 
	Id, UserName, AccountStatusId, CreatedON
FROM TempUsers;
```

## UPDATE 
```
IF object_id('tempdb..#tblUsers') IS NOT NULL
    DROP TABLE #tblUsers;
IF object_id('tempdb..#tempUsers') IS NOT NULL
    DROP TABLE #tempUsers;

CREATE TABLE #tblUsers
(
    Id INT,
    FirstName VARCHAR(MAX) NOT NULL,
	LastName VARCHAR(MAX) NOT NULL
);
INSERT INTO #tblUsers (Id, FirstName, LastName)
VALUES 
(1, 'User', '1'),
(2, 'User', '2');
CREATE TABLE #tempUsers
(
    Id INT,
    FullName VARCHAR(MAX) NULL,
	StatusId INT
);
INSERT INTO #tempUsers (Id, StatusId)
VALUES 
(1, 0),
(2, 0),
(3, 0);
```
```
UPDATE #tempUsers 
	SET StatusId = 1
WHERE Id > 0;
```
**Using Alias**
```
UPDATE u
	SET u.StatusId = 2
FROM #tempUsers AS u
WHERE u.Id > 0;
```
**Using WITH**
```
WITH
Users
AS
(
	SELECT *
	FROM #tempUsers
	WHERE Id > 0
)
UPDATE Users
SET StatusId = 3;
```
**Using JOIN**
```
UPDATE tu
	SET tu.FullName = u.FirstName +' ' +u.LastName
FROM #tempUsers AS tu
JOIN #tblUsers AS u ON tu.Id = u.Id
WHERE tu.Id > 0;
```

## DELETE 
```
IF object_id('tempdb..#tblUsers') IS NOT NULL
    DROP TABLE #tblUsers;
IF object_id('tempdb..#tempUsers') IS NOT NULL
    DROP TABLE #tempUsers;

CREATE TABLE #tblUsers
(
    Id INT,
    FirstName VARCHAR(MAX) NOT NULL,
	LastName VARCHAR(MAX) NULL
);
INSERT INTO #tblUsers (Id, FirstName, LastName)
VALUES 
(1, 'User', '1'),
(2, 'User', '2');
CREATE TABLE #tempUsers
(
    Id INT,
    FullName VARCHAR(MAX) NULL,
	StatusId INT
);
INSERT INTO #tempUsers (Id, StatusId)
VALUES 
(1, 0),
(2, 0),
(3, 0);
```
```
DELETE FROM #tempUsers WHERE Id > 0;
```
**Using Alias**
```
DELETE u FROM #tempUsers u WHERE u.Id > 0;
```
**Using WITH**
```
WITH
Users
AS
(
	SELECT *
	FROM #tempUsers
	WHERE Id > 0
)
DELETE Users;
```
**Using JOIN**
```
DELETE tu
FROM #tempUsers AS tu
JOIN #tblUsers AS u ON tu.Id = u.Id
WHERE tu.Id > 0;
```

## INTO 
```
DROP TABLE IF EXISTS tblTestUsers;  
DROP TABLE IF EXISTS NewTestUsers;
IF object_id('tempdb..#newTempUsers') IS NOT NULL
    DROP TABLE #newTempUsers;
	 
CREATE TABLE tblTestUsers
(
    Id INT,
    FirstName VARCHAR(MAX) NOT NULL,
	LastName VARCHAR(MAX) NULL
);
INSERT INTO tblTestUsers (Id, FirstName, LastName)
VALUES 
(1, 'User', '1'),
(2, 'User', '2');
INSERT INTO tblTestUsers
VALUES 
(3, 'User', '3');

SELECT Id, (FirstName +' ' +LastName) AS FullName, 1 AS StatusId
INTO #newTempUsers
FROM tblTestUsers

SELECT Id, (FirstName +' ' +LastName) AS FullName, 1 AS StatusId
INTO NewTestUsers
FROM tblTestUsers


DECLARE @tempUsers TABLE 
(
    Id INT,
    FullName VARCHAR(MAX) NULL,
	StatusId INT
);
--SELECT Id, (FirstName +' ' +LastName) AS FullName, 1 AS StatusId
--FROM tblTestUsers
--INTO @tempUsers
INSERT INTO tblTestUsers
OUTPUT inserted.Id, inserted.FirstName +' ' +inserted.LastName, 0
INTO @tempUsers
VALUES 
(4, 'User', '4');



DECLARE @Id INT;
--SELECT 3.14 INTO @id;
--SELECT TOP(1) Id INTO @id FROM tblTestUsers;
--SELECT TOP(1) Id FROM tblTestUsers INTO @id;
--SELECT (SELECT TOP(1) Id FROM tblTestUsers) INTO @id;
SELECT TOP(1) @Id = Id FROM tblTestUsers
SET @Id = (SELECT TOP(1) Id FROM tblTestUsers)
```
**Using JOIN**
```
DELETE tu
FROM #tempUsers AS tu
JOIN #tblUsers AS u ON tu.Id = u.Id
WHERE tu.Id > 0;
```


## OUTPUT 
Doesn't work with funciton
```
INSERT INTO 
	Users (Id, UserName, AccountStatusId, CreatedON)
OUTPUT inserted.Id
VALUES (1, 'A', 2, GETUTCDATE());

UPDATE Users
SET UserName = 'B'
OUTPUT inserted.Id, deleted.UserName, inserted.UserName
WHERE Id = 1

DELETE Users
OUTPUT deleted.Id
WHERE Id = 1

DECLARE @Ids_tbl TABLE ([Id] INT); /*should be table variable*/
INSERT INTO 
	Users (Id, UserName, AccountStatusId, CreatedON)
OUTPUT inserted.Id INTO @Ids_tbl(Id)
VALUES (1, 'A', 2, GETUTCDATE());
SELECT * FROM @Ids_tbl;
```
**With MERGE**
```
DROP TABLE IF EXISTS AccountStatus;
CREATE TABLE [dbo].[AccountStatus] (
    [ID]                   INT NOT NULL,
    [AccountStatusName]    NVARCHAR (200) NOT NULL,
    [AccountStatusName_FR] NVARCHAR (200) NULL
);
INSERT INTO AccountStatus VALUES (1, N'x', N'x');
INSERT INTO AccountStatus VALUES (5, N'x', N'x');

DECLARE @logs TABLE (
	ID INT,
	ActionType NVARCHAR (200) NULL
)

MERGE [dbo].[AccountStatus] AS T
USING ( VALUES 
	(1, N'Activated', N'Activé'),
	(2, N'Inactive', N'Inactif'),
	(3, N'Disabled', N'Désactivé'),
	(4, N'None', N'Aucun') /*will delete after db migrations*/
) AS S ([ID], [AccountStatusName], [AccountStatusName_FR])
ON T.[ID] = S.[ID]
WHEN MATCHED
    THEN UPDATE 
        SET 
            T.[AccountStatusName] = S.[AccountStatusName],
            T.[AccountStatusName_FR] =  S.[AccountStatusName_FR]
		--OUTPUT inserted.ID, $action INTO @logs;
WHEN NOT MATCHED BY TARGET
    THEN INSERT ([ID], [AccountStatusName], [AccountStatusName_FR])
        VALUES ([ID], [AccountStatusName], [AccountStatusName_FR])
		--OUTPUT inserted.ID, $action INTO @logs;
WHEN NOT MATCHED BY SOURCE
    THEN DELETE
	--OUTPUT deleted.ID, $action INTO @logs;
	OUTPUT 
	(
		CASE $action
			WHEN 'INSERT' THEN inserted.ID
			WHEN 'UPDATE' THEN inserted.ID
			WHEN 'DELETE' THEN deleted.ID
			ELSE ''
		END
	)
	, $action INTO @logs;

SELECT * FROM @logs;
```
**With PROCEDURE**
```
DROP PROCEDURE IF EXISTS SP_GenerateBill;
GO
CREATE PROCEDURE SP_GenerateBill (
	@month INT,
	@year INT,
	@totalAmount DECIMAL OUTPUT
)
AS
BEGIN
	/*Do other things*/
	SET @totalAmount = 100
END;
GO

DECLARE @amount DECIMAL
EXEC SP_GenerateBill @month=12, @year=12, @totalAmount=@amount OUTPUT;
SELECT @amount AS BillAmount;
```
**With CURSOR**
```
DROP PROCEDURE IF EXISTS GetItems;
DROP TYPE IF EXISTS UT_Ids;

GO
CREATE TYPE UT_Ids AS TABLE
(
	Id INT NULL
);
GO
CREATE OR ALTER PROCEDURE GetItems (
	@ids UT_Ids READONLY,
	@listCursor CURSOR VARYING OUTPUT
)
AS
BEGIN
	SET @listCursor = CURSOR FORWARD_ONLY STATIC FOR
	SELECT 
		s.Id Id
	FROM @ids AS s
	WHERE s.Id > 0

	OPEN @listCursor;
END

GO 

DECLARE @ids UT_Ids;
INSERT INTO @ids VALUES (-1), (0), (1), (2);
DECLARE @MyCursor CURSOR;
EXEC GetItems @ids, @MyCursor OUTPUT;

DECLARE @id INT;
FETCH NEXT FROM @MyCursor INTO @id; 
WHILE (@@FETCH_STATUS = 0)  
BEGIN;  
	PRINT @id;
	FETCH NEXT FROM @MyCursor INTO @id; 
END;  
CLOSE @MyCursor;  
DEALLOCATE @MyCursor; 
```

## PIVOT
```
DROP TABLE IF EXISTS Employeesalarydetails;
CREATE TABLE Employeesalarydetails(
    Empid BIGINT,
    Component VARCHAR(100),
    Amount FLOAT,
);
INSERT 
    INTO Employeesalarydetails
    VALUES(2, 'Basic', 10000),
    (2, 'Hra', 1000),
    (2, 'TA', 750),
    (1, 'Basic', 20000),
    (1, 'Hra', 1000),
    (3, 'Basic', 6700),
    (3, 'Hra', 100),
    (4, 'Basic', 5000),
    (4, 'Hra', 1000);


SELECT *
    FROM Employeesalarydetails
    PIVOT(
        SUM(Amount)
            FOR Component
            IN([Basic],[Hra],[TA])
    )AS DtlPivot
```

## Leading Trailling 
```
DECLARE @defaultChar CHAR = '0', @defaultLen INT = 6, @inputValue VARCHAR(100) = 'xxx';
SELECT 
	--RIGHT(STUFF(@inputValue, 1, 0, REPLICATE(@defaultChar,@defaultLen)),@defaultLen) AddLeading,
	--LEFT(STUFF(@inputValue+' ', LEN(@inputValue)+1, 0, REPLICATE(@defaultChar,@defaultLen)),@defaultLen) AddTrailing;
	REPLICATE(@defaultChar, @defaultLen-LEN(@inputValue)) + @inputValue AddLeading,
	@inputValue + REPLICATE(@defaultChar, @defaultLen-LEN(@inputValue)) AddTrailing;
```

```
DECLARE @pattern NVARCHAR(1000) = '100000000001', @value VARCHAR(100) = 'xxx';
SELECT 
	SUBSTRING(@pattern, 0, LEN(@pattern) + 1 - LEN(@value)) + CAST(@value AS NVARCHAR),
	CAST(@value AS NVARCHAR) + SUBSTRING(@pattern, LEN(@value) + 1, LEN(@pattern))
```
## Pagination
```
DROP TABLE IF EXISTS [dbo].[Users]
GO
CREATE TABLE [dbo].[Users](
	[Id] [bigint] IDENTITY(1,1) NOT NULL PRIMARY KEY,
	[Name] [nvarchar](max) NULL,
	[Email] [nvarchar](max) NULL,
	[IsDeleted] BIT NOT NULL
)
GO
TRUNCATE TABLE Users
GO
DECLARE @count INT = 0;
WHILE @count < 100
BEGIN
	INSERT INTO Users([Name], [IsDeleted], [Email]) VALUES ('d', 0, 'd@gmail.com');
	SET @count = @count + 1;
END
GO
SELECT COUNT(*) FROM Users WITH(NOLOCK);
```

```
DECLARE @pageSize INT = 10, @pageNumber INT = 10;
WITH PagedData 
as
(
	SELECT 
		*,
		ROW_NUMBER() OVER (ORDER BY Id) AS RowNumber
	FROM Users
)
SELECT 
	*
FROM PagedData
WHERE RowNumber BETWEEN ((@pageNumber-1)*@pageSize)+1 AND @pageSize*@pageNumber;
```
```
DECLARE @pageSize INT = 10, @pageNumber INT = 10;
SELECT * 
FROM Users 
ORDER BY ID OFFSET @pageSize*(@pageNumber-1) ROWS FETCH NEXT @pageSize ROWS ONLY;
```

## ROW_NUMBER, RANK, DENSE_RANK

https://www.c-sharpcorner.com/blogs/difference-between-rownumber-rank-denserank-in-sql-server#:~:text=RANK()%20will%20assign%20the,without%20skipping%20the%20next%20number.

```
DROP TABLE IF EXISTS Employee;
GO
CREATE TABLE Employee  
(  
	Names VARCHAR(10),  
	Salary DECIMAL  
)  
GO
INSERT INTO dbo.Employee  
VALUES ('Rony',10000),('Joy',9000),('Devid',8000),('Warner',7000),('Elly',7000),('Frenil',6000), ('Tod',7000) 
GO  

SELECT   
	*,  
	ROW_NUMBER() OVER (ORDER BY Salary DESC) RowNumber,  
	RANK() OVER (ORDER BY Salary DESC) [Rank],  
	DENSE_RANK() OVER (ORDER BY Salary DESC) DenseRank 
FROM Employee
```
| Name | Salary | RowNumber | Rank | DenseRank |
|--|--|--|--|--|
|Rony	|10000	|1	|1	|1
|Joy		|9000	|2	|2	|2
|Devid	|8000	|3	|3	|3
|Warner	|7000	|4	|4	|4
|Elly	|7000	|5	|4	|4
|Tod		|7000	|6	|4	|4
|Frenil	|6000	|7	|7	|5

**DENSE_RANK alternative**
```
WITH UniquSalary
AS
(
	SELECT Salary FROM Employee GROUP BY Salary	
),
SalaryRank
AS
(
	SELECT 
		*,
		ROW_NUMBER() OVER (ORDER BY Salary DESC) RankNumber
	FROM UniquSalary
)
SELECT
	e.*,
	s.RankNumber
FROM Employee AS e
LEFT JOIN SalaryRank AS s ON e.Salary = s.Salary
ORDER BY s.RankNumber
```

**Use with partition**
```
DROP TABLE IF EXISTS Users;
CREATE TABLE Users
(
	Id INT,
	UserName VARCHAR(300),
	RoleCount INT,
);
INSERT INTO 
	Users (Id, UserName, RoleCount)
VALUES 
(1, 'AA', 0),
(2, 'Aa', 1),
(3, 'aa', 4),

(4, 'BB', 0),
(5, 'Bb', 1),
(6, 'bb', 4);
GO

SELECT 
	*,
	UPPER(LTRIM(RTRIM([UserName]))) as NormalizedUserName,
	ROW_NUMBER() OVER (PARTITION BY UPPER(LTRIM(RTRIM([UserName]))) ORDER BY RoleCount DESC) AS Sequance,
	DENSE_RANK() OVER (ORDER BY UPPER(LTRIM(RTRIM([UserName])))) AS GroupNo
FROM Users
```
| Id | UserName | RoleCount | NormalizedUserName | Sequance | GroupNo |
|--|--|--|--|--|--|
|3	|aa	|4	|AA	|1	|1
|2	|Aa	|1	|AA	|2	|1
|1	|AA	|0	|AA	|3	|1
|6	|bb	|4	|BB	|1	|2
|5	|Bb	|1	|BB	|2	|2
|4	|BB	|0	|BB	|3	|2

## PARTITION BY
https://www.sqlshack.com/sql-partition-by-clause-overview/

## ROW_NUMBER Without Column
```
IF object_id('tempdb..#tempUsers') IS NOT NULL
    DROP TABLE #tempUsers;
IF object_id('tempdb..#newTempUsers') IS NOT NULL
    DROP TABLE #newTempUsers;
	
CREATE TABLE #tempUsers
(
    Id INT,
    FirstName VARCHAR(MAX) NOT NULL,
	LastName VARCHAR(MAX) NULL
);
INSERT INTO #tempUsers (Id, FirstName, LastName)
VALUES 
(1, 'User', '1'),
(2, 'User', '2');
INSERT INTO #tempUsers
VALUES 
(3, 'User', '3');


SELECT 
	*,
	IDENTITY(INT, 1, 1) AS SelectedOrder
INTO #newTempUsers
FROM #tempUsers;
SELECT * FROM #newTempUsers;

SELECT 
	*,
	ROW_NUMBER() OVER(ORDER BY (SELECT 1)) AS SelectedOrder,	--1 can be NULL
	ROW_NUMBER() OVER(ORDER BY @@SPID) AS AnotherSelectedOrder	--@@SPID is process id
FROM #tempUsers;
```

## Multiple rows into a single row 
```
DECLARE @tblUserRoles TABLE (
	Id INT IDENTITY(1, 1),
	UserId INT,
	RoleId INT,
	RoleSource VARCHAR(50)
);
INSERT INTO 
@tblUserRoles (UserId, RoleId, RoleSource)
VALUES
(1, 1, 'User'),
(1, 2, 'Admin'),
(1, 3, 'Reseller'),
(2, 1, 'User'),
(3, 2, 'Admin'),
(4, 3, 'Reseller');
```
**Comma Separated**
```
SELECT 
	UserId, 
	COALESCE(STUFF((SELECT ',' + CONVERT(NVARCHAR(MAX), RoleId) 
			FROM @tblUserRoles AS AA
			WHERE AA.UserId = A.UserId
			ORDER BY RoleId
			FOR XML PATH(''), TYPE).value('.','VARCHAR(MAX)')		/*if change .value to .VALUE it will not work*/
			, 1, 1, ''), '') AS CommaSeparatedRoleIds
FROM @tblUserRoles A
GROUP BY UserId
```
**Additional Column**
```
SELECT 
	p.UserId, 
	MIN(CASE p.RoleSource WHEN 'User' THEN p.RoleId END) AS UserRoleId,
	MIN(CASE p.RoleSource WHEN 'Admin' THEN p.RoleId END) AS AdminRoleId,
	MIN(CASE p.RoleSource WHEN 'Reseller' THEN p.RoleId END) AS ResellerRoleId
FROM @tblUserRoles p 
WHERE p.RoleSource IN ('User', 'Admin', 'Reseller')
GROUP BY UserId
```
**First Row**
```
WITH LastResponse
AS
(
    SELECT 
		*,
		ROW_NUMBER() OVER(
			PARTITION BY UserId		/*group by*/
			ORDER BY Id DESC		/*for first try 'ASC'*/
		) AS OrderId
    FROM @tblUserRoles
)
SELECT *
    FROM LastResponse
    WHERE OrderId = 1;
```

## Table As Param
**With Procedure**
```
DROP PROCEDURE IF EXISTS GetItems;
DROP TYPE IF EXISTS UT_Ids;

GO
CREATE TYPE UT_Ids AS TABLE
(
	Id INT NULL
);
GO
CREATE OR ALTER PROCEDURE GetItems (
	@ids UT_Ids READONLY,
	@storeId INT
)
AS
BEGIN
	SELECT 
		s.Id Id,
		@storeId StoreId
	FROM @ids AS s
END

GO 
DECLARE @ids UT_Ids;
INSERT INTO @ids VALUES (1), (2);
EXEC GetItems @ids, 10;
```

## Function
**Scalar Valued**
```
DROP FUNCTION IF EXISTS GetName;

GO
CREATE FUNCTION GetName (
	@FirstName VARCHAR(MAX),
	@LastName VARCHAR(MAX)
) 
RETURNS VARCHAR(MAX) 
BEGIN
	RETURN @FirstName +' ' +@LastName;
END

GO
SELECT dbo.GetName('x', 'y') AS FullName;
```
```
DROP FUNCTION IF EXISTS GetName;

GO
CREATE FUNCTION GetName (
	@FirstName VARCHAR(MAX),
	@LastName VARCHAR(MAX)
) 
RETURNS VARCHAR(MAX)
BEGIN
	DECLARE @item VARCHAR(MAX);
	SET @item = @FirstName +' ' +@LastName;
	SET @item = LTRIM(RTRIM(@item));
	RETURN @item;
END

GO
SELECT dbo.GetName('x', 'y') AS FullName;
```

**Table Valued**
```
DROP TABLE IF EXISTS Games;
DROP FUNCTION IF EXISTS GetGames;

GO
CREATE TABLE Games(
	Id INT,
	[Name] VARCHAR(MAX),
	[TypeId] INT
);
GO
CREATE FUNCTION GetGames(
	@TypeId INT
) 
RETURNS TABLE
AS
RETURN (
	SELECT * FROM Games WHERE TypeId = @TypeId
)

GO
SELECT * FROM dbo.GetGames(1);
```
```
DROP TABLE IF EXISTS Games;
DROP FUNCTION IF EXISTS GetGames;

GO
CREATE TABLE Games(
	Id INT,
	[Name] VARCHAR(MAX),
	[TypeId] INT
);
GO
CREATE FUNCTION GetGames(
	@RuleId INT
) 
RETURNS @items TABLE (
	Id INT,
	[Name] VARCHAR(MAX),
	[TypeId] INT
)
AS
BEGIN
	IF @RuleId IN (10, 12)
	BEGIN
		INSERT INTO @items
		SELECT * FROM Games WHERE TypeId = @RuleId
	END
	ELSE
	BEGIN
		INSERT INTO @items
		SELECT * FROM Games
	END
	UPDATE @items SET [Name] = LTRIM(RTRIM([Name]));
	DELETE FROM @items WHERE Id > 15;
RETURN;     
END
GO
SELECT * FROM dbo.GetGames(1);
```

**User Type As Param**
```
DROP TABLE IF EXISTS Games;
DROP FUNCTION IF EXISTS GetGames;
DROP TYPE IF EXISTS UT_Ids;

GO
CREATE TYPE UT_Ids AS TABLE
(
	Id INT NULL
);
GO
CREATE TABLE Games(
	Id INT,
	[Name] VARCHAR(MAX),
	[TypeId] INT
);
GO
CREATE FUNCTION GetGames(
	@TypeIds UT_Ids READONLY
) 
RETURNS TABLE
AS
RETURN (
	SELECT * FROM Games WHERE TypeId IN (SELECT Id FROM @TypeIds)
)

GO
DECLARE @ids UT_Ids;
INSERT INTO @ids VALUES (1), (2);
SELECT * FROM dbo.GetGames(@ids);
```

## Variables
**@@VERSION**
```
SELECT @@VERSION
```

**@@SPID**
```
SELECT @@SPID	--process id, not session id

SELECT *
FROM sys.sysprocesses 
WHERE spid = @@SPID
```

**@@IDENTITY**
```
DECLARE @tableSource TABLE(
	Id INT IDENTITY(1, 1),
	[Name] VARCHAR(MAX),
	[CreatedOnUtc] DATETIME NOT NULL DEFAULT(GETDATE())
)

INSERT INTO @tableSource([Name]) VALUES ('x');
SELECT @@IDENTITY;	--1

INSERT INTO @tableSource([Name]) VALUES ('y'), ('z');
SELECT @@IDENTITY;	--3 id of z, last inserted id
```

**@@ROWCOUNT**
```
DECLARE @tableSource TABLE(
	Id INT IDENTITY(1, 1),
	[Name] VARCHAR(MAX),
	[CreatedOnUtc] DATETIME NOT NULL DEFAULT(GETDATE())
)
INSERT INTO @tableSource([Name]) VALUES ('y'), ('z');
```
```
INSERT INTO @tableSource([Name]) VALUES ('x');
PRINT @@ROWCOUNT;	--Inserted rows count

UPDATE @tableSource SET [Name] = 'x' WHERE [Name] = 'x';
PRINT @@ROWCOUNT;	--Updated rows count

DELETE FROM @tableSource WHERE [Name] = 'x';
PRINT @@ROWCOUNT;	--Deleted rows count
```
```
WHILE 1 = 1
BEGIN
	DELETE TOP (1)
	FROM @tableSource
	IF @@rowcount = 0
	BEGIN
		BREAK;
	END;
END;
```

**@@FETCH_STATUS**
```
DECLARE @id INT, @name VARCHAR(250);
DECLARE tblCoursor CURSOR FOR
	SELECT Id, UserName
	FROM [Users]  o
	WHERE o.AccountStatusId = 2;
OPEN tblCoursor
	FETCH NEXT FROM tblCoursor INTO @id, @name
	WHILE(@@FETCH_STATUS = 0)
	BEGIN
		PRINT CONCAT(@id, ' ', @name);
		FETCH NEXT FROM tblCoursor INTO @id, @name
	END
CLOSE tblCoursor
DEALLOCATE tblCoursor
```

**@@DBTS**
```
SELECT @@DBTS AS LastUsedRowVersion;
```

## tmpl
**item**
```
DROP TABLE IF EXISTS Users;
GO
CREATE TABLE Users
(
	Id INT,
	UserName VARCHAR(300),
	AccountStatusId INT,
	CreatedON DATETIME NULL
);
GO
INSERT INTO 
	Users (Id, UserName, AccountStatusId, CreatedON)
VALUES 
(1, 'A', 2, GETUTCDATE()),
(2, 'B', 2, DATEADD(MINUTE, -2, GETUTCDATE())),
(3, 'B', 2, '2021-12-20 14:34:03'),
(3, 'B', 2, '2021-12-20 10:34:03'),
(3, 'B', 2, '2021-12-21 00:00:00'),
(4, 'B', 2, NULL);
GO
DROP TABLE IF EXISTS Group_User
GO
CREATE TABLE Group_User
(
	GroupId INT NOT NULL,
	UserId INT NOT NULL
)
GO
DROP TABLE IF EXISTS GroupUserImage
GO
CREATE TABLE GroupUserImage
(
	Id INT NOT NULL,
	GroupId INT NOT NULL,
	UserId INT NOT NULL,
	ImageLink VARCHAR(MAX)
)
GO
```