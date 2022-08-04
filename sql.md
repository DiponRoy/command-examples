- Basic https://www.w3schools.com/sql/sql_constraints.asp
- Unique Indexes vs Unique Constraints https://www.mssqltips.com/sqlservertip/4270/difference-between-sql-server-unique-indexes-and-unique-constraints/
- DateTime conversion number https://www.mssqltips.com/sqlservertip/1145/date-and-time-conversions-using-sql-server/

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

## OUTPUT 
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
```
**With Merge**
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

## tmpl
**item**
```
DROP TABLE IF EXISTS Users;
CREATE TABLE Users
(
	Id INT,
	UserName VARCHAR(300),
	AccountStatusId INT,
	CreatedON DATETIME NULL
);
INSERT INTO 
	Users (Id, UserName, AccountStatusId, CreatedON)
VALUES 
(1, 'A', 2, GETUTCDATE()),
(2, 'B', 2, DATEADD(MINUTE, -2, GETUTCDATE())),
(3, 'B', 2, '2021-12-20 14:34:03'),
(3, 'B', 2, '2021-12-20 10:34:03'),
(3, 'B', 2, '2021-12-21 00:00:00'),
(4, 'B', 2, NULL);
SELECT COUNT(1) FROM Users;
```
