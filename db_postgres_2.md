# PostgreSQL Database Relationships Tutorial

## Introduction

This tutorial covers the four main types of relationships in relational databases:

1. One-to-One (1:1)
2. One-to-Many (1:N)
3. Many-to-Many (M:N)
4. Many-to-One (N:1)

We'll explore each relationship type using PostgreSQL, providing examples and SQL queries for creating tables, inserting data, and querying related data.

## 1. One-to-One (1:1) Relationship

A one-to-one relationship exists when one record in a table is associated with exactly one record in another table.

### Example: User and Passport

Let's create tables for users and their passports:

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL
);

CREATE TABLE passports (
    passport_id SERIAL PRIMARY KEY,
    user_id INTEGER UNIQUE,
    passport_number VARCHAR(20) NOT NULL,
    expiry_date DATE NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```

Insert some sample data:

```sql
INSERT INTO users (username, email) VALUES
('john_doe', 'john@example.com'),
('jane_smith', 'jane@example.com');

INSERT INTO passports (user_id, passport_number, expiry_date) VALUES
(1, 'AB123456', '2025-12-31'),
(2, 'CD789012', '2026-06-30');
```

Query to retrieve user and passport information:

```sql
SELECT u.username, u.email, p.passport_number, p.expiry_date
FROM users u
JOIN passports p ON u.user_id = p.user_id;
```

## 2. One-to-Many (1:N) Relationship

A one-to-many relationship exists when one record in a table can be associated with multiple records in another table.

### Example: Author and Books

Let's create tables for authors and their books:

```sql
CREATE TABLE authors (
    author_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    birth_date DATE
);

CREATE TABLE books (
    book_id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    publication_year INTEGER,
    author_id INTEGER,
    FOREIGN KEY (author_id) REFERENCES authors(author_id)
);
```

Insert some sample data:

```sql
INSERT INTO authors (name, birth_date) VALUES
('J.K. Rowling', '1965-07-31'),
('George Orwell', '1903-06-25');

INSERT INTO books (title, publication_year, author_id) VALUES
('Harry Potter and the Philosopher''s Stone', 1997, 1),
('Harry Potter and the Chamber of Secrets', 1998, 1),
('1984', 1949, 2),
('Animal Farm', 1945, 2);
```

Query to retrieve authors and their books:

```sql
SELECT a.name, b.title, b.publication_year
FROM authors a
LEFT JOIN books b ON a.author_id = b.author_id
ORDER BY a.name, b.publication_year;
```

## 3. Many-to-Many (M:N) Relationship

A many-to-many relationship exists when multiple records in one table can be associated with multiple records in another table.

### Example: Students and Courses

Let's create tables for students, courses, and their enrollments:

```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE courses (
    course_id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    credits INTEGER NOT NULL
);

CREATE TABLE enrollments (
    enrollment_id SERIAL PRIMARY KEY,
    student_id INTEGER,
    course_id INTEGER,
    enrollment_date DATE NOT NULL,
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id),
    UNIQUE (student_id, course_id)
);
```

Insert some sample data:

```sql
INSERT INTO students (name, email) VALUES
('Alice Johnson', 'alice@example.com'),
('Bob Williams', 'bob@example.com');

INSERT INTO courses (title, credits) VALUES
('Introduction to Computer Science', 3),
('Database Management', 4),
('Web Development', 3);

INSERT INTO enrollments (student_id, course_id, enrollment_date) VALUES
(1, 1, '2023-09-01'),
(1, 2, '2023-09-01'),
(2, 2, '2023-09-02'),
(2, 3, '2023-09-02');
```

Query to retrieve students and their enrolled courses:

```sql
SELECT s.name, c.title, e.enrollment_date
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id
ORDER BY s.name, c.title;
```

## 4. Many-to-One (N:1) Relationship

A many-to-one relationship is similar to a one-to-many relationship, but viewed from the opposite perspective. Multiple records in one table are associated with a single record in another table.

### Example: Employees and Departments

Let's create tables for employees and their departments:

```sql
CREATE TABLE departments (
    department_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    location VARCHAR(100)
);

CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    hire_date DATE NOT NULL,
    department_id INTEGER,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
```

Insert some sample data:

```sql
INSERT INTO departments (name, location) VALUES
('HR', 'New York'),
('IT', 'San Francisco'),
('Marketing', 'Chicago');

