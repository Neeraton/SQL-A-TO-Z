# Core SQL Concepts 

**SQL (Structured Query Language)** is the standard language for interacting with **relational databases**. It allows users to **create**, **retrieve**, **update**, and **delete** data efficiently. Below is a breakdown of its most fundamental concepts with clear explanations on their meaning and how they work.

---

## 1. Databases and Tables

### **Database**

A **database** is a structured collection of data. You can think of it as a container that stores related sets of data in an organized way. For instance, a company's system might include a database for **employees**, **products**, and **sales**.

### **Table**

A **table** is the basic unit of storage within a database, consisting of **columns** and **rows**.

* **Columns (Fields/Attributes)**: Define the type and name of the data being stored. Each column has a specific **data type** (e.g., `INT`, `VARCHAR`, `DATE`).
* **Rows (Records/Tuples)**: Represent individual entries in the table. Each row corresponds to a single data item with values for each column.

#### 🔹 Example Table: Employees

| EmployeeID | FirstName | LastName | DepartmentID | HireDate   |
| ---------- | --------- | -------- | ------------ | ---------- |
| 101        | Alice     | Smith    | 1            | 2022-03-15 |
| 102        | Bob       | Johnson  | 2            | 2021-07-20 |
| 103        | Carol     | Williams | 1            | 2023-01-10 |

---

## 2. SQL Commands (Sublanguages)

SQL commands are organized into **sublanguages**, each serving a specific purpose.

---

### a. Data Definition Language (DDL)

DDL commands are used to **define** and **modify** the structure (schema) of a database.

#### 🔹 `CREATE`

Creates new databases, tables, indexes, views, etc.

```sql
CREATE DATABASE CompanyDB;
CREATE TABLE Employees (
    EmployeeID INT,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    HireDate DATE
);
```

#### 🔹 `ALTER`

Modifies an existing table structure (e.g., add, remove, or modify columns).

```sql
ALTER TABLE Employees ADD Email VARCHAR(100);
ALTER TABLE Employees MODIFY COLUMN HireDate DATETIME;
ALTER TABLE Employees DROP COLUMN Email;
```

#### 🔹 `DROP`

Deletes a database or table permanently.

```sql
DROP TABLE Employees;
DROP DATABASE CompanyDB;
```

#### 🔹 `TRUNCATE`

Deletes all rows in a table but keeps the structure intact.

```sql
TRUNCATE TABLE Employees;
```

🛠 **Working**: DDL commands affect the schema. Once executed, most DDL changes are auto-committed, meaning they're permanent and can't be rolled back in some systems.

---

### b. Data Manipulation Language (DML)

DML is used to **insert**, **update**, **retrieve**, and **delete** actual data in database tables.

#### 🔹 `SELECT`

Retrieves data from a table.

```sql
SELECT FirstName, LastName FROM Employees;
SELECT * FROM Employees;
```

#### 🔹 `INSERT`

Adds new rows (records) into a table.

```sql
INSERT INTO Employees (EmployeeID, FirstName, LastName, HireDate)
VALUES (104, 'David', 'Clark', '2023-06-01');
```

#### 🔹 `UPDATE`

Modifies existing records.

```sql
UPDATE Employees
SET LastName = 'Brown'
WHERE EmployeeID = 101;
```

#### 🔹 `DELETE`

Removes existing records from the table.

```sql
DELETE FROM Employees WHERE EmployeeID = 103;
```

🛠 **Working**: DML operations are not automatically saved. You need to use TCL (like `COMMIT`) to make them permanent.

---

### c. Data Query Language (DQL)

DQL is primarily concerned with querying data.

#### 🔹 `SELECT`

Used to fetch data according to conditions.

```sql
SELECT * FROM Employees WHERE DepartmentID = 1;
```

🛠 **Working**: Internally, the SQL engine parses, optimizes, and executes the query plan to retrieve only the matching records.

---

### d. Data Control Language (DCL)

DCL is used to **grant** and **revoke** permissions to users.

#### 🔹 `GRANT`

Gives specified privileges to users.

```sql
GRANT SELECT, INSERT ON Employees TO 'username';
```

#### 🔹 `REVOKE`

Removes previously granted privileges.

```sql
REVOKE SELECT, INSERT ON Employees FROM 'username';
```

🛠 **Working**: These commands control **security** and **access** to database objects.

