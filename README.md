
# 🐬 MySQL Notes – Topic-wise with Examples

---

## 📘 1. Introduction to Databases

- Data evolution: **Paper ➝ Notepad ➝ Excel ➝ SQL ➝ NoSQL**

---

## 🆚 2. DBMS vs RDBMS

| Feature            | DBMS                       | RDBMS                      |
|--------------------|----------------------------|----------------------------|
| Example            | File System (e.g., Excel)  | SQL-based Relational DB    |
| Storage Structure  | Flat Files                 | Tables with Relationships  |

---

### 🏥 Hospital Case Study

**DBMS Table:**

| pid | pname | disease | did | dname | dexp | dcontact | ddept |
|-----|-------|---------|-----|-------|------|----------|--------|

**RDBMS Normalized Tables:**

- **Patients Table**
```sql
pid, pname, disease, did
```

- **Doctors Table**
```sql
did, dname, dexp, dcontact, ddeptid
```

- **Departments Table**
```sql
ddeptid, deptname
```

---

## 🔁 3. SQL vs NoSQL

| Feature       | SQL                           | NoSQL                               |
|---------------|-------------------------------|--------------------------------------|
| Full Form     | Structured Query Language     | Not Only SQL                        |
| Structure     | Tables ➝ Rows                 | Collections ➝ Documents             |

---

## 🧰 4. MySQL Installation

- Download from [MySQL Official Site](https://dev.mysql.com/)
- Install and login using:

```bash
mysql -u root -p
```

---

## 📊 5. Data Flow

| Database Type | Structure                                   |
|---------------|---------------------------------------------|
| SQL           | Database ➝ Tables ➝ Rows/Records            |
| NoSQL         | Database ➝ Collections ➝ Documents          |

---

## 🧠 6. MySQL Data Types

| Data Type                  | Example               |
|----------------------------|-----------------------|
| `VARCHAR(n)`               | 'Akhil', 'Priya'      |
| `CHAR(n)`                  | Fixed-length strings  |
| `INT`                      | 1, 2, 100             |
| `DECIMAL(p, s)`            | 23784.23              |
| `DATE`                     | '2001-11-16'          |
| `DATETIME`                 | '2001-11-16 15:34:15' |

---

## 🧾 7. SQL Language Categories

| Category | Full Form                    | Examples                               |
|----------|------------------------------|----------------------------------------|
| DDL      | Data Definition Language     | `CREATE`, `DROP`, `ALTER`, `TRUNCATE`  |
| DML      | Data Manipulation Language   | `INSERT`, `UPDATE`, `DELETE`           |
| TCL      | Transaction Control Language | `COMMIT`, `ROLLBACK`, `SAVEPOINT`      |
| DCL      | Data Control Language        | `GRANT`, `REVOKE`                      |
| DQL      | Data Query Language          | `SELECT`                               |

---

# 🧱 8. DDL Operations

## 🔹 CREATE TABLE

```sql
CREATE TABLE users (
    uid INT PRIMARY KEY,
    name VARCHAR(50),
    salary DECIMAL(10, 2)
);
```

## ✏️ ALTER TABLE

```sql
ALTER TABLE users ADD COLUMN age INT DEFAULT 18;
ALTER TABLE users DROP COLUMN age;
ALTER TABLE users MODIFY COLUMN salary DECIMAL(12,2) NOT NULL;
```

## ❌ DROP TABLE

```sql
DROP TABLE users;
```

## 🧹 TRUNCATE TABLE

```sql
TRUNCATE TABLE users;
```

## 🔁 RENAME TABLE

```sql
RENAME TABLE employees TO team_members;
```

---

# 🔐 9. Constraints with Examples

## ✅ PRIMARY KEY

```sql
CREATE TABLE students (
    roll_no INT PRIMARY KEY,
    name VARCHAR(50)
);
```

## ✅ NOT NULL

```sql
CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);
```

## ✅ UNIQUE

```sql
CREATE TABLE contacts (
    phone VARCHAR(15) UNIQUE
);
```

## ✅ DEFAULT

```sql
CREATE TABLE orders (
    id INT,
    status VARCHAR(20) DEFAULT 'pending'
);
```

## ✅ CHECK

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT CHECK (age > 18)
);

