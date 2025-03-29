<font color="#7f7f7f"><em>Monday, 24-03-2025 - 15:55</em></font>
#programming #database #sql 

## Index

[[#1. Introduction to SQL]]
[[#2. Database Design and Normalization]]
[[#3. Basic SQL Commands]]
[[#4. Data Manipulation Language (DML)]]
[[#5. Data Querying and Filtering]]
[[#6. Joins and Relationships]]
[[#7. Subqueries and Nested Queries]]
[[#9. SQL Views]]
[[#10. SQL Transactions and ACID Properties]]
[[#11. Stored Procedures and Functions]]
[[#12. Triggers]]
[[#13. Indexing for Performance Optimization]]
[[#14. SQL Security]]

---
---
## 1. Introduction to SQL

### What is SQL (Structure Query Language)?

**SQL** is a standard language used to communicate with **relational databases**. It allows you to define, manipulate, query, and control access to data stored in structured tables.

**Key Operations**
- Define schemas and structures -> `CREATE TABLE`
- Manipulate data -> `INSERT`, `UPDATE`, `DELETE`
- Query data -> `SELECT`
- Control access -> `GRANT`, `REVOKE`

```sql
SELECT name, email FROM Users WHERE active = TRUE;
```

> SQL is *declarative*, not imperative - you describe **what** you want, not **how** to get it

### Relational Databases vs. Non-Relational Databases

| Feature           | Relational (SQL)                            | Non-Relational (NoSQL)                                          |
| ----------------- | ------------------------------------------- | --------------------------------------------------------------- |
| Structure         | Tables (rows, columns)                      | Key-Value, Document, Graph, etc.                                |
| Schema            | Strict (schema-based)                       | Flexible or schema-less                                         |
| Query Language    | SQL                                         | Varies (MongoDB uses BSON-style)                                |
| Transactions      | Strong ACID compliance                      | Often eventual consistency                                      |
| Scalability       | Vertical (scale-up)                         | Horizontal (scale-out)                                          |
| Use Cases         | Structured, normalized, **complex queries** | Unstructured, scalable, flexible, **big data & real-time apps** |
| Project Use Cases | Banking systems, ERPs, Inventory management | Social networks, IoT systems, Real-time analytics               |

**Relational DB (SQL) Example**
```sql
CREATE TABLE Orders (
	order_id INT PRIMARY KEY,
	customer_id INT,
	total DECIMAL(10,2)
);
```

**Non-Relational DB (NoSQL - MongoDB-like) Example**
```json
{
	"order_id": 101,
	"costumer_id": 1,
	"total": 250.00
}
```

>[!important]
>Relational databases (SQL) are ideal when **data integrity, joins, and normalization** are key.
>Non-Relational (NoSQL) shines with **unstructured or rapidly changing data**.

### Popular SQL Databases

| DBMS       | Notes                                                                          |
| ---------- | ------------------------------------------------------------------------------ |
| MySQL      | Widely used in web apps (PHP + MySQL stack), free and open-source              |
| PostgreSQL | Feature-rich, ACID-compliant, supports advanced data types (e.g. JSONB, array) |
| SQLite     | Lightweight, file-based, used in mobile/dev tools                              |
| SQL Server | Microsoft ecosystem, integrates with .NET stack                                |
| Oracle     | Enterprise-grade, robust features, paid licensing                              |

---
## 2. Database Design and Normalization
### Tables, Columns and Rows

A **table** is the core structure in a relational database. It organizes data into **rows** (records) and **columns** (attributes), each with a **specific data type**.

```sql
CREATE TABLE Students (
	student_id INT,
	name VARCHAR(100),
	birthdate DATE,
	gpa DECIMAL(3,2)
);
```

- **Rows:** individual entries (e.g. one student)
- **Columns:** fields (e.g. `name`)
- **Data Types:**
	- `INT`, `BIGINT`: integer
	- `VARCHAR(n)`, `TEXT`: strings
	- `DATE`, `DATETIME`: dates/timestamps
	- `BOOLEAN`: true/false
	- `DECIMAL(x,y)`, `FLOAT`: numeric precision

> Prefer `DECIMAL` **over** `FLOAT` when exact precision matters.

### Primary Key, Foreign Key, and Composite Key

**Primary Key (PK):** uniquely identifies each row (entry); must be `NOT NULL` and unique.

**Foreign Key (FK):** enforces a relationship between two tables. Allows for accessing in between tables.

**Composite Key:** a primary key made up of **multiple columns**.

```sql
CREATE TABLE Departments (
	department_id INT PRIMARY KEY,
	name VARCHAR(100)
);

CREATE TABLE Employees (
	employee_id INT PRIMARY KEY,
	name VARCHAR(100),
	department_id INT,
	FOREIGN KEY (department_id) REFERENCES Departments(department_id)
);

-- Composite PK example
CREATE TABLE CourseEnrollments (
	student_id INT,
	course_id INT,
	PRIMARY KEY (student_id, course_id)
);
```

### Indexing and Performance Optimization

An **index** improves query speed by allowing faster lookup on specific columns - especially for filtering (`WHERE`), sorting (`ORDER BY`), or joining.

```sql
CREATE INDEX idx_name ON Students(name); -- Create index

DROP INDEX idx_name; -- Drop index
```

>[!important]
>- Indexes **speed up reads**, but **slow down writes**.
>- Too many indexes can **hurt performance**.
>- Use `EXPLAIN` or `QUERY PLAN` to check index usage.

> When and why to use indexes? And how does it support query optimization?

### Normalization (1NF, 2NF, 3NF, BCNF, etc.)

**Normalization** is the process of organizing data to reduce redundancy and improve integrity.

| Normal Form | Goal                           | Rule                                  |
| ----------- | ------------------------------ | ------------------------------------- |
| 1NF         | Atomic columns                 | No repeating groups or arrays         |
| 2NF         | Eliminate partial dependencies | Non-key columns depend on whole PK    |
| 3NF         | Remove transitive dependencies | Non-key columns depend **only** on PK |
| BCNF        | Stricter 3NF                   | Every determinant is a candidate key  |

> Students(student_id, name, courses_taken)
> -- courses_taken = "Math, English" -> violates 1NF
> 
> To fix this create a StudentCourses table with a composite PK of student_id and course_name

### Denormalization

**Denormalization** is the process of **intentionally adding redundancy** to optimize read performance (e.g. reduce joins in frequently used queries).

Instead of:
```sql
SELECT o.order_id, c.name
FROM Orders o JOIN Customers c ON o.costumer_id = c.costumer_id;
```

-> You might **store** `costumer_name` **inside** `Orders` for faster access.

Trade-offs:
- Pros: Faster reads, fewer joins
- Cons: Data redundancy, harder updates.

> Denormalization is justified in reporting or read-heavy systems.
---
## 3. Basic SQL Commands

### Create/Drop Database 

```sql
-- Create and remove entire databases
CREATE DATABASE my_app_db;
DROP DATABASE my_app_db;
```

> `DROP DATABASE` **removes all tables and data permanently**. Use with caution in production!

### Create/Alter/Drop Table

```sql
-- Define, modify, and delete tables and their structures
CREATE TABLE Products (
	product_id INT PRIMARY KEY,
	name VARCHAR(100),
	price DECIMAL(10,2)
);

ALTER TABLE Products ADD COLUMN stock INT DEFAULT 0; -- add column

DROP TABLE Products;
```

> `ALTER` can also modify types, constraints, or rename columns.

### Constraints

Define **rules** for data integrity within the tables.

| Constraint    | Use                        |
| ------------- | -------------------------- |
| `NOT NULL`    | Prevent empty values       |
| `UNIQUE`      | Ensure no duplicates       |
| `PRIMARY KEY` | Uniquely identifies row    |
| `FOREIGN KEY` | Link to another table's PK |
| `CHECK`       | Enforce custom logic       |
| `DEFAULT`     | Provide default value      |
```sql
CREATE TABLE Users (
	id INT PRIMARY KEY,
	email VARCHAR(100) UNIQUE NOT NULL,
	created_at DATE DEFAULT CURRENT_DATE,
	age INT CHECK (age >= 13)
);
```

> `PRIMARY KEY` vs. `UNIQUE NOT NULL` => Only one PK per table; multiple UNIQUEs allowed.

---
## 4. Data Manipulation Language (DML)

### CRUD Operations
##### Create - Insert
```sql
INSERT INTO Products (product_id, name, price) VALUES (1, 'Headphones', 49.99);
```
##### Read - Select
```sql
SELECT name, price FROM Products WHERE price < 100;
SELECT * FROM Products -- selects all rows/attributes
```
##### Update
```sql
UPDATE Products SET price = 59.99 WHERE product_id = 1;
```
##### Delete
```sql
DELETE FROM Products WHERE product_id = 1;
```

```sql
TRUNCATE TABLE Products; -- deletes all rows efficiently & resets auto-increment
```

> Forgetting the `WHERE` clause on `UPDATE and DELETE` will cause it to update **every row**.
---
## 5. Data Querying and Filtering

### `Where` Clause - Conditional Filtering

```sql
SELECT * FROM Users WHERE age > 18;
```

> Filters rows. Allows logical operators: `AND`, `OR`, and `NOT`. 


### `ORDER BY` Clause - Sorting Query Results

```sql
SELECT * FROM Users ORDER BY name ASC;
SELECT * FROM Users ORDER BY age DESC, name ASC;
```

> Sorting is done **after filtering** - `WHERE` and then `ORDER BY`.


### `DISTINCT` - Duplicates Removal

```sql
SELECT DISTINCT grade FROM Students;
```


### `LIMIT`/`OFFSET` - Pagination or Limiting Results

```sql
SELECT * FROM Products ORDER BY price LIMIT 5 OFFSET 10;
```

> Pattern commonly used in paginated APIs. `OFFSET` skips rows.


### `GROUP BY`, `HAVING` and Aggregate Functions - Group and Filter

##### Group by

```sql
SELECT department, Count(*) AS emp_count FROM Employees GROUP BY department
```

> Groups rows that share a value in one or more columns.

##### Having

```sql
SELECT department, COUNT(*) AS emp_count FROM Employees 
GROUP BY department 
HAVING COUNT(*) > 5;
```

> Filters groups **after aggregation** (like WHERE, but for grouped data).

##### Aggregate Functions

| Aggregate Function | Description               |
| ------------------ | ------------------------- |
| `COUNT()`          | Total number of rows      |
| `SUM()`            | Total of a numeric column |
| `AVG()`            | Average value             |
| `MIN()`            | Smallest value            |
| `MAX()`            | Largest vallue            |

```sql
SELECT COUNT(*) FROM Orders;                 -- Total number of orders
SELECT AVG(salary) FROM Employees;           -- Average salary
SELECT MAX(age) FROM Students;               -- Oldest student
```

```sql
SELECT COUNT(*) FROM Orders WHERE status = 'Pending';
```

##### Combining Group by + Having + Aggregate Functions

```sql
SELECT department, 
       COUNT(*) AS emp_count,
       AVG(salary) AS avg_salary,
       MAX(salary) AS highest_salary
FROM Employees
GROUP BY department
HAVING AVG(salary) > 50000;

-- Shows only departments where the avarage salary is over 50k
```


### `LIKE` - Pattern Matching

- `%`: any number of characters
- `_`: exactly one character

```sql
SELECT * FROM Users WHERE name LIKE 'A%';    -- Starts with A
SELECT * FROM Users WHERE name LIKE '%son';  -- Ends with son
SELECT * FROM Users WHERE name LIKE '_im';   -- Exactly 3 letters, ending in "im"
```

> Use `ILIKE` for case-insensitive search.
---
## 6. Joins and Relationships

Joins allow you to **combine rows** from two or more tables based on related columns (usually foreign keys). They're fundamental for working with normalized databases.

![[Pasted image 20250329131016.png]]

### Inner Join

Returns only rows with matching keys in both tables -> **INTERSECTION**

```pgsql
Employees               Departments
-----------             -----------------
employee_id             department_id
name                    name
department_id (FK) ───▶ department_id (PK)
```

```sql
SELECT e.name, d.name AS department FROM Employees e
INNER JOIN Departments d ON e.department_id = d.department_id;
```

> **RESULT:** Employees with a **matching department**.

### Left Join (or Left Outer Join)

Returns all rows from the **left table**, plus matches from the right table (or NULL if no match).

```pgsql
Employees               Departments
-----------             -----------------
employee_id             department_id
name                    name
department_id (FK) ───▶ department_id (PK)
```

```sql
SELECT e.name, d.name AS department FROM Employees e
LEFT JOIN Departments d ON e.department_id = d.department_id;
```

> **RESULT:** Employees **with a matching department**.

### Right Join (or Right Outer Join)

Return all rows from the **right table**, and matched rows from the left (NULL if no match).

```sql
SELECT e.name, d.name AS department FROM Employees e
RIGHT JOIN Departments d ON e.department_id = d.department_id;
```

> **RESULT:** All departments, even if **no employees** belong to them.

### Full Join (or Full Outer Join)

```sql
SELECT e.name, d.name AS department FROM Employees e
FULL OUTER JOIN Departments d ON e.department_id = d.department_id;
```

> **RESULT:** All employees + all departments - matched or not.

### Cross Join

Returns the **Cartesian product** - every row from the first table matched with every row from the second.

```java
Colors                  Sizes
------------            ------------
color                   size
('Red')                 ('S')
('Blue')                ('M')
                        ('L')
```

```sql
SELECT c.color, s.size FROM Colors c CROSS JOIN Sizes s;
```

> **RESULT:** Every colour-size pair. Useful for combinations.

### Self Join

A table joins with itself - often used for hierarchical or relational lookups.

```md
Employees
------------
employee_id (PK)
name
manager_id  ────▶ employee_id (FK)
```

```sql
SELECT e1.name AS employee, e2.name AS manager FROM Employees y1
JOIN Employees e2 ON e1.manager-id = e2.employee_id;
```

> **RESULT:** Pairs of employees and their managers.

### Table Relationships

These relationships define **how records in one table relate** to records in another. Designing these correctly ensures data integrity.

### One-to-One (1-1)

Each row in Table A maps to **ONE** row in Table B.

Example: Table `Users` <-> Table `UserProfiles`.

```scss
Users                   UserProfiles
------------            -----------------
user_id (PK) ───────▶   user_id (PK, FK)
```

```sql
CREATE TABLE Users (
  user_id INT PRIMARY KEY
);

CREATE TABLE UserProfiles (
  user_id INT PRIMARY KEY,
  bio TEXT,
  FOREIGN KEY (user_id) REFERENCES Users(user_id)
);
```

> Usually enforced via shared `PRIMARY KEY`.

### One-to-Many (1-N)

A row in Table A maps to **MANY** rows in Table B. The most common relationship.

Example: One `Department` <->> (has) many `Employees`.

```scss
Departments             Employees
--------------          ----------------
department_id (PK)      employee_id (PK)
                        department_id ──▶ department_id (FK)
```

```sql
CREATE TABLE Departments (
  department_id INT PRIMARY KEY
);

CREATE TABLE Employees (
  employee_id INT PRIMARY KEY,
  department_id INT,
  FOREIGN KEY (department_id) REFERENCES Departments(department_id)
);
```

> Think foreign key in the **many** side pointing to the **one** side.

### Many-to-Many (N-N)

Multiple rows in Table A relate to multiple rows in Table B. Requires a **junction table**.

Example: Students enrolled in multiple courses.

```scss
Students                Courses
------------            -------------
student_id (PK)         course_id (PK)

        ↘              ↙
        Enrollments (junction table)
        ----------------------------
        student_id (FK)
        course_id (FK)
        PRIMARY KEY (student_id, course_id)
```

```sql
CREATE TABLE Students (
  student_id INT PRIMARY KEY
);

CREATE TABLE Courses (
  course_id INT PRIMARY KEY
);

CREATE TABLE Enrollments (
  student_id INT,
  course_id INT,
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES Students(student_id),
  FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);
```

> Junction tables often have composite keys.

---
## 7. Subqueries and Nested Queries

Subqueries (aka inner queries or nested queries) are SQL queries **embedded inside another query**.

### Subqueries in `SELECT`, `FROM`, and `WHERE`

```sql
SELECT name, salary,
	(SELECT AVG(salary) FROM Employees AS avg_salary)
FROM Employees;

-- Shows each employee with the overall company average salary
```

```sql
SELECT * FROM Employees WHERE department_id = (
	SELECT department_id FROM Employees WHERE name = 'Alice'
);

-- Shows employees in the same department as Alice
```

```sql
SELECT dept_stats.department_id, dept_stats.avg_salary FROM (
	SELECT department_id, AVG(salary) AS avg_salary FROM Employees GROUP BY department_id
) AS dept_stats;

-- Get average salary per department using an inline table
```


### Independent vs. Correlated Subqueries

**Independent Subquery:** Can run by itself. Evaluated once.

```sql
SELECT name FROM Employees WHERE salary > (SELECT AVG(salary) FROM Employees);
```

**Correlated Subquery:** Refers to a column from the outer query. Re-evaluated **per row** of the outer query. 

```sql
SELECT name, salary FROM Employees e WHERE salary > (
	SELECT AVG(salary) FROM Employees WHERE department_id = e.department_id
);
```

> Correlated subqueries are less efficient and can be replaced with `JOIN` + `GROUP BY`.

### Common Table Expressions (CTEs)

CTEs let you define a **Temporary result set** that you can reuse in the same query. Great for clarity and breaking down logic.

```sql
WITH HighEarners AS (
	SELECT * FROM Employees WHERE salary > 50000
)

SELECT name from HighEarners;
```

##### When to use CTEs?
- You want clean, readable logic
- You reuse the same subquery
- You're writing recursive queries.

---
## 9. SQL Views

A **view** is a **virtual table** based on the result of a query. It behaves like a real table but doesn't store data itself - it fetches live data from the base tables every time you query it.

### Creating and Managing Views 
##### Create a View

```sql
CREATE VIEW HighSalaryEmployees AS
SELECT name, salary FROM Employees WHERE salary > 50000;
```

##### Querying a View

```sql
SELECT * FROM HighSalaryEmployees;
```

##### Drop a View

```sql
DROP VIEW HighSalaryEmployees;
```

> Views help abstract complex queries, **improve readability**, and **limit access** to sensitive data (e.g. expose salary, not a password).

### Updatable Views

An **updatable view** allows `INSERT`, `UPDATE`, or `DELETE` operations directly on the view, which then affect the base table(s).

A view is updatable if:
- Based on a **single table**
- Doesn't use `GROUP BY`, `DISTINCT`, `JOIN`, `UNION`, or aggregate functions
- Doesn't include subqueries or calculated columns

Updatable View

```sql
CREATE VIEW ActiveUsers AS 
SELECT user_id, name, email FROM Users WHERE is_active = TRUE;
```

```sql
UPDATE ActiveUsers SET email = 'new@email.com' WHERE user_id = 101;
```

Non-Updatable View

```sql
CREATE VIEW DepartmentSalaries AS
SELECT department_id, AVG(salary) AS avg_salary FROM Employees GROUP BY department_id;
```

---
## 10. SQL Transactions and ACID Properties

A **transaction** is a sequence of one or more SQL operations executed as a **single unit of work**. 
Transactions ensure data integrity - especially in systems where multiple operations must succeed or fail together.

### ACID Properties

ACID is the set of four key properties that guarantee reliable transaction processing

| Property    | Meaning                                                               |
| ----------- | --------------------------------------------------------------------- |
| Atomicity   | All operations in a transaction succeed or none do (all-or-nothing).  |
| Consistency | The database moves from one valid state to another.                   |
| Isolation   | Transactions are isolated from one another (no dirty reads, etc.)     |
| Durability  | Once committed, changes are permanent - even in case of system crash. |
> ACID allows to design systems that are both **reliable** and **concurrent**.

### `BEGIN TRANSACTION`, `COMMIT`, `ROLLBACK`

Used to manually control transaction boundaries.

```sql
BEGIN TRANSACTION;

UPDATE Accounts
SET balance = balance - 100 WHERE account_id = 1;

UPDATE Accounts
SET balance = balance + 100 WHERE account_id = 2;

COMMIT;
```

```sql
BEGIN TRANSACTION;

-- some operations

ROLLBACK; -- Undo all changes in this transaction
```

> Most databases auto-commit by default.

### Savepoints

A **savepoint** is a named point in a transaction you can roll back to - useful in complex transactions where you don't want to undo everything.

```sql
BEGIN TRANSACTION;

UPDATE Orders SET status = 'processing' WHERE order_id = 1;

SAVEPOINT before_stock_check;

UPDATE Stock SET quantity = quantity - 1 WHERE product_id = 42;

-- something goes wrong here
ROLLBACK TO SAVEPOINT before_stock_check;

COMMIT;
```

---
## 11. Stored Procedures and Functions

**Stored Procedures** and **Functions** are blocks of reusable SQL logic that are **stored in the database** and can be called by applications or other queries.

They help encapsulate business logic, promote code reuse, and improve performance by reducing client-server communication.

### Creating Stored Procedures (`CREATE PROCEDURE`)

A **stored procedure** is a routine that can perform **multiple operations** like queries, inserts, updates, and conditionals. It may or may not return a value.

```sql
DELIMITER //

CREATE PROCEDURE GetEmployeesByDepartments(IN dept_id INT)
BEGIN
	SELECT * FROM Employees WHERE department_id = dept_id;
END //

DELIMITER ;
```

```sql
CALL GetEmployeesByDepartment(2);
```

### Creating Functions (`CREATE FUNCTION`)

A **function** returns a **single value** and is usually used inside SQL expressions. It's expected to be **Deterministic and side-effect free**.

```sql
DELIMITER //

CREATE FUNCTION GetBonus(salary DECIMAL(10,2))
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
	RETURN salary * 0.1;
END //

DELIMITER ;
```

```sql
SELECT name, GetBonus(salary) AS bonus FROM Employees;
```

> Functions are often used for **computer columns**, **valid logic**, or **conversions**.

### Difference Between Stored Procedures and Functions

| Feature              | Stored Procedure                     | Function                            |
| -------------------- | ------------------------------------ | ----------------------------------- |
| Returns a value      | Optional (can return 0 or more rows) | Mandatory (must return a value)     |
| Used in `SELECT`     | No                                   | Yes                                 |
| Side effects allowed | Yes (can modify data)                | No (should not modify data)         |
| Called with          | `CALL procedure_name()`              | Inside queries like `SELECT func()` |
| Output parameters    | Yes                                  | No                                  |
- Use **procedures** for **multi-step workflows** or data manipulation.
- Use **functions** when you need **reusable expressions or calculations** inside `SELECT`, `WHERE`, etc.

---
## 12. Triggers

A **trigger** is a special kind of stored procedure that **automatically executes** in response to certain events on a table (like `INSERT`, `UPDATE`, or `DELETE`).

Triggers are used to enforce **business rules**, maintain **audit logs**, or perform **automatic actions** on data changes.

### What are Triggers?

Triggers are **event-driven** - they fire **automatically** when a specified database event occurs on a table.

Use Cases:
- Logging changes to sensitive tables
- Automatically updating related records
- Preventing invalid data updates

> Triggers may introduce performance issues or debugging difficulties.

### `BEFORE` and `AFTER` Triggers

| Trigger Type | Executes...                           | Common Use                            |
| ------------ | ------------------------------------- | ------------------------------------- |
| `BEFORE`     | Before the change is applied          | Validate or modify incoming data      |
| `AFTER`      | After the change is successfully made | Logging, triggering follow-up actions |

### `INSERT`, `UPDATE`, and `DELETE` Triggers

Triggers respond to **data modification events** on a table.

```sql
-- Trigger that logs salary updates with an AFTER UPDATE effect
DELIMITER //

CREATE TRIGGER AfterSalaryUpdate
AFTER UPDATE ON Employees
FOR EACH ROW
BEGIN
    INSERT INTO EmployeeAudit (employee_id, old_salary, new_salary)
    VALUES (OLD.employee_id, OLD.salary, NEW.salary);
END //

DELIMITER ;
```

---
## 13. Indexing for Performance Optimization - Minimalistic

**What is an Index?**
A data structure (often a B-tree) that speeds up data retrieval - like a book's index for quick lookup.

**When to Use It**
On columns used in `WHERE`, `JOIN`, `ORDER BY`, or frequently searched fields (like `email`, `username`, `product_id`).

**Types**
- **Single-column index**
- **Composite (multi-column) index**
- **Unique index** (enforces uniqueness)
- **Clustered index** (affects physical row order - one per table)

**Downsides**
- **Slower** `INSERT`/`UPDATE`/`DELETE` due to index maintenance
- More **storage usage**
- Too many indexes can **hurt** performance instead of helping

---
## 14. SQL Security - Minimalistic

**User Privileges & Access Control**
Use `GRANT` and `REVOKE` to control who can `SELECT`, `INSERT`, `UPDATE`, etc. Prevents unauthorized actions.

**Role-Based Access Control (RBAC)**
Assign roles (e.g. `admin`, `readonly`) to users for easier and more scalable permission management.

**SQL Injection Attacks**
One of the biggest threats. Happens when user input is directly included in queries. Use **prepared statements** to prevent it.

**Least Privilege Principle**
Give users **only the permissions they absolutely need** - avoid running apps as root or superuser.

**Audit Logging**
Track access and changes to sensitive tables to detect suspicious behaviour or maintain compliance.

**Encryption**
Use **data-at-rest** and **data-in-transit** encryption (e.g. SSL/TLS) to protect sensitive data like password and PII.