---

### e. Transaction Control Language (TCL)

TCL manages **transactions**—a group of SQL operations treated as a single logical unit of work.

#### 🔹 `COMMIT`

Saves all changes made during the current transaction.

```sql
COMMIT;
```

#### 🔹 `ROLLBACK`

Reverts all changes since the last COMMIT.

```sql
ROLLBACK;
```

#### 🔹 `SAVEPOINT`

Creates a point within a transaction to which you can later roll back.

```sql
SAVEPOINT SavePoint1;
ROLLBACK TO SavePoint1;
```

🛠 **Working**:

* **Transactions** ensure that either all operations succeed or none do (atomicity).
* **COMMIT** finalizes changes.
* **ROLLBACK** undoes changes.
* **SAVEPOINT** provides partial rollback control.

---

## 3. Clauses

**Clauses** are keywords in SQL that provide additional functionality to SQL commands, especially `SELECT`.

### 🔹 FROM

Specifies the table(s) to retrieve data from.
*(Used with SELECT, DELETE, UPDATE)*

---

### 🔹 WHERE

Filters records based on a specified condition.

```sql
SELECT column1 FROM table_name WHERE column2 > 100;
```

---

### 🔹 ORDER BY

Sorts the result set in ascending (`ASC`) or descending (`DESC`) order based on one or more columns.

```sql
SELECT column1, column2 FROM table_name ORDER BY column1 ASC, column2 DESC;
```

---

### 🔹 GROUP BY

Groups rows that have the same values in specified columns into summary rows.
Often used with aggregate functions.

```sql
SELECT COUNT(EmployeeID), DepartmentID FROM Employees GROUP BY DepartmentID;
```

---

### 🔹 HAVING

Filters groups created by the `GROUP BY` clause.

> `WHERE` filters rows **before grouping**, `HAVING` filters groups **after they are created**.

```sql
SELECT COUNT(EmployeeID), DepartmentID 
FROM Employees 
GROUP BY DepartmentID 
HAVING COUNT(EmployeeID) > 5;
```

---

### 🔹 JOIN

Combines rows from two or more tables based on a related column between them.

#### 🔸 INNER JOIN (or JOIN)

Returns records that have matching values in both tables.