-- Examples
INSERT INTO employees VALUES (1, 'Akhil', 25); -- Success
INSERT INTO employees VALUES (2, 'Priya', 15); -- Fails
```

## ✅ FOREIGN KEY

```sql
CREATE TABLE departments (
    did INT PRIMARY KEY,
    dname VARCHAR(50)
);

CREATE TABLE emp (
    eid INT PRIMARY KEY,
    ename VARCHAR(50),
    did INT,
    FOREIGN KEY (did) REFERENCES departments(did)
);
```

---

# 🔗 10. Foreign Key: ON DELETE Behaviors

## 🧪 Setup Tables

```sql
CREATE TABLE departments (
    did INT PRIMARY KEY,
    dname VARCHAR(50)
);

CREATE TABLE emp_cascade (
    eid INT PRIMARY KEY,
    ename VARCHAR(50),
    did INT,
    FOREIGN KEY (did) REFERENCES departments(did)
    ON DELETE CASCADE
);

CREATE TABLE emp_setnull (
    eid INT PRIMARY KEY,
    ename VARCHAR(50),
    did INT,
    FOREIGN KEY (did) REFERENCES departments(did)
    ON DELETE SET NULL
);
```

## 📥 Insert Sample Data

```sql
INSERT INTO departments VALUES (1, 'HR'), (2, 'IT');
INSERT INTO emp_cascade VALUES (101, 'Akhil', 1), (102, 'Priya', 2);
INSERT INTO emp_setnull VALUES (201, 'Kiran', 1), (202, 'Vani', 2);
```

## ❌ Case: Delete Parent Record (with No ON DELETE Action)

```sql
-- If ON DELETE is not set, the following will throw an error
DELETE FROM departments WHERE did = 1;

-- 🔴 Error:
-- Cannot delete or update a parent row: a foreign key constraint fails
```

## ✅ Case: ON DELETE CASCADE

```sql
DELETE FROM departments WHERE did = 1;

-- ✅ Automatically deletes employees from emp_cascade where did = 1
```

## ✅ Case: ON DELETE SET NULL

```sql
DELETE FROM departments WHERE did = 2;

-- ✅ Automatically sets did = NULL in emp_setnull for rows where did = 2
```

---

# SELECT, DISTINCT, WHERE, EXPRESSION, AS, INSERT, UPDATE, DELETE, 

---

## 📘 Table: `students`

```sql
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    city VARCHAR(50),
    age INT
);
```

### ✅ Insert Records

```sql
INSERT INTO students (id, name, city, age) VALUES
(1, 'Akhil', 'Delhi', 21),
(2, 'Priya', 'Mumbai', 22),
(3, 'Nikhil', 'Delhi', 23),
(4, 'Akhil', 'Delhi', 21),
(5, 'Sara', 'Mumbai', 22),
(6, 'Priya', 'Bangalore', 22),
(7, 'Akhil', 'Delhi', 28);
```


### 🔍 SELECT Queries

```sql
SELECT * FROM students;
```

| id | name   | city      | age |
| -- | ------ | --------- | --- |
| 1  | Akhil  | Delhi     | 21  |
| 2  | Priya  | Mumbai    | 22  |
| 3  | Nikhil | Delhi     | 23  |
| 4  | Akhil  | Delhi     | 21  |
| 5  | Sara   | Mumbai    | 22  |
| 6  | Priya  | Bangalore | 22  |
| 7  | Akhil  | Delhi     | 28  |

```sql
SELECT DISTINCT * FROM students;
```

| id | name   | city      | age |
| -- | ------ | --------- | --- |
| 1  | Akhil  | Delhi     | 21  |
| 2  | Priya  | Mumbai    | 22  |
| 3  | Nikhil | Delhi     | 23  |
| 4  | Akhil  | Delhi     | 21  |
| 5  | Sara   | Mumbai    | 22  |
| 6  | Priya  | Bangalore | 22  |
| 7  | Akhil  | Delhi     | 28  |

```sql
SELECT name FROM students;
```

| name   |
| ------ |
| Akhil  |
| Priya  |
| Nikhil |
| Akhil  |
| Sara   |
| Priya  |
| Akhil  |

```sql
SELECT DISTINCT name FROM students;
```

| name   |
| ------ |
| Akhil  |
| Priya  |
| Nikhil |
| Sara   |

```sql
SELECT name, age FROM students;
```

| name   | age |
| ------ | --- |
| Akhil  | 21  |
| Priya  | 22  |
| Nikhil | 23  |
| Akhil  | 21  |
| Sara   | 22  |
| Priya  | 22  |
| Akhil  | 28  |

```sql
SELECT DISTINCT name, age FROM students;
```

| name   | age |
| ------ | --- |
| Akhil  | 21  |
| Priya  | 22  |
| Nikhil | 23  |
| Sara   | 22  |
| Akhil  | 28  |

```sql
SELECT name, city FROM students;
```

| name   | city      |
| ------ | --------- |
| Akhil  | Delhi     |
| Priya  | Mumbai    |
| Nikhil | Delhi     |
| Akhil  | Delhi     |
| Sara   | Mumbai    |
| Priya  | Bangalore |
| Akhil  | Delhi     |

```sql
SELECT DISTINCT name, city FROM students;
```

| name   | city      |
| ------ | --------- |
| Akhil  | Delhi     |
| Priya  | Mumbai    |
| Nikhil | Delhi     |
| Sara   | Mumbai    |
| Priya  | Bangalore |

---

### 🔢 Expressions in SELECT

```sql
SELECT age + 5 FROM students;
```

| age + 5 |
| ------- |
| 26      |
| 27      |
| 28      |
| 26      |
| 27      |
| 27      |
| 33      |

```sql
SELECT age + 5 AS futureAge FROM students;
```

| futureAge |
| --------- |
| 26        |
| 27        |
| 28        |
| 26        |
| 27        |
| 27        |
| 33        |

```sql
SELECT 100 * 2;
```

| ?column? |
| -------- |
| 200      |

```sql
SELECT 100 * 2 AS result;
```

| result |
| ------ |
| 200    |

```

