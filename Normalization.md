Excellent question. Before learning **JOINs**, it's very helpful to understand **normalization**, because **normalization is the reason JOINs exist**.

---

# Why Normalization?

Imagine you're running a small company and store everything in one Excel sheet.

## Unnormalized Table (Bad Design)

| OrderID | Customer | Phone    | Product1 | Product2 | Product3 | Salesperson | Department |
| ------- | -------- | -------- | -------- | -------- | -------- | ----------- | ---------- |
| 1001    | John     | 555-1111 | Laptop   | Mouse    | Bag      | Alice       | IT         |
| 1002    | Mary     | 555-2222 | Keyboard | NULL     | NULL     | Bob         | Sales      |

Problems:

* Customer phone is repeated in every order.
* Product columns (Product1, Product2, Product3) are fixed—you can't easily add a fourth product.
* If Alice moves to another department, you must update many rows.
* If you delete John's only order, you lose his phone number too.

This leads to:

* **Duplicate data**
* **Wasted storage**
* **Update mistakes**
* **Insert/Delete problems**

Normalization fixes these issues.

---

# What is Normalization?

Normalization is the process of organizing data into multiple related tables to:

* Reduce redundancy (duplicate data)
* Improve consistency
* Prevent update, insert, and delete anomalies
* Make the database easier to maintain

---

# 1NF (First Normal Form)

## Rule

Every column must contain **one value only** (atomic values). No repeating groups or lists.

### Bad (Not 1NF)

| Student | Courses                |
| ------- | ---------------------- |
| Alice   | Math, Science, English |

One cell contains multiple values.

### Good (1NF)

| Student | Course  |
| ------- | ------- |
| Alice   | Math    |
| Alice   | Science |
| Alice   | English |

Each cell has a single value.

---

## Another Example

### Bad

| Employee | Phone Numbers      |
| -------- | ------------------ |
| Bob      | 555-1111, 555-2222 |

### Good

| Employee | Phone    |
| -------- | -------- |
| Bob      | 555-1111 |
| Bob      | 555-2222 |

---

# 2NF (Second Normal Form)

**Must already be in 1NF.**

## Rule

Every non-key column must depend on the **entire primary key**, not just part of it.

This mostly matters when a table has a **composite primary key** (more than one column).

---

## Example

Suppose students enroll in courses.

Primary Key:

```
(StudentID, CourseID)
```

Table:

| StudentID | CourseID | StudentName | CourseName | Grade |
| --------- | -------- | ----------- | ---------- | ----- |
| 101       | DB101    | Alice       | Database   | A     |
| 101       | JAVA1    | Alice       | Java       | B     |

What depends on what?

* StudentName depends only on StudentID.
* CourseName depends only on CourseID.
* Grade depends on both StudentID and CourseID.

So StudentName and CourseName don't belong here.

---

Split it into:

### Students

| StudentID | StudentName |
| --------- | ----------- |
| 101       | Alice       |

### Courses

| CourseID | CourseName |
| -------- | ---------- |
| DB101    | Database   |
| JAVA1    | Java       |

### Enrollment

| StudentID | CourseID | Grade |
| --------- | -------- | ----- |
| 101       | DB101    | A     |
| 101       | JAVA1    | B     |

Now every non-key column depends on the whole primary key.

---

# 3NF (Third Normal Form)

Must already be in 2NF.

## Rule

Non-key columns should **not depend on other non-key columns**.

In other words:

> A non-key attribute should depend only on the primary key.

---

## Bad Example

| EmployeeID | EmployeeName | DepartmentID | DepartmentName |
| ---------- | ------------ | ------------ | -------------- |
| 101        | Alice        | 10           | IT             |
| 102        | Bob          | 20           | Sales          |

Primary Key:

```
EmployeeID
```

Notice:

DepartmentName depends on DepartmentID.

It does **not** depend directly on EmployeeID.

This is called a **transitive dependency**.

---

Split it into:

### Employees

| EmployeeID | EmployeeName | DepartmentID |
| ---------- | ------------ | ------------ |
| 101        | Alice        | 10           |
| 102        | Bob          | 20           |

### Departments

| DepartmentID | DepartmentName |
| ------------ | -------------- |
| 10           | IT             |
| 20           | Sales          |

Now DepartmentName depends on DepartmentID in its own table.

---

# BCNF (Boyce-Codd Normal Form)

BCNF is a stricter version of 3NF.

## Rule

Every determinant must be a candidate key.

A determinant is a column (or set of columns) that uniquely determines another column.

---

## Example

Suppose in a university:

Rules:

* Every Course has one Instructor.
* Every Instructor teaches only one Course.

Table:

| Student | Course  | Instructor |
| ------- | ------- | ---------- |
| Alice   | Math    | Smith      |
| Bob     | Math    | Smith      |
| John    | Physics | Jones      |

Functional dependencies:

```
Course → Instructor

Instructor → Course
```

But suppose the primary key is:

```
(Student, Course)
```

Notice:

Instructor determines Course.

Instructor is **not** a candidate key in this table.

This violates BCNF.

---

Split it into:

### Instructor

| Instructor | Course  |
| ---------- | ------- |
| Smith      | Math    |
| Jones      | Physics |

### Enrollment

| Student | Course  |
| ------- | ------- |
| Alice   | Math    |
| Bob     | Math    |
| John    | Physics |

Now every determinant is a candidate key.

---

# Normalization Flow

```text
Unnormalized Data
        │
        ▼
1NF
• One value per cell
• No repeating groups
        │
        ▼
2NF
• Remove partial dependencies
• Separate data that depends on only part of a composite key
        │
        ▼
3NF
• Remove transitive dependencies
• Non-key columns depend only on the primary key
        │
        ▼
BCNF
• Every determinant is a candidate key
```

---

# Real-World Example: Amazon

Instead of one giant table:

| OrderID | CustomerName | CustomerPhone | Product | Supplier | SupplierPhone |
| ------- | ------------ | ------------- | ------- | -------- | ------------- |

Normalize into:

```text
Customers
-----------
CustomerID
Name
Phone

Orders
--------
OrderID
CustomerID

Products
----------
ProductID
ProductName
SupplierID

Suppliers
-----------
SupplierID
SupplierName
Phone
```

Now, to see an order with the customer's name and product details, the database uses **JOINs** to bring the related information back together.

---

# Summary Table

| Normal Form | Main Rule                                      | Fixes                                                     |
| ----------- | ---------------------------------------------- | --------------------------------------------------------- |
| **1NF**     | One value per cell; no repeating groups        | Eliminates multi-valued columns                           |
| **2NF**     | Every non-key column depends on the whole key  | Removes partial dependencies (composite keys)             |
| **3NF**     | Non-key columns depend only on the primary key | Removes transitive dependencies                           |
| **BCNF**    | Every determinant must be a candidate key      | Handles special dependency cases that 3NF can still allow |

### An easy way to remember

* **1NF:** "One fact per cell."
* **2NF:** "Every non-key fact depends on the whole key."
* **3NF:** "Non-key facts don't depend on other non-key facts."
* **BCNF:** "Only keys determine other data."

Once you understand normalization, the purpose of SQL joins becomes much clearer: **normalization splits related data into multiple tables to avoid duplication, and joins are the mechanism for combining that data back together when you need it.**