```sql
SELECT Orders.OrderID, Customers.CustomerName
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

---

#### 🔸 LEFT JOIN (or LEFT OUTER JOIN)

Returns all records from the left table, and the matched records from the right table.
If there's no match, the result is `NULL` from the right side.

```sql
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```

---

#### 🔸 RIGHT JOIN (or RIGHT OUTER JOIN)

Returns all records from the right table, and the matched records from the left table.
If there's no match, the result is `NULL` from the left side.

```sql
SELECT Orders.OrderID, Employees.LastName
FROM Orders
RIGHT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID;
```

---

#### 🔸 FULL JOIN (or FULL OUTER JOIN)

Returns all records when there is a match in either left or right table.
If there is no match, the result is `NULL` for the columns of the table that lacks a match.

```sql
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```

---

#### 🔸 SELF JOIN

Joins a table to itself. Requires using table aliases.

```sql
SELECT A.EmployeeName AS Employee1, B.EmployeeName AS Employee2, A.City
FROM Employees A, Employees B
WHERE A.EmployeeID <> B.EmployeeID AND A.City = B.City;
```

---

#### 🔸 CROSS JOIN (or CARTESIAN JOIN)

Returns the **Cartesian product** of the two tables (all possible combinations of rows).
Generally used with caution.

```sql
SELECT Customers.CustomerName, Products.ProductName
FROM Customers
CROSS JOIN Products;
```

---

### 🔹 AS

Used to assign an alias to a column or table.

```sql
SELECT column_name AS alias_name FROM table_name;
SELECT c.CustomerName FROM Customers AS c;
```

---

### 🔹 DISTINCT

Returns only unique values for a specified column.

```sql
SELECT DISTINCT City FROM Customers;
```

---

### 🔹 LIMIT / TOP / ROWNUM

Restricts the number of rows returned by a query (syntax varies by SQL dialect).

```sql
SELECT * FROM table_name LIMIT 10;         -- MySQL, PostgreSQL
SELECT TOP 10 * FROM table_name;           -- SQL Server
SELECT * FROM table_name WHERE ROWNUM <= 10; -- Oracle
```

---

## 4. Aggregate Functions

**Aggregate functions** perform a calculation on a set of values and return a **single summary value**.

---

### 🔹 COUNT()

Returns the number of rows.

```sql
SELECT COUNT(*) FROM Employees; -- Counts all rows
SELECT COUNT(DepartmentID) FROM Employees; -- Counts non-NULL DepartmentID values
SELECT COUNT(DISTINCT DepartmentID) FROM Employees; -- Counts unique DepartmentID values
```

---

### 🔹 SUM()

Returns the total sum of a numeric column.

```sql
SELECT SUM(Salary) FROM Employees;
```

---

### 🔹 AVG()

Returns the average value of a numeric column.

```sql
SELECT AVG(Salary) FROM Employees;
```

---

### 🔹 MIN()

Returns the minimum value in a column.

```sql
SELECT MIN(Salary) FROM Employees;
```

---

### 🔹 MAX()

Returns the maximum value in a column.

```sql
SELECT MAX(Salary) FROM Employees;
```

---


## 5. Data Types

Data types specify the kind of value a column can hold. Common data types include:

### 🔹 Character Strings

* **CHAR(n)**: Fixed-length string of *n* characters.
* **VARCHAR(n)**: Variable-length string up to *n* characters.
* **TEXT**: Variable-length string (large text).

---

### 🔹 Numeric Types

* **INT or INTEGER**: Whole numbers.
* **SMALLINT, BIGINT**: Smaller or larger whole numbers.
* **DECIMAL(p,s)** or **NUMERIC(p,s)**: Fixed-point number with *p* total digits and *s* digits after the decimal point.
* **FLOAT, REAL, DOUBLE PRECISION**: Floating-point numbers.

---

### 🔹 Date and Time Types

* **DATE**: Stores date (`YYYY-MM-DD`).
* **TIME**: Stores time (`HH:MM:SS`).
* **DATETIME** or **TIMESTAMP**: Stores both date and time. *(Behavior can vary, e.g., TIMESTAMP might update automatically)*

---

### 🔹 Boolean Type

* **BOOLEAN**: Stores `TRUE` or `FALSE` (*some databases use BIT or TINYINT(1)*)

---

### 🔹 Binary Types

* **BINARY(n), VARBINARY(n), BLOB**: For storing binary data like images or files.

---

## 6. Keys

Keys are columns or sets of columns used to uniquely identify rows in a table or to establish relationships between tables.

---

### 🔹 PRIMARY KEY

* Uniquely identifies each record in a table.
* Cannot contain NULL values.
* A table can have only one Primary Key.
* Often automatically creates an index.

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(100)
);
```

---

### 🔹 FOREIGN KEY

* Refers to the **PRIMARY KEY** in another table.
* Used to link two tables and enforce referential integrity.
* Can contain NULL values.

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATE,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

---

### 🔹 UNIQUE KEY (or UNIQUE Constraint)

* Ensures all values in a column (or set of columns) are unique.
* A table can have multiple Unique Keys.
* Can accept one NULL value (*varies by database system*).

```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Email VARCHAR(100) UNIQUE
);
```

---

### 🔹 COMPOSITE KEY

A key made up of **two or more columns** to uniquely identify a record.
This can be a Primary Key or a Unique Key.

---

## 7. Indexes

An **index** is a special lookup table that the database search engine can use to speed up data retrieval.

* Think of it like an index in a book.
* **Primary Keys** are automatically indexed.
* You can create indexes on columns often used in `WHERE` or `JOIN`.

⚠️ **Note**: Indexes speed up `SELECT` queries but can slow down `INSERT`, `UPDATE`, and `DELETE`.

```sql
CREATE INDEX idx_lastname ON Employees (LastName);
DROP INDEX idx_lastname ON Employees; -- Syntax varies by SQL dialect
```

---

## 8. Normalization

**Normalization** is the process of organizing columns and tables in a relational database to minimize **data redundancy** and improve **data integrity**.

It involves:

* Dividing large tables into smaller, well-defined tables.
* Defining relationships between them.

### 🔹 Goals:

* Eliminate redundant data
* Reduce insert/update/delete anomalies
* Simplify queries

---

### 🔹 Normal Forms

* **1NF (First Normal Form)**:
  Atomic values; no repeating groups; same type in each column.