---

```


## 📘 Table: `users`

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(40) NOT NULL,
    age INT NOT NULL
);
```

### ✅ Insert Records

```sql
INSERT INTO users (id, name) VALUES (1, 'akhil'); -- ❌ ERROR: age is NOT NULL
INSERT INTO users (id, name, age) VALUES 
(2, 'priya', 23),
(3, 'kanth', 24);
INSERT INTO users VALUES (4, 'nikhil', 25);
INSERT INTO users (id, age) VALUES (5, 26); -- ❌ ERROR: name is NOT NULL
INSERT INTO users (id, name) VALUES (6, 'sudha'); -- ❌ ERROR: age is NOT NULL
```

### 📋 Table Data Example

| id | name   | age |
|----|--------|-----|
| 2  | priya  | 23  |
| 3  | kanth  | 24  |
| 4  | nikhil | 25  |

---

## ✏️ UPDATE Operations

```sql
UPDATE users SET age = 25 WHERE name = 'akhil';
UPDATE users SET age = 18 WHERE age < 25;
UPDATE users SET age = 25;
```

### 🧾 Output (after full update)

| id | name   | age |
|----|--------|-----|
| 2  | priya  | 25  |
| 3  | kanth  | 25  |
| 4  | nikhil | 25  |

---

## ❌ DELETE Operations

```sql
DELETE FROM users WHERE age = 0;
DELETE FROM users WHERE id > 3;
DELETE FROM users;
```

### 🧾 Output (after deletions)

```sql
SELECT * FROM users;
-- Empty result set (0 rows)
```

---

## ✅ Summary

- Use `DISTINCT` to remove duplicate results.
- `WHERE` filters records based on specific conditions.
- `EXPRESSION` computes values using columns, literals, functions, or operators.
- `AS` renames columns or tables for readability or convenience.
  
- `INSERT` must respect NOT NULL constraints.
- `UPDATE` can conditionally or fully modify records.
- `DELETE` can target specific rows or remove all records.


---


# 🛡️ DATA CONTROL LANGUAGE

## 🧑‍💻 Listing Existing Users

