# MySQL Syntax
Some of The Most Important SQL Commands
``` sql
SELECT - extracts data from a database
UPDATE - updates data in a database
DELETE - deletes data from a database
INSERT INTO - inserts new data into a database
CREATE DATABASE - creates a new database
ALTER DATABASE - modifies a database
CREATE TABLE - creates a new table
ALTER TABLE - modifies a table
DROP TABLE - deletes a table
CREATE INDEX - creates an index (search key)
DROP INDEX - deletes an index
```

## 1. SELECT
``` sql
SELECT column1, column2, ...
FROM table_name;
```
``` sql
SELECT * FROM table_name;
```
#### EX:
``` sql
SELECT CustomerName, City, Country FROM Customers;
```
``` sql
SELECT * FROM Customers;
```
### SELECT DISTINCT Statement
The SELECT DISTINCT statement is used to return only distinct (different) values.

Inside a table, a column often contains many duplicate values; and sometimes you only want to list the different (distinct) values.

SELECT DISTINCT Syntax
``` sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```