INSERT INTO employees (name, hire_date, department_id) VALUES
('John Doe', '2022-01-15', 1),
('Jane Smith', '2022-03-01', 1),
('Mike Johnson', '2022-02-10', 2),
('Sarah Brown', '2022-04-05', 2),
('Tom Wilson', '2022-05-20', 3);
```

Query to retrieve employees and their departments:

```sql
SELECT e.name AS employee_name, e.hire_date, d.name AS department_name, d.location
FROM employees e
JOIN departments d ON e.department_id = d.department_id
ORDER BY d.name, e.name;
```

## Conclusion

This tutorial covered the four main types of database relationships: one-to-one, one-to-many, many-to-many, and many-to-one. We provided examples and SQL queries for PostgreSQL to illustrate how to create tables, insert data, and query related information for each relationship type.

Understanding these relationships is crucial for designing efficient and effective database schemas. By properly structuring your data and relationships, you can ensure data integrity, reduce redundancy, and simplify complex queries in your database applications.


# PostgreSQL SQL Joins Tutorial

## Introduction

This tutorial covers the various types of SQL joins in PostgreSQL:

1. INNER JOIN
2. LEFT JOIN (LEFT OUTER JOIN)
3. RIGHT JOIN (RIGHT OUTER JOIN)
4. FULL JOIN (FULL OUTER JOIN)
5. CROSS JOIN
6. SELF JOIN

We'll explore each join type with examples and SQL queries using PostgreSQL.

## Setting Up Sample Data

First, let's create some sample tables to use throughout this tutorial:

```sql
-- Create Departments table
CREATE TABLE departments (
    dept_id SERIAL PRIMARY KEY,
    dept_name VARCHAR(50) NOT NULL
);

-- Create Employees table
CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    dept_id INTEGER,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);

-- Insert sample data into Departments
INSERT INTO departments (dept_name) VALUES
('HR'),
('IT'),
('Finance'),
('Marketing');

-- Insert sample data into Employees
INSERT INTO employees (first_name, last_name, dept_id) VALUES
('John', 'Doe', 1),
('Jane', 'Smith', 2),
('Mike', 'Johnson', 2),
('Emily', 'Brown', 3),
('David', 'Wilson', NULL),
('Sarah', 'Lee', 4);
```

## 1. INNER JOIN

An INNER JOIN returns only the rows that have matching values in both tables.

### Example:

```sql
SELECT e.emp_id, e.first_name, e.last_name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;
```

This query will return all employees who have a department assigned, along with their department names.

## 2. LEFT JOIN (LEFT OUTER JOIN)

A LEFT JOIN returns all rows from the left table and the matched rows from the right table. If there's no match, NULL values are returned for the right table's columns.

### Example:

```sql
SELECT e.emp_id, e.first_name, e.last_name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id;
```

This query will return all employees, including those without a department assigned (like David Wilson).

## 3. RIGHT JOIN (RIGHT OUTER JOIN)

A RIGHT JOIN returns all rows from the right table and the matched rows from the left table. If there's no match, NULL values are returned for the left table's columns.

### Example:

```sql
SELECT e.emp_id, e.first_name, e.last_name, d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.dept_id;
```

This query will return all departments, including those without any employees assigned.

## 4. FULL JOIN (FULL OUTER JOIN)

A FULL JOIN returns all rows when there's a match in either the left or right table. If there's no match, NULL values are returned for the columns of the table without a match.

### Example:

```sql
SELECT e.emp_id, e.first_name, e.last_name, d.dept_name
FROM employees e
FULL JOIN departments d ON e.dept_id = d.dept_id;
```

This query will return all employees and all departments, regardless of whether there's a match between them.

## 5. CROSS JOIN

A CROSS JOIN returns the Cartesian product of both tables, i.e., each row from the first table is combined with each row from the second table.

### Example:

```sql
SELECT e.first_name, e.last_name, d.dept_name
FROM employees e
CROSS JOIN departments d;
```

This query will return all possible combinations of employees and departments, regardless of their actual associations.

## 6. SELF JOIN

A SELF JOIN is used to join a table with itself. This is useful when you want to compare rows within the same table.

### Example:

Let's add a 'manager_id' column to our employees table to demonstrate a self join:

```sql
ALTER TABLE employees ADD COLUMN manager_id INTEGER;

UPDATE employees SET manager_id = 1 WHERE emp_id IN (2, 3);
UPDATE employees SET manager_id = 4 WHERE emp_id IN (5, 6);
```

Now, let's use a self join to find employees and their managers:

```sql
SELECT e.first_name AS employee_first_name, 
       e.last_name AS employee_last_name,
       m.first_name AS manager_first_name, 
       m.last_name AS manager_last_name
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id;
```

This query will return all employees along with their manager's name (if they have a manager).

## Conclusion

SQL joins are powerful tools for combining data from multiple tables. Each type of join serves a specific purpose:

- INNER JOIN: Use when you want only the rows that have matches in both tables.
- LEFT JOIN: Use when you want all rows from the left table, regardless of matches in the right table.
- RIGHT JOIN: Use when you want all rows from the right table, regardless of matches in the left table.
- FULL JOIN: Use when you want all rows from both tables, regardless of matches.
- CROSS JOIN: Use when you need all possible combinations of rows from two tables.
- SELF JOIN: Use when you need to compare rows within the same table.

Understanding and using these joins effectively will allow you to retrieve and analyze complex data relationships in your PostgreSQL databases.

