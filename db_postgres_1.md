# PostgreSQL DB Creation Tutorial

## Introduction

This tutorial covers the various types of SQL joins in PostgreSQL:

1. CREATE DATABASE
2. CREATE USER
3. GRANT PRIVILEGES
4. CRETE SCHEMA
5. SET SEARCH PATH (OPTIONAL)

We'll explore each join type with examples and SQL queries using PostgreSQL.

## Creating Database, Create User and Grant Ptivileges
```sql
CREATE DATABASE "angshu-db"; -- if you want '-' in the name, use ""
CREATE USER angshu WITH ENCRYPTED PASSWORD 'Angshu!123';
GRANT ALL PRIVILEGES ON DATABASE "angshu-db" to angshu;
```

## Creating Schema and Set Search Path 9Optional)
```sql
CREATE SCHEMA angshuschema
SET SEARCH_PATH = angshuschema; -- temp change to default schema
ALTER DATABASE "angshu-db" SET search_path TO angshuschema; -- parmanent change to default schema
```