```sql
SELECT user FROM mysql.user;
````

**✅ Sample Output:**

| user |
| ---- |
| root |

---

## 🔑 Login as Root User

```bash
mysql -u root -pPassword#123
```

---

## 👤 Create New Users(Login as root)

```sql
CREATE USER 'akhil'@'localhost' IDENTIFIED BY 'Akhil#123';
CREATE USER 'shini'@'localhost' IDENTIFIED BY 'Shini#123';
CREATE USER 'nikhil'@'localhost' IDENTIFIED BY 'Nikhil#123';
```

✅ After creation, list again:

```sql
SELECT user FROM mysql.user;
```

**✅ Sample Output:**

| user   |
| ------ |
| root   |
| akhil  |
| shini  |
| nikhil |

---

### 🔐 sample Login as akhil

```bash
mysql -u akhil -pAkhil#123
```

---
---

### 🔐 sample Login as shini

```bash
mysql -u shini -pShini#123
```

---

## 🟢 Grant Privileges(Login as root)

### Grant SHOW DATABASES


```sql
GRANT SHOW DATABASES ON *.* TO 'nikhil'@'localhost';
```

✅ Try listing databases:(Login as nikhil)

```sql
SHOW DATABASES;
```

**✅ Works fine.**

---

## 🔴 Revoke Privileges (Login as root)

### Revoke SHOW DATABASES

```sql
REVOKE SHOW DATABASES ON *.* FROM 'nikhil'@'localhost';
```

✅ Try again:(Login as nikhil)

```sql
SHOW DATABASES;
```

**❌ works but limited databases will be shown:**

```
only default databases will be shown
```

✅ This shows privilege was revoked.

---

## 🟢 Grant SELECT Privilege (Login as root)

```sql
GRANT SELECT ON war.* TO 'nikhil'@'localhost';
```

✅ Nikhil can now:(Login as nikhil)

```sql
USE war;
```

**✅ Works fine.**

---

## 🔴 Revoke SELECT Privilege(Login as root)

```sql
REVOKE SELECT ON war.* FROM 'nikhil'@'localhost';
```

✅ Try again:(Login as nikhil)

```sql
USE war;
```

**❌ Error Example:**

```
ERROR 1044 (42000): Access denied for user 'nikhil'@'localhost' to database 'war'
```

---

## 🟢 Grant INSERT Privilege(Login as root)

```sql
GRANT INSERT ON war.* TO 'akhil'@'localhost';
```

✅ Akhil can insert:

```sql
INSERT INTO users VALUES (1, 'nikhil', 23);
```

**✅ Works fine.**

---

## 🔴 🔒 Revoke INSERT Privilege(Login as root)

```sql
REVOKE INSERT ON war.* FROM 'akhil'@'localhost';
```

✅ Try again:

```sql
INSERT INTO users VALUES (1, 'akhil', 23);
```

**❌ Error Example:**

```
ERROR 1142 (42000): INSERT command denied to user 'akhil'@'localhost' for table 'users'
```

---

## ✅ Summary

* `GRANT` provides specific permissions.
* `REVOKE` removes those permissions.
* Always test by switching users and re-issuing commands.
* Use `SHOW GRANTS FOR 'USER'@'HOST';` to inspect granted permissions.

---

# Transaction Control Language (TCL)

---

**🟢 CREATE DATABASE AND TABLE**

```sql
CREATE DATABASE bank;
USE bank;

