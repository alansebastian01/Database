Absolutely! In fact, **diagrams are the best way to learn JOINs**. Below is the same example with visual diagrams showing what each join returns.

---

# Step 1: Our Tables

## Customers

```
+-------------+--------------+
| customer_id | customer_name|
+-------------+--------------+
|      1      | John         |
|      2      | Maria        |
|      3      | Kevin        |
|      4      | Sara         |
+-------------+--------------+
```

## Orders

```
+----------+-------------+--------+
| order_id | customer_id | amount |
+----------+-------------+--------+
|   1001   |      1      | 250    |
|   1002   |      1      | 125    |
|   1003   |      2      | 300    |
|   1004   |      5      | 90     | <-- Customer doesn't exist
+----------+-------------+--------+
```

Relationship

```
Customers                 Orders

1 John     -------------> 1001
1 John     -------------> 1002

2 Maria    -------------> 1003

3 Kevin    (no orders)

4 Sara     (no orders)

Customer 5 doesn't exist
                 |
                 +-----> Order 1004
```

---

# 1 INNER JOIN

SQL

```sql
SELECT *
FROM customers c
INNER JOIN orders o
ON c.customer_id = o.customer_id;
```

Diagram

```
Customers           Orders

+------+            +------+
| John |<---------->|1001 |
|      |<---------->|1002 |
|Maria |<---------->|1003 |
|Kevin |            |     |
|Sara  |            |1004 | (no customer)
+------+            +------+

Only the matches are returned.
```

Result

```
John   1001
John   1002
Maria  1003
```

Think:

> Give me only rows where both tables match.

---

# 2 LEFT JOIN

SQL

```sql
SELECT *
FROM customers c
LEFT JOIN orders o
ON c.customer_id = o.customer_id;
```

Diagram

```
Customers (keep ALL)

John  ------>1001
      ------>1002

Maria ----->1003

Kevin -----> NULL

Sara  -----> NULL

Order 1004 ignored
```

Result

```
John   1001
John   1002
Maria  1003
Kevin  NULL
Sara   NULL
```

Think:

> Keep everything on the LEFT table.

---

# 3 RIGHT JOIN

SQL

```sql
SELECT *
FROM customers c
RIGHT JOIN orders o
ON c.customer_id = o.customer_id;
```

Diagram

```
Orders (keep ALL)

1001 <------ John

1002 <------ John

1003 <------ Maria

1004 <------ NULL
```

Result

```
John   1001
John   1002
Maria  1003
NULL   1004
```

Think:

> Keep everything on the RIGHT table.

---

# 4 FULL OUTER JOIN

MariaDB doesn't support it directly.

Conceptually:

```
Customers               Orders

John    <---------->1001

John    <---------->1002

Maria   <---------->1003

Kevin

Sara

                     1004
```

Result

```
John   1001
John   1002
Maria  1003
Kevin  NULL
Sara   NULL
NULL   1004
```

Everything from both tables.

---

# 5 CROSS JOIN

SQL

```sql
SELECT *
FROM customers
CROSS JOIN departments;
```

Suppose Departments

```
HR
IT
Finance
```

Diagram

```
John   HR
John   IT
John   Finance

Maria  HR
Maria  IT
Maria  Finance

Kevin  HR
Kevin  IT
Kevin  Finance

Sara   HR
Sara   IT
Sara   Finance
```

Every customer combines with every department.

Formula

```
Rows Returned =
Rows(Table A)
×

Rows(Table B)
```

Example

```
4 customers

×

3 departments

=

12 rows
```

---

# 6 SELF JOIN

Employees

```
+----+---------+------------+
| ID | Name    | Manager_ID |
+----+---------+------------+
|101 | Alice   | NULL       |
|102 | Bob     | NULL       |
|103 | Charlie | 102        |
|104 | Diana   | NULL       |
+----+---------+------------+
```

Diagram

```
Employees Table

Charlie
    |
    | manager_id = 102
    V
Bob

Alice (no manager)

Diana (no manager)
```

SQL

```sql
SELECT
e.employee_name,
m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.employee_id;
```

Result

```
Employee    Manager

Alice       NULL

Bob         NULL

Charlie     Bob

Diana       NULL
```

---

# 7 Multiple JOINs

```
Departments

1 HR

2 IT

3 Finance
```

Employees

```
Alice -> HR

Bob -> IT

Charlie -> IT
```

Diagram

```
Departments
     ^
     |
department_id
     |
Employees
     |
customer_id
     |
Orders
```

SQL

```sql
SELECT
e.employee_name,
d.department_name
FROM employees e
JOIN departments d
ON e.department_id=d.department_id;
```

Output

```
Alice      HR

Bob        IT

Charlie    IT
```

---

# 8 JOIN Flow Visualization

When SQL executes

```
Customers

      |
      |
      | customer_id
      V

Orders

      |
      |
      | Filter (WHERE)
      V

GROUP BY

      |
      V

ORDER BY
```

---

# Visual Summary

```
INNER JOIN

A ●●●●
   XXXX
B ●●●●

Only intersection


LEFT JOIN

A ●●●●●●
   XXXX
B ●●●

Everything from A


RIGHT JOIN

A ●●●
   XXXX
B ●●●●●●

Everything from B


FULL JOIN

A ●●●●●●
   XXXX
B ●●●●●●

Everything


CROSS JOIN

A × B

Cartesian Product


SELF JOIN

Employee
     |
     +------ Employee

Same table joined to itself.
```

## A great next step

Once you're comfortable with these basics, I recommend learning joins using a **real-world company database** (Customers → Orders → Order Items → Products → Categories → Suppliers). That lets you practice 2-table, 3-table, 4-table, and even 5-table joins, which is how joins are commonly used in real applications. I can also create a complete practice database with 8–10 related tables and 100+ sample rows for hands-on learning in MariaDB.
