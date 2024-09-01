# PostgreSQL Cheat Sheet

## Table of Contents
1. [Connection](#connection)
2. [Database Operations](#database-operations)
3. [Table Operations](#table-operations)
4. [Data Manipulation](#data-manipulation)
5. [Querying Data](#querying-data)
6. [Joins](#joins)
7. [Indexing](#indexing)
8. [Transactions](#transactions)
9. [Users and Permissions](#users-and-permissions)
10. [Backup and Restore](#backup-and-restore)
11. [Performance Tuning](#performance-tuning)

## Connection

Connect to PostgreSQL:
```
psql -U username -d database_name
```

Connect to a remote server:
```
psql -h hostname -U username -d database_name
```

## Database Operations

List all databases:
```sql
\l
```

Create a new database:
```sql
CREATE DATABASE database_name;
```

Switch to a different database:
```sql
\c database_name
```

Drop a database:
```sql
DROP DATABASE database_name;
```

## Table Operations

List all tables:
```sql
\dt
```

Create a new table:
```sql
CREATE TABLE table_name (
    column1 datatype1,
    column2 datatype2,
    ...
);
```

Describe table structure:
```sql
\d table_name
```

Alter table:
```sql
ALTER TABLE table_name ADD COLUMN new_column datatype;
```

Drop table:
```sql
DROP TABLE table_name;
```

## Data Manipulation

Insert data:
```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

Update data:
```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

Delete data:
```sql
DELETE FROM table_name WHERE condition;
```

## Querying Data

Select all columns:
```sql
SELECT * FROM table_name;
```

Select specific columns:
```sql
SELECT column1, column2 FROM table_name;
```

Filter data:
```sql
SELECT * FROM table_name WHERE condition;
```

Sort data:
```sql
SELECT * FROM table_name ORDER BY column1 ASC, column2 DESC;
```

Limit results:
```sql
SELECT * FROM table_name LIMIT 10;
```

Aggregate functions:
```sql
SELECT COUNT(*), AVG(column_name), SUM(column_name)
FROM table_name;
```

Group by:
```sql
SELECT column1, COUNT(*)
FROM table_name
GROUP BY column1;
```

## Joins

Inner join:
```sql
SELECT *
FROM table1
INNER JOIN table2 ON table1.column = table2.column;
```

Left join:
```sql
SELECT *
FROM table1
LEFT JOIN table2 ON table1.column = table2.column;
```

Right join:
```sql
SELECT *
FROM table1
RIGHT JOIN table2 ON table1.column = table2.column;
```

Full outer join:
```sql
SELECT *
FROM table1
FULL OUTER JOIN table2 ON table1.column = table2.column;
```

## Indexing

Create index:
```sql
CREATE INDEX index_name ON table_name (column_name);
```

Create unique index:
```sql
CREATE UNIQUE INDEX index_name ON table_name (column_name);
```

Drop index:
```sql
DROP INDEX index_name;
```

## Transactions

Start a transaction:
```sql
BEGIN;
```

Commit a transaction:
```sql
COMMIT;
```

Rollback a transaction:
```sql
ROLLBACK;
```

## Users and Permissions

Create a new user:
```sql
CREATE USER username WITH PASSWORD 'password';
```

Grant privileges:
```sql
GRANT ALL PRIVILEGES ON DATABASE database_name TO username;
```

Revoke privileges:
```sql
REVOKE ALL PRIVILEGES ON DATABASE database_name FROM username;
```

## Backup and Restore

Backup a database:
```
pg_dump database_name > backup_file.sql
```

Restore a database:
```
psql database_name < backup_file.sql
```

## Performance Tuning

Explain query plan:
```sql
EXPLAIN ANALYZE SELECT * FROM table_name WHERE condition;
```

Vacuum (clean up and optimize):
```sql
VACUUM ANALYZE table_name;
```

Reindex:
```sql
REINDEX TABLE table_name;
```

This cheat sheet covers the essential operations you need to work with PostgreSQL. Remember to refer to the official PostgreSQL documentation for more detailed information and advanced features.
