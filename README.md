# MySQL Notes

## 📘 Introduction to Database

- **Paper ➝ Notepad ➝ Excel ➝ SQL, NoSQL**

---

## 🆚 DBMS vs RDBMS

| Feature | DBMS | RDBMS |
|--------|------|--------|
| Example | File Storage System (Excel) | SQL-Based System |
| Data Organization | Flat Files | Tables with Relationships |

---

### Hospital Example

**DBMS Representation:**

| pid | pname | disease | did | dname | dexp | dcontact | ddept |
|-----|--------|----------|-----|--------|------|----------|--------|
|     |        |          |     |        |      |          |        |

**RDBMS Representation (Normalized Tables):**

- **Patients Table**
```

pid, pname, disease, did

```

- **Doctors Table**
```

did, dname, dexp, dcontact, ddeptid

```

- **Departments Table**
```

ddeptid, deptname

````

---

## 🔁 SQL vs NoSQL

| Feature | SQL | NoSQL |
|---------|-----|--------|
| Full Form | Structured Query Language | Not Only SQL |
| Structure | Tables (Relational) | Collections/Documents (Non-Relational) |

---

## 🧰 Installation of MySQL

No specific instructions mentioned, but typically involves downloading from [MySQL Official Website](https://dev.mysql.com/) and running the installer.

---

## 📊 Flow of Data Creation

| Operation | SQL | NoSQL |
|----------|-----|--------|
| Structure | databases ➝ tables ➝ rows/records | databases ➝ collections ➝ documents |

---

# 🗓️ Day 2

## 🔐 Login Command

```bash
mysql -uroot -pPassword#123
````

## 📦 Sample Data

```
1001, Akhil, Ch, 987654, 25, 25-12-2000, 23784.23, akhil@gmail.com
```

## 🔠 Data Types

| Type                        | Example               |
| --------------------------- | --------------------- |
| `VARCHAR`                   | Variable length text  |
| `CHAR`                      | Fixed length text     |
| `DATE`                      | `16-11-2001`          |
| `DATETIME`                  | `16-11-2001 15:34:15` |
| `INT`                       | Whole number          |
| `DECIMAL(precision, scale)` | e.g., `23784.23`      |

## 🔐 Constraints (Rules)

| Constraint    | Description                                  |
| ------------- | -------------------------------------------- |
| `PRIMARY KEY` | Not null, unique. Only one allowed per table |
| `NOT NULL`    | Cell cannot be empty                         |
| `UNIQUE`      | Can be empty, but no duplicates              |
| `DEFAULT`     | Fills with default value if not provided     |
| `FOREIGN KEY` | Links two tables on a common column          |
| `CHECK`       | Applies condition-based rule                 |

---

# 🗓️ Day 3

## 💡 SQL Language Categories

| Category | Full Form                    | Examples                               |
| -------- | ---------------------------- | -------------------------------------- |
| DDL      | Data Definition Language     | `CREATE`, `RENAME`, `DROP`, `TRUNCATE` |
| DML      | Data Manipulation Language   | `INSERT`, `UPDATE`, `DELETE`           |
| TCL      | Transaction Control Language | `COMMIT`, `ROLLBACK`, `SAVEPOINT`      |
| DCL      | Data Control Language        | `GRANT`, `REVOKE`                      |
| DQL      | Data Query Language          | `SELECT`                               |

---


###### Login Again

```bash
mysql -uroot -p
```

## DDL(`CREATE`, `ALTER`, `DROP`, `TRUNCATE`)
### Create Database

```sql
CREATE DATABASE batch9;
SHOW DATABASES;
USE batch9;
```

### Create Tables

```sql
CREATE TABLE users (
    uid INT,
    name VARCHAR(20),
    salary DECIMAL(10, 2)
);
```

```sql
CREATE TABLE users2 (
    uid INT PRIMARY KEY,
    name VARCHAR(20) NOT NULL,
    salary DECIMAL(10, 2) NOT NULL
);
```

---
