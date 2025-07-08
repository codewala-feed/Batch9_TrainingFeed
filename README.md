
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




