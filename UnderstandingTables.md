Perfect! Let's pretend you've just joined a company as a Junior Database Developer. I'll explain this table exactly as I would to a new team member.

---

# Step 1: What is this table?

The table is called **employees**.

Think of it like an Excel spreadsheet.

| Employee ID | First Name | Last Name | Email                                     | Job Title | Salary | Hire Date  | Department |
| ----------- | ---------- | --------- | ----------------------------------------- | --------- | ------ | ---------- | ---------- |
| 1           | John       | Smith     | [john@gmail.com](mailto:john@gmail.com)   | Developer | 75000  | 2024-01-15 | IT         |
| 2           | Alice      | Brown     | [alice@gmail.com](mailto:alice@gmail.com) | Manager   | 95000  | 2023-10-10 | HR         |

Each **row** represents one employee.

Each **column** stores one piece of information.

---

# Step 2: CREATE TABLE

```sql
CREATE TABLE employees (
```

This tells MariaDB:

> "Create a new table called `employees`."

Think of it as creating a new Excel sheet named **Employees**.

---

# Step 3: employee_id

```sql
employee_id INT(11) NOT NULL AUTO_INCREMENT
```

Let's break this into pieces.

### INT

```sql
INT
```

Means this column stores whole numbers.

Examples:

```
1
25
999
10000
```

Not allowed:

```
John
75.50
Hello
```

---

### NOT NULL

```sql
NOT NULL
```

Means the value is **mandatory**.

Every employee **must** have an ID.

Not allowed:

| employee_id |
| ----------- |
| NULL        |

---

### AUTO_INCREMENT

```sql
AUTO_INCREMENT
```

MariaDB automatically generates the next number.

Suppose the table is

| employee_id |
| ----------- |
| 1           |
| 2           |
| 3           |

You insert

```sql
INSERT INTO employees(first_name,last_name)
VALUES('David','Wilson');
```

MariaDB automatically creates

| employee_id |
| ----------- |
| 4           |

You never type the ID yourself.

Think of it like a token machine at a bank:

```
Customer 1

Customer 2

Customer 3

Customer 4
```

The machine gives the next number automatically.

---

# Step 4: first_name

```sql
first_name VARCHAR(50) NOT NULL
```

### VARCHAR

Variable-length text.

Maximum length:

```
50 characters
```

Examples

```
John

Alexander

Maria
```

---

Why VARCHAR instead of CHAR?

Suppose the name is

```
Tom
```

CHAR(50)

```
Tom_____________________________________
```

47 spaces are wasted.

VARCHAR

```
Tom
```

Only stores what is needed.

---

# Step 5: last_name

Exactly the same idea.

```
Smith

Brown

Johnson
```

---

# Step 6: email

```sql
email VARCHAR(100)
```

Stores

```
john@gmail.com

alice@yahoo.com

mike@hotmail.com
```

Notice

```
DEFAULT NULL
```

means email is optional.

Some employees may not have one.

---

# Step 7: job_title

```sql
job_title VARCHAR(100)
```

Stores

```
Software Engineer

Manager

HR Executive

Database Administrator
```

---

# Step 8: salary

```sql
salary DECIMAL(10,2)
```

This is very important.

Never store money using FLOAT.

Use DECIMAL.

Examples

```
55000.00

123456.78

750.50
```

The `(10,2)` means:

```
10 total digits

2 digits after decimal
```

Largest value

```
99999999.99
```

---

# Step 9: hire_date

```sql
hire_date DATE
```

Stores

```
2024-05-15

2023-11-20

2025-01-01
```

No time.

Only the date.

---

# Step 10: department_id

```sql
department_id INT
```

Instead of storing

```
Information Technology
```

the table stores

```
3
```

Because another table already contains department details.

Example

Departments table

| department_id | department_name |
| ------------- | --------------- |
| 1             | HR              |
| 2             | Finance         |
| 3             | IT              |
| 4             | Marketing       |

Employee table

| employee | department_id |
| -------- | ------------- |
| John     | 3             |
| Alice    | 1             |

This avoids repeating long department names.

---

# Step 11: Primary Key

```sql
PRIMARY KEY(employee_id)
```

This is the most important column.

Rules

* Cannot be NULL
* Must be unique

Good

| ID |
| -- |
| 1  |
| 2  |
| 3  |

Bad

| ID |
| -- |
| 1  |
| 1  |

Duplicate IDs are not allowed.

Think of it like:

* Passport Number
* Aadhaar Number
* SSN

Every person has a unique identifier.

---

# Step 12: Unique Key

```sql
UNIQUE KEY(email)
```

Emails must also be unique.

Allowed

```
john@gmail.com

alice@gmail.com
```

Not allowed

```
john@gmail.com

john@gmail.com
```

MariaDB gives

```
Duplicate entry
```

---

# Step 13: Index

```sql
KEY(department_id)
```

Imagine this table grows to **10 million employees**.

Without an index:

```
John
Alice
Bob
Mike
...
10 million rows
```

Query

```sql
SELECT *
FROM employees
WHERE department_id = 3;
```

MariaDB reads

```
Row 1

Row 2

Row 3

...

Row 10,000,000
```

Very slow.

---

With an index

MariaDB creates something like

```
Department 1 → rows ...

Department 2 → rows ...

Department 3 → rows ...
```

It jumps directly to department 3.

Much faster.

---

# Step 14: Foreign Key

```sql
FOREIGN KEY(department_id)
REFERENCES departments(department_id)
```

This creates a relationship.

Employees

| employee | department_id |
| -------- | ------------- |
| John     | 3             |
| Alice    | 1             |

Departments

| department_id | name    |
| ------------- | ------- |
| 1             | HR      |
| 2             | Finance |
| 3             | IT      |

If you try

```sql
INSERT INTO employees
VALUES (...,99);
```

MariaDB checks

```
Does department 99 exist?
```

No.

Result

```
ERROR
```

This prevents invalid references.

---

# Step 15: ENGINE=InnoDB

```sql
ENGINE=InnoDB
```

This tells MariaDB how to store the table.

InnoDB provides:

* Transactions (`COMMIT` and `ROLLBACK`)
* Foreign key support
* Row-level locking
* Crash recovery
* High concurrency for many users

It's the recommended engine for almost all new applications.

---

# Step 16: Character Set

```sql
CHARSET=utf8mb4
```

This allows text from many languages and symbols.

Examples:

```
English

தமிழ்

日本語

😊
```

Without `utf8mb4`, some characters (especially emojis) may not be stored correctly.

---

# How this table fits into a database

```text
                departments
          +----------------------+
          | department_id (PK)   |
          | department_name      |
          +----------+-----------+
                     |
                     | 1 department
                     |
                     | has many employees
                     |
          +----------v-----------+
          | employees            |
          | employee_id (PK)     |
          | first_name           |
          | last_name            |
          | email (UNIQUE)       |
          | job_title            |
          | salary               |
          | hire_date            |
          | department_id (FK)   |
          +----------------------+
```

## What to learn next

Now that you understand the table structure, the next topics I'd recommend are:

1. `INSERT` – Add employee records.
2. `SELECT` – Retrieve data.
3. `WHERE` – Filter rows.
4. `ORDER BY` – Sort results.
5. `JOIN` – Combine `employees` with `departments`.
6. **Indexes** – Learn how they speed up queries and how to read `EXPLAIN`.
7. **Transactions** – Understand `COMMIT` and `ROLLBACK`.
8. **Normalization** – Learn why data is split into related tables.

These topics build naturally on the table you've shared and will give you a solid foundation in MariaDB.