CREATE TABLE accounts (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    amount DECIMAL(10,2)
);
```

---

**🟢 INSERT INITIAL DATA**

```sql
INSERT INTO accounts VALUES (1, 'akhil', 1000.00);
INSERT INTO accounts VALUES (2, 'nikhil', 500.00);
```

---

**🟢 VERIFY TABLE DATA**

```sql
SELECT * FROM accounts;
```

Sample Output:

| id | name   | amount |
|----|--------|--------|
| 1  | akhil  | 1000.00 |
| 2  | nikhil | 500.00 |


---

**🟢 CHECK AUTOCOMMIT**

```sql
SELECT @@autocommit;
```

If it returns `1`, it means autocommit is ON.

---

**🟢 DISABLE AUTOCOMMIT**

```sql
SET autocommit=0;
```

---

**🟢 START TRANSACTION (ALTERNATIVE)**

Instead of `SET autocommit=0;` you can also start transaction explicitly:

```sql
START TRANSACTION;
```

or

```sql
BEGIN;
```

This will start a transaction block.

---

**🟢 SAVEPOINT 1 - ADD SHINI**

```sql
INSERT INTO accounts VALUES (3, 'shini', 500.00);
SAVEPOINT sp1;
```

---

**🟢 SAVEPOINT 2 - TRANSFER 200 FROM AKHIL TO NIKHIL**

```sql
UPDATE accounts SET amount = amount - 200 WHERE name = 'akhil';
UPDATE accounts SET amount = amount + 200 WHERE name = 'nikhil';
SAVEPOINT sp2;
```

---

**🟢 SAVEPOINT 3 - ADD HARSHA**

```sql
INSERT INTO accounts VALUES (4, 'harsha', 400.00);
SAVEPOINT sp3;
```

---

**🟢 SAVEPOINT 4 - UPDATE NAME TO HARSHA IN CAPS**

```sql
UPDATE accounts SET name = 'HARSHA' WHERE name = 'harsha';
SAVEPOINT sp4;
```

---

**🟢 ROLLBACK TO SAVEPOINT 3**

```sql
ROLLBACK TO sp3;
```

Effect:

* Name remains `harsha` in lowercase(original).

---

**🟢 ROLLBACK TO SAVEPOINT 1**

```sql
ROLLBACK TO sp1;
```

Effect:

* Transfer of 200 from AKHIL to NIKHIL will be undone.
* "harsha" removed.

---

**🟢 FULL ROLLBACK**

```sql
ROLLBACK;
```

Effect:

* All uncommitted changes discarded.
* Table back to:

| id | name   | amount |
| -- | ------ | ------ |
| 1  | akhil  | 1000.00 |
| 2  | nikhil | 500.00 |

---

**🟢 COMMIT CHANGES**

If you want to keep all uncommitted changes:

```sql
COMMIT;
```

---

**🟢 SELECT FINAL DATA**

```sql
SELECT * FROM accounts;
```

---

**✅ QUICK NOTES**

* Use `START TRANSACTION;` or `BEGIN;` to start transaction.
* `SAVEPOINT` marks points you can roll back to.
* `ROLLBACK TO spX;` undoes only after that savepoint.
* `ROLLBACK;` undoes everything uncommitted.
* `COMMIT;` saves everything permanently.

---


Perfect—let’s organize this **clearly** with headings, sample outputs, and a **short list of examples only (not every single task)** so it’s easier to paste in your notes.

---

## 🎯 MySQL Practice Notes

---

### 🟢 DATABASE & TABLE CREATION

```sql
CREATE DATABASE IF NOT EXISTS temp;
USE temp;

DROP TABLE IF EXISTS products;

CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    category VARCHAR(50),
    price DECIMAL(10,2),
    qty INT
);
```

---

### 🟢 INSERT SAMPLE DATA

```sql
INSERT INTO products (id, name, category, price, qty) VALUES
(1, 'Pen', 'Stationery', 10.00, 5),
(2, 'Pencil', 'Stationery', 5.00, 10),
(3, 'Notebook', 'Stationery', 25.00, 7),
(4, 'Eraser', 'Stationery', 3.00, 15),
(5, 'Marker', 'Stationery', 15.00, 8),
(6, 'Stapler', 'Office Supplies', 50.00, 2);
```

✅ **Sample Data**

| id | name     | category        | price | qty |
| -- | -------- | --------------- | ----- | --- |
| 1  | Pen      | Stationery      | 10.00 | 5   |
| 2  | Pencil   | Stationery      | 5.00  | 10  |
| 3  | Notebook | Stationery      | 25.00 | 7   |
| 4  | Eraser   | Stationery      | 3.00  | 15  |
| 5  | Marker   | Stationery      | 15.00 | 8   |
| 6  | Stapler  | Office Supplies | 50.00 | 2   |

---

## ⚙️ OPERATORS IN MYSQL

MySQL has several categories of operators:

---

### 🟢 1️⃣ Arithmetic Operators

Used for calculations.

| Operator | Meaning        |
| -------- | -------------- |
| `+`      | Addition       |
| `-`      | Subtraction    |
| `*`      | Multiplication |
| `/`      | Division       |

**Example: Calculate Total Value**

```sql
SELECT name, price, qty, price * qty AS total_value FROM products;
```

✅ **Sample Output**

| name     | price | qty | total\_value |
| -------- | ----- | --- | ------------ |
| Pen      | 10.00 | 5   | 50.00        |
| Pencil   | 5.00  | 10  | 50.00        |
| Notebook | 25.00 | 7   | 175.00       |

---

### 🟢 2️⃣ Relational Operators 
(`=` `!=`) can also be called as `comparision` operators

Used to compare values.

| Operator | Meaning                  |
| -------- | ------------------------ |
| `=`      | Equal to                 |
| `!=`     | Not equal to             |
| `<`      | Less than                |
| `>`      | Greater than             |
| `<=`     | Less than or equal to    |
| `>=`     | Greater than or equal to |

**Example: Products costing more than 20**

```sql
SELECT * FROM products WHERE price > 20;
```

✅ **Sample Output**

| id | name     | category        | price | qty |
| -- | -------- | --------------- | ----- | --- |
| 3  | Notebook | Stationery      | 25.00 | 7   |
| 6  | Stapler  | Office Supplies | 50.00 | 2   |

---

### 🟢 3️⃣ Logical Operators

Combine multiple conditions.

| Operator | Meaning             |
| -------- | ------------------- |
| `AND`    | All conditions true |
| `OR`     | At least one true   |
| `NOT`    | Negates a condition |

**Example: Stationery items with qty > 5**

```sql
SELECT * FROM products WHERE category='Stationery' AND qty > 5;
```

✅ **Sample Output**

| id | name     | category   | price | qty |
| -- | -------- | ---------- | ----- | --- |
| 2  | Pencil   | Stationery | 5.00  | 10  |
| 3  | Notebook | Stationery | 25.00 | 7   |
| 4  | Eraser   | Stationery | 3.00  | 15  |
| 5  | Marker   | Stationery | 15.00 | 8   |

---

### 🟢 4️⃣ Special Operators

These are commonly used for filtering patterns, sets, nulls, and ranges.

---

**🔹 IN / NOT IN**

Check if a value is in a set.

**Example: Orders with status in 'Delivered', 'Pending'**

```sql
SELECT * FROM orders WHERE status IN ('Delivered', 'Pending');
```

---

**🔹 IS / IS NOT**

Check for NULLs.

**Example: Orders where status is NULL**

```sql
SELECT * FROM orders WHERE status IS NULL;
```

✅ **Sample Output**

| id | user  | product     | qty | order\_date | status |
| -- | ----- | ----------- | --- | ----------- | ------ |
| 6  | Fiona | Scissors    | 2   | 2025-07-06  | NULL   |
| 10 | Julia | File Folder | 1   | 2025-07-10  | NULL   |

---

**🔹 LIKE / NOT LIKE**

Pattern matching.

| Symbol | Meaning                  |
| ------ | ------------------------ |
| `%`    | Any number of characters |
| `_`    | A single character       |

**Example: Users starting with 'A'**

```sql
SELECT * FROM orders WHERE user LIKE 'A%';
```

✅ **Sample Output**

| id | user  | product | qty | order\_date | status    |
| -- | ----- | ------- | --- | ----------- | --------- |
| 1  | Alice | Pen     | 5   | 2025-07-01  | Delivered |

---

**🔹 BETWEEN / NOT BETWEEN**

Range filtering (inclusive).

**Example: Orders with quantity between 2 and 5**

```sql
SELECT * FROM orders WHERE qty BETWEEN 2 AND 5;
```

✅ **Sample Output**

| id | user   | product  | qty | order\_date | status    |
| -- | ------ | -------- | --- | ----------- | --------- |
| 2  | Bob    | Notebook | 2   | 2025-07-02  | Pending   |
| 4  | Daisy  | Eraser   | 3   | 2025-07-04  | Cancelled |
| 8  | Hannah | Tape     | 4   | 2025-07-08  | Delivered |

---

✅ **Tip:** You can combine all these operators with `AND`, `OR`, and parentheses to build complex queries.

---




Great—let’s **organize all of this cleanly**, with example queries and explanations per topic.
Below is everything **structured by topic**:

---

## 🟢 1️⃣ Table Setup

```sql
USE temp;

DROP TABLE IF EXISTS users;

CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    amount DECIMAL(10, 2)
);

