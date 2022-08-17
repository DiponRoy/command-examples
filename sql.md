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
```

**DateTime to String**
```
DECLARE @dateTime DATETIME = GETDATE();
SELECT 
	CONVERT(VARCHAR, @dateTime, 23),  --yyyy-mm-dd
	CONVERT(VARCHAR, @dateTime, 8),   --hh:mm:ss
	RIGHT(STUFF(SUBSTRING(CONVERT(VARCHAR, @dateTime, 22), 11, LEN(CONVERT(VARCHAR, @dateTime, 22))-1), 1, 0, replicate('0',11)),11),	--hh:mm:ss AM/PM
	CONVERT(VARCHAR, @dateTime, 120),  --yyyy-mm-dd hh:mm:ss
	CONVERT(VARCHAR, @dateTime, 121)   --yyyy-mm-dd hh:mm:ss:nnn
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
