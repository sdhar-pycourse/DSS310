# Week 4 DDL/ DML syntax
## CREATE TABLE
CREATE TABLE statement is used to create a new table in any of the given database. Creating a basic table involves naming the table and defining its columns and each column's data type.
A unique name for the table needs to follow the CREATE TABLE statement. You can specify _database_nam_e along with _table_name_

```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```
## DROP TABLE
DROP TABLE statement is used to remove a table definition and all associated data, indexes, triggers, constraints, and permission specifications for that table.

**You must be careful while using this command because once a table is deleted then all the information available in the table would also be lost forever.**
```sql
DROP TABLE COMPANY;
```
## INSERT INTO
SQLite INSERT INTO Statement is used to add new rows of data into a table. 
You may not need to specify the column(s) name in the SQLite query if you are adding values for all the columns of the table. However, make sure the order of the values is in the same order as the columns in the table.
```sql
INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (1, 'Paul', 32, 'California', 20000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (2, 'Allen', 25, 'Texas', 15000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (3, 'Teddy', 23, 'Norway', 20000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (5, 'David', 27, 'Texas', 85000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (6, 'Kim', 22, 'South-Hall', 45000.00 );
```

```sql
```

```

```
