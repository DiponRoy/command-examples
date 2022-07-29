
# Db
<p align="left">
    <a href="https://www.microsoft.com/en-us/sql-server/sql-server-downloads" target="_blank"><img src="https://cdn.iconscout.com/icon/free/png-512/sql-4-190807.png" alt="microsoftsqlserver" width="40" height="40" /></a>    
    <a href="https://www.oracle.com/index.html" target="_blank"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/oracle/oracle-original.svg" alt="oracle" width="40" height="40" /> </a>
    <a href="https://www.postgresql.org" target="_blank"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/postgresql/postgresql-original-wordmark.svg" alt="postgresql" width="40" height="40" /> </a>
    <a href="https://www.mysql.com/" target="_blank"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/mysql/mysql-original-wordmark.svg" alt="mysql" width="40" height="40" /> </a>   
    <a href="https://www.w3schools.com/sql/" target="_blank"> <img src="https://hackr.io/tutorials/sql/logo-sql.svg?ver=1555309685" alt="sql" width="40" height="40" /> </a> 
</p>



**SQL Server**
 - https://github.com/ktaranov/sqlserver-kit/blob/master/SQL%20Server%20Name%20Convention%20and%20T-SQL%20Programming%20Style.md
 - https://www.isbe.net/documents/sql_server_standards.pdf

**Oracle**
 - https://oracle-base.com/articles/misc/naming-conventions
 - https://ss64.com/ora/syntax-naming.html

**PostgreSQL**   
 - u
 
**MySQL**
 - u



| Object | SQL Server | Oracle  | PostgreSQL | MySQL |
|---|---|---|---|---|
| Db  | KCRM  |   |   |   |
| Schema  | dbo  |   |   |   |
| Table | Student|  STUDENT | student | student |
| Table | StudentDetail |  STUDENT_DETAIL | student_detail | student_detail |
| Column | Id | ID | id |id |
| Column | StudentId | STUDENT_ID | sutent_id |student_id |
| Column | UserName | USER_NAME | user_name |user_name |
| PK  |   |   |   |   |
| FK  |   |   |   |   |
| SP  |   |   |   |   |
| Index  |   |   |   |   |
| View  |   |   |   |   |
| Function  |   |   |   |   |
| Trigger  |   |   |   |   |
| Variable  |   |   |   |   |
| Teamp Table  |   |   |   |   |
|   |   |   |   |   |
|   |   |   |   |   |
|   |   |   |   |   |
|   |   |   |   |   |

**Common Columns**
 - Id 
 - DataSource 
 - IsActive 
 - CreatedAtUtc 
 - CreatedBy 
 - UpdatedAtUtc 
 - UpdatedBy
 - DeletedAtUtc 
 - DeletedBy

**Style of Writing**
 - camelCase
 - PascalCase
 - snake_case
 - MACRO_CASE
 
 https://stackoverflow.com/questions/17326185/what-are-the-different-kinds-of-cases
```
+--------------------------+-------------------------------------------------------------+
| Formatting               | Name(s)                                                     |
+--------------------------+-------------------------------------------------------------|
| namingidentifier         | flat case/Lazy Case                                         |
| NAMINGIDENTIFIER         | upper flat case                                             |
| namingIdentifier         | (lower) camelCase, dromedaryCase                            |
| NamingIdentifier         | (upper) CamelCase, PascalCase, StudlyCase, CapitalCamelCase |
| naming_identifier        | snake_case, snake_case, pothole_case, C Case                |
| Naming_Identifier        | Camel_Snake_Case                                            |
| NAMING_IDENTIFIER        | SCREAMING_SNAKE_CASE, MACRO_CASE, UPPER_CASE, CONSTANT_CASE |
| naming-identifier        | Kebab Case/caterpillar-case/dash-case, hyphen-case,         |
|                          | lisp-case, spinal-case and css-case                         |
| NAMING-IDENTIFIER        | TRAIN-CASE, COBOL-CASE, SCREAMING-KEBAB-CASE                |
| Naming-Identifier        | Train-Case, HTTP-Header-Case                                |
| _namingIdentifier        | Undercore Notation (prefixed by "_" followed by camelCase   |
| datatypeNamingIdentifier | Hungarian Notation (variable names Prefixed by metadata     |
|                          | data-types which is out-dated)                              |
|--------------------------+-------------------------------------------------------------+
```
