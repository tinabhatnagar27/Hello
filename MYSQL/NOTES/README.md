# MySQL Basics 

This document covers **basic MySQL concepts** including Database, Query, Table creation, and common SQL operations. It is suitable for **beginners, exams, and practice**.

---

## What is a Database?

A **database** is an organized collection of data that is stored electronically and can be easily accessed, managed, and updated.

### Examples:

* Student records
* Bank details
* User login information

---

## What is MySQL?

MySQL is an **open-source Relational Database Management System (RDBMS)** that uses **SQL (Structured Query Language)** to manage data stored in tables.

---

##  What is a Query?

A **query** is a request or command given to the database to **read, insert, update, or delete data**.

### Example:

```sql
SELECT * FROM students;
```

---

##  Basic MySQL Commands

###  Create Database

```sql
CREATE DATABASE college;
```

###  Use Database

```sql
USE college;
```

###  Create Table

```sql
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT,
    course VARCHAR(50),
    marks INT
);
```

---

### Insert Data

```sql
INSERT INTO students (name, age, course, marks)
VALUES ('Tina', 22, 'MCA', 85);
```

---

### Read Data (SELECT)

```sql
SELECT * FROM students;
```

---

### Update Data

```sql
UPDATE students
SET marks = 90
WHERE name = 'Tina';
```

---

### Delete Data

```sql
DELETE FROM students
WHERE id = 1;
```

---

## Useful MySQL Commands

```sql
SHOW DATABASES;
SHOW TABLES;
DESCRIBE students;
```

---

##  CRUD Operations

| Operation | SQL Command |
| --------- | ----------- |
| Create    | INSERT      |
| Read      | SELECT      |
| Update    | UPDATE      |
| Delete    | DELETE      |

---

##  Common Error & Fix

### Error:

```text
Table 'sys.users' doesn't exist
```

### Reason:

* Wrong database selected
* Table name does not exist

### Fix:

```sql
USE college;
SELECT * FROM students;
```

---

##  Best Practice

* Do NOT create tables in `sys` database
* Always create and use your own database

---

##  Conclusion

MySQL is widely used in backend development to store and manage structured data efficiently. Understanding databases, queries, and CRUD operations is essential for any developer.

---