INSERT INTO users (id, name, amount) VALUES (1, 'john', 100.00);
INSERT INTO users (id, name, amount) VALUES (2, 'ALICE', 200.50);
INSERT INTO users (id, name, amount) VALUES (3, 'Bob_Smith', 150.25);
INSERT INTO users (id, name, amount) VALUES (4, 'Anna%Marie', 80.00);
INSERT INTO users (id, name, amount) VALUES (5, 'cHaRlie', 300.00);
INSERT INTO users (id, name, amount) VALUES (6, 'O''Connor', 120.00);
INSERT INTO users (id, name, amount) VALUES (7, 'Zoë', 250.75);
INSERT INTO users (id, name, amount) VALUES (8, 'MARCUS', 0.00);
INSERT INTO users (id, name, amount) VALUES (9, 'eve', 99.99);
INSERT INTO users (id, name, amount) VALUES (10, 'Back\\Slash', 175.50);
```

✅ **Sample Data Example**

| id | name       | amount |
| -- | ---------- | ------ |
| 1  | john       | 100.00 |
| 2  | ALICE      | 200.50 |
| 3  | Bob\_Smith | 150.25 |
| 4  | Anna%Marie | 80.00  |
| 5  | cHaRlie    | 300.00 |
| 6  | O'Connor   | 120.00 |
| 7  | Zoë        | 250.75 |
| 8  | MARCUS     | 0.00   |
| 9  | eve        | 99.99  |
| 10 | Back\Slash | 175.50 |

---

## 🟢 2️⃣ String Functions

---

### 🔹 LENGTH

**Get the length of each name:**

```sql
SELECT id, name, LENGTH(name) AS name_length FROM users;
```

---

### 🔹 UPPER

**Convert names to uppercase:**

```sql
SELECT id, UPPER(name) AS name_upper FROM users;
```

---

### 🔹 LOWER

**Convert names to lowercase:**

```sql
SELECT id, LOWER(name) AS name_lower FROM users;
```

---

### 🔹 CONCAT

**Concatenate name and amount:**

```sql
SELECT CONCAT(name, ' has $', amount) AS info FROM users;
```

---

### 🔹 REPLACE

**Replace underscores with spaces in names:**

```sql
SELECT name, REPLACE(name, '_', ' ') AS cleaned_name FROM users;
```

---

## 🟢 3️⃣ Aggregate Functions

---

### 🔹 MAX

**Highest amount:**

```sql
SELECT MAX(amount) AS max_amount FROM users;
```

---

### 🔹 MIN

**Lowest amount:**

```sql
SELECT MIN(amount) AS min_amount FROM users;
```

---

### 🔹 SUM

**Total of all amounts:**

```sql
SELECT SUM(amount) AS total_amount FROM users;
```

---

### 🔹 AVG

**Average amount:**

```sql
SELECT AVG(amount) AS avg_amount FROM users;
```

---

### 🔹 COUNT

**Number of users:**

```sql
SELECT COUNT(*) AS user_count FROM users;
```

---

## 🟢 4️⃣ Escape Characters in LIKE

Escape characters help when the search pattern includes special symbols (%, \_, ).

---

✅ **Escape Backslash (`\`):**
Find names containing a literal backslash:

```sql
SELECT * FROM users WHERE name LIKE '%\\%' ESCAPE '\\';
```

---

✅ **Escape Percent (`%`) with `$`:**
Find names containing literal `%` character:

```sql
SELECT * FROM users WHERE name LIKE '%$%%' ESCAPE '$';
```

*This matches `Anna%Marie`.*

---

✅ **Escape Underscore (`_`) with `!`:**
Find names containing literal `_` character:

```sql
SELECT * FROM users WHERE name LIKE '%!_%' ESCAPE '!';
```

*This matches `Bob_Smith`.*

---

✅ **Escape Backslash with `^`:**
Find names containing backslash, using `^` as escape:

```sql
SELECT * FROM users WHERE name LIKE '%^\\%' ESCAPE '^';
```

*This matches `Back\Slash`.*

---


✅ **General Tip:**

* `%` = any sequence of characters
* `_` = any single character
* ESCAPE defines what character signals a literal
* If you see `Anna%Marie`, you must escape `%`
* If you see `Back\Slash`, you must escape `\`

---

Let me know if you’d like me to generate more examples!