* **2NF (Second Normal Form)**:
  Must be in 1NF; non-key attributes must depend on **entire** primary key (for composite keys).

* **3NF (Third Normal Form)**:
  Must be in 2NF; no transitive dependencies (non-key should not depend on other non-key).

* **BCNF (Boyce-Codd Normal Form)**:
  Stricter than 3NF; for every non-trivial `X → Y`, `X` must be a **superkey**.

* **4NF, 5NF, 6NF**:
  Exist but are rarely used in practice.

---

## 9. Views

A **view** is a virtual table based on the result-set of an SQL statement.
It contains rows and columns just like a real table.

### 🔹 Benefits:

* **Simplicity**: Hides complex queries
* **Security**: Restricts data access
* **Consistency**: Abstracts structural changes

```sql
CREATE VIEW ActiveEmployees AS
SELECT EmployeeID, FirstName, LastName
FROM Employees
WHERE IsActive = TRUE;

-- Query the view
SELECT * FROM ActiveEmployees;
```

---

## 10. Stored Procedures & Functions

### 🔹 Stored Procedure

A set of SQL statements stored in the database and executed by name.

* Can accept input/output parameters
* Can return result sets
* Encapsulates logic
* Syntax varies by RDBMS

---

### 🔹 Function (User-Defined Function / UDF)

* Returns a **single value** (scalar) or a **table** (table-valued).
* Can be used like built-in functions in SQL.
* Syntax also varies across systems.

---

## 11. Triggers

A **trigger** is a stored procedure that executes **automatically** in response to events (`INSERT`, `UPDATE`, `DELETE`) on a specific table.

### 🔹 Uses:

* Enforce data integrity
* Automate actions
* Log changes

```sql
CREATE TRIGGER before_employee_update
BEFORE UPDATE ON Employees
FOR EACH ROW
BEGIN
    -- Trigger logic here
END;
```

---

## 12. SQL Comments

Used to explain parts of SQL code. **Ignored** by the engine.

```sql
-- This is a single-line comment

/*
This is a
multi-line comment
*/
```

---

## 13. Operators

SQL uses various types of operators:

### 🔹 Arithmetic Operators

* `+`, `-`, `*`, `/`, `%` (Modulo)

### 🔹 Comparison Operators

* `=`, `!=` or `<>`, `>`, `<`, `>=`, `<=`

### 🔹 Logical Operators

* `AND`, `OR`, `NOT`, `ALL`, `ANY`, `BETWEEN`, `EXISTS`, `IN`, `LIKE`, `IS NULL`, `UNIQUE`

---

### ✅ SQL `LIKE` Pattern Matching (Interview-Focused)

**Purpose:**
Used with `WHERE` to match **string patterns** using wildcards.

---

### 🔹 Wildcards & Usage

| Wildcard | Meaning                                   | Example              | Matches                   |
| -------- | ----------------------------------------- | -------------------- | ------------------------- |
| `%`      | Any number of characters (including none) | `'a%'`               | `'a'`, `'apple'`, `'abc'` |
| `_`      | Exactly one character                     | `'h_t'`              | `'hat'`, `'hot'`, `'hit'` |
| `ESCAPE` | Used to treat `%` or `_` as literal       | `'50\%%' ESCAPE '\'` | `'50%'`, `'50% off'`      |

---

### 🔸 Common Patterns

* **Starts with**: `'A%'` → `Alice`, `Adam`
* **Ends with**: `'%son'` → `Johnson`, `Emerson`
* **Contains**: `'%tech%'` → `Techteam`, `Fintech`
* **Fixed-length**: `'___'` → Any 3-letter word

---

### 🔹 Example Queries

```sql
-- Names starting with 'A'
SELECT * FROM Employees WHERE Name LIKE 'A%';

-- Emails ending with '@gmail.com'
SELECT * FROM Users WHERE Email LIKE '%@gmail.com';

-- Codes with exactly 3 letters
SELECT * FROM Items WHERE Code LIKE '___';

-- Escaping literal %
SELECT * FROM Offers WHERE Promo LIKE '50\%%' ESCAPE '\';
```

---

### ✅ Key Points for Interviews

* `%` → multiple chars; `_` → single char
* `LIKE` is **case-insensitive** in most SQL engines
* Use `ESCAPE` to treat `%` or `_` as literal characters
* Avoid `LIKE` on large tables without indexes—it’s slow

---

