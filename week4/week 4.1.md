# Week 4 DDL/ DML syntax
**Please make sure you have the DB browser for SQLite  installed** https://sqlitebrowser.org/dl/
## Step 1: CREATE TABLE
CREATE TABLE statement is used to create a new table in any of the given database. Creating a basic table involves naming the table and defining its columns and each column's data type.
A unique name for the table needs to follow the CREATE TABLE statement. 

```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```
## Step 2: DROP TABLE
DROP TABLE statement is used to remove a table definition and all associated data, indexes, triggers, constraints, and permission specifications for that table.

**You must be careful while using this command because once a table is deleted then all the information available in the table would also be lost forever.**
```sql
DROP TABLE COMPANY;
```
:red_circle: **Please re-run step 1 before proceeding**
## Step 3: INSERT INTO
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
## Step 4: SELECT STATEMENT
This is categorized as a DQL but is required to inspect data in any table. Hence this is being talked about here ahead of time
SELECT statement is used to fetch the data from a SQLite database table which returns data in the form of a result table. These result tables are also called result sets

```sql
SELECT * FROM COMPANY;
```
### WHERE CLAUSE
SQLite WHERE clause is used to specify a condition while fetching the data from one table or multiple tables.
If the given condition is satisfied, means true, then it returns the specific value from the table. You will have to use WHERE clause to filter the records and fetching only necessary records.
The WHERE clause not only is used in SELECT statement, but it is also used in UPDATE, DELETE statement, etc., which will be covered in subsequent chapters.
Examples:

_Step 5: Find all the records where AGE is greater than or equal to 25 AND salary is greater than or equal to 65000.00_

```sql
SELECT * FROM COMPANY WHERE AGE >= 25 AND SALARY >= 65000;
```
_Step 6: Find all the records where AGE is not NULL, which means all the records because none of the record has AGE equal to NULL_

```sql
SELECT * FROM COMPANY WHERE AGE IS NOT NULL;
```
## Step 7: UPDATE
UPDATE Query is used to modify the existing records in a table. You can use WHERE clause with UPDATE query to update selected rows, otherwise all the rows would be updated
E.g. Update ADDRESS for a customer whose ID is 6
```sql
UPDATE COMPANY SET ADDRESS = 'Texas' WHERE ID = 6;
```
To modify ALL rows of data with a single value you do not need to use WHERE clause and UPDATE query. E.g. ADDRESS and SALARY column values in COMPANY table,
```sql
UPDATE COMPANY SET ADDRESS = 'Texas', SALARY = 20000.00;
```
## Step 8: 
After executing the statement, the transaction is open until it is explicitly committed or rolled back e.g.  SQL statements to select or update data in the database. Note that the change is only visible to the current session (or client).
To make the changes permanent to the database use COMMIT 
```sql
COMMIT;
```
## Step 9: DELETE
DELETE Query is used to delete the existing records from a table. You can use WHERE clause with DELETE query to delete the selected rows, otherwise all the records would be deleted
 ```sql
 DELETE FROM COMPANY WHERE ID = 7;
 ```
 DELETING without a WHERE statement deletes the content of the table
 ```sql
 DELETE FROM COMPANY;
 ```
 :red_circle: **Repeat step 1 and then 3-8**
 ## ALTER
ALTER TABLE command modifies an existing table without performing a full dump and reload of the data. You can rename a table using ALTER TABLE statement and additional columns can be added in an existing table using ALTER TABLE statement.
ALTER TABLE command can be farily advanced; but in SQLite except renaming a table and adding a column in an existing table

### Step 10: Rename Table
```sql
ALTER TABLE COMPANY RENAME TO EMPLOYEE;
```
### Step 11: Add a column
```sql
ALTER TABLE EMPLOYEE ADD COLUMN GENDER char(1);
```

## CONSTRAINTS
SQLite CHECK constraints allow you to define expressions to test values whenever they are inserted into or updated within a column. If the values do not meet the criteria defined by the expression, SQLite will issue a constraint violation and abort the statement. The CHECK constraints allow you to define additional data integrity checks beyond UNIQUE or NOT NULL to suit your specific application. SQLite allows you to define a CHECK constraint at the column level or the table level.

### Step 12: Column level check constraint example
```sql
CREATE TABLE CUSTOMER (
    customerID INTEGER PRIMARY KEY,
    first_name TEXT    NOT NULL,
    last_name  TEXT    NOT NULL,
    email      TEXT,
    phone      TEXT    NOT NULL
                    CHECK (length(phone) >= 10) 
);
```
The phone number has a check constraint. This CHECK constraint ensures that the values in the phone column must be at least 10 characters.

### Step 13: Test CHECK Constraint

```sql
INSERT INTO CUSTOMER(first_name, last_name, phone)
VALUES('John','Doe','408123456');
```
**What happens?**

Now try...

```sql
INSERT INTO CUSTOMER(first_name, last_name, phone)
VALUES('John','Doe','(408)-123-456');

INSERT INTO CUSTOMER(first_name, last_name, phone)
VALUES('Jane','Smith','(408)-987-123');
```

| :memo:        |If a table contains a column of type INTEGER PRIMARY KEY, then that column becomes an alias for the ROWID. You can then access the ROWID using any of four different names, the original three names described above or the name given to the INTEGER PRIMARY KEY column. All these names are aliases for one another and work equally well in any context.      |
|---------------|:------------------------|

