
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


