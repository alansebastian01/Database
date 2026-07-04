Here’s a complete beginner-friendly MariaDB practice script you can run in **DBeaver**.

```sql
CREATE DATABASE join_practice;
USE join_practice;

DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS employees;
DROP TABLE IF EXISTS departments;
DROP TABLE IF EXISTS customers;

CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50) NOT NULL
);

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50) NOT NULL,
    department_id INT,
    manager_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id),
    FOREIGN KEY (manager_id) REFERENCES employees(employee_id)
);

CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(50) NOT NULL,
    city VARCHAR(50)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

Load sample data:

```sql
INSERT INTO departments VALUES
(1, 'HR'),
(2, 'IT'),
(3, 'Finance'),
(4, 'Marketing');

INSERT INTO employees VALUES
(101, 'Alice', 1, NULL),
(102, 'Bob', 2, NULL),
(103, 'Charlie', 2, 102),
(104, 'Diana', 3, NULL),
(105, 'Ethan', NULL, NULL);

INSERT INTO customers VALUES
(1, 'John', 'San Jose'),
(2, 'Maria', 'Fremont'),
(3, 'Kevin', 'Oakland'),
(4, 'Sara', 'San Jose');

INSERT INTO orders VALUES
(1001, 1, '2026-01-10', 250.00),
(1002, 1, '2026-01-15', 125.50),
(1003, 2, '2026-02-01', 300.00),
(1004, 5, '2026-02-10', 90.00);
```

Note: the last order has `customer_id = 5`, which does not exist. Because of the foreign key, MariaDB will reject it. For join practice with unmatched orders, use this version instead:

```sql
DROP TABLE IF EXISTS orders;

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    amount DECIMAL(10,2)
);

INSERT INTO orders VALUES
(1001, 1, '2026-01-10', 250.00),
(1002, 1, '2026-01-15', 125.50),
(1003, 2, '2026-02-01', 300.00),
(1004, 5, '2026-02-10', 90.00);
```

## 1. INNER JOIN

Shows only matching rows from both tables.

```sql
SELECT
    c.customer_id,
    c.customer_name,
    o.order_id,
    o.amount
FROM customers c
INNER JOIN orders o
    ON c.customer_id = o.customer_id;
```

## 2. LEFT JOIN

Shows all customers, even if they have no orders.

```sql
SELECT
    c.customer_id,
    c.customer_name,
    o.order_id,
    o.amount
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id;
```

## 3. RIGHT JOIN

Shows all orders, even if the customer does not exist.

```sql
SELECT
    c.customer_id,
    c.customer_name,
    o.order_id,
    o.amount
FROM customers c
RIGHT JOIN orders o
    ON c.customer_id = o.customer_id;
```

## 4. Find customers with no orders

```sql
SELECT
    c.customer_id,
    c.customer_name
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

## 5. Find orders with no matching customer

```sql
SELECT
    o.order_id,
    o.customer_id,
    o.amount
FROM orders o
LEFT JOIN customers c
    ON o.customer_id = c.customer_id
WHERE c.customer_id IS NULL;
```

## 6. FULL OUTER JOIN equivalent in MariaDB

MariaDB does not directly support `FULL OUTER JOIN`, so use `UNION`.

```sql
SELECT
    c.customer_id,
    c.customer_name,
    o.order_id,
    o.amount
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id

UNION

SELECT
    c.customer_id,
    c.customer_name,
    o.order_id,
    o.amount
FROM customers c
RIGHT JOIN orders o
    ON c.customer_id = o.customer_id;
```

## 7. JOIN between employees and departments

```sql
SELECT
    e.employee_id,
    e.employee_name,
    d.department_name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id;
```

## 8. LEFT JOIN employees and departments

Shows employees even if they do not belong to a department.

```sql
SELECT
    e.employee_id,
    e.employee_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

## 9. Departments with no employees

```sql
SELECT
    d.department_id,
    d.department_name
FROM departments d
LEFT JOIN employees e
    ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
```

## 10. SELF JOIN

Used when a table joins to itself. Here, employees join to their managers.

```sql
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

## 11. CROSS JOIN

Combines every row from one table with every row from another table.

```sql
SELECT
    c.customer_name,
    d.department_name
FROM customers c
CROSS JOIN departments d;
```

## 12. JOIN with GROUP BY

Total order amount per customer.

```sql
SELECT
    c.customer_id,
    c.customer_name,
    SUM(o.amount) AS total_spent
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY
    c.customer_id,
    c.customer_name;
```

## Best learning order

Start with:

```sql
INNER JOIN
LEFT JOIN
RIGHT JOIN
FULL OUTER JOIN using UNION
SELF JOIN
CROSS JOIN
GROUP BY with JOIN
```

The most important one for real work is usually **LEFT JOIN**.
