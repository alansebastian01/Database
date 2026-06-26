A **database** is an organized collection of data that is stored electronically so it can be easily accessed, managed, updated, and analyzed. Databases are used in almost every application, from banking systems and online stores to social media and university management systems.

For example, a university database might store:

* Student information
* Course details
* Instructor information
* Enrollment records
* Grades

## Main Parts of a Database

### 1. Tables

A **table** is where data is stored. It is similar to a spreadsheet with rows and columns.

Example: **Students Table**

| StudentID | Name  | Major | GPA |
| --------- | ----- | ----- | --- |
| 101       | Alice | CS    | 3.8 |
| 102       | Bob   | Math  | 3.5 |

Each table represents one type of information.

---

### 2. Rows (Records/Tuples)

A **row** represents one complete item or record.

Example:

| StudentID | Name  | Major |
| --------- | ----- | ----- |
| 101       | Alice | CS    |

This row contains all the information about one student.

---

### 3. Columns (Fields/Attributes)

A **column** represents one type of data.

Example:

| StudentID | Name | Major |
| --------- | ---- | ----- |

* StudentID
* Name
* Major

Each column has a specific data type (number, text, date, etc.).

---

### 4. Primary Key

A **primary key** uniquely identifies each row in a table.

Example:

| StudentID (Primary Key) | Name  |
| ----------------------- | ----- |
| 101                     | Alice |
| 102                     | Bob   |

No two students can have the same StudentID.

---

### 5. Foreign Key

A **foreign key** connects one table to another.

Example:

**Students**

| StudentID | Name  |
| --------- | ----- |
| 101       | Alice |

**Enrollments**

| EnrollmentID | StudentID | CourseID |
| ------------ | --------- | -------- |
| 1            | 101       | CS146    |

Here, `StudentID` in the **Enrollments** table is a foreign key that refers to the **Students** table.

---

### 6. Relationships

Relationships define how tables are connected.

Common relationship types:

**One-to-One (1:1)**

* One person has one passport.

**One-to-Many (1:N)**

* One professor teaches many courses.

**Many-to-Many (M:N)**

* Many students enroll in many courses.
* This is usually implemented with a junction table (like an `Enrollments` table).

---

### 7. Schema

A **schema** is the blueprint of the database. It defines:

* Tables
* Columns
* Data types
* Relationships
* Constraints

Think of it as the architectural plan for the database.

---

### 8. Constraints

Constraints enforce rules to keep data accurate.

Examples:

* **NOT NULL** – A value is required.
* **UNIQUE** – No duplicate values.
* **PRIMARY KEY** – Must be unique and not null.
* **FOREIGN KEY** – Must match a value in another table.
* **CHECK** – Ensures values meet a condition (e.g., GPA between 0.0 and 4.0).

---

### 9. Indexes

An **index** helps the database find data faster, much like an index in a book.

Without an index:

* The database may need to scan every row.

With an index:

* It can quickly locate matching records.

---

### 10. Queries

A **query** is a request for data, usually written in SQL (Structured Query Language).

Example:

```sql
SELECT Name, GPA
FROM Students
WHERE GPA > 3.5;
```

This retrieves students with a GPA greater than 3.5.

---

### 11. Views

A **view** is a virtual table created from one or more queries. It doesn't usually store data itself but presents data in a useful way.

Example:

* A view showing only students with a GPA above 3.5.

---

### 12. Database Management System (DBMS)

A **DBMS** is the software used to create, manage, and interact with databases.

Popular DBMSs include:

* MySQL
* PostgreSQL
* Oracle Database
* Microsoft SQL Server
* SQLite
* MongoDB (a NoSQL database)

---

## How the Parts Work Together

Imagine a university database:

```
                DATABASE
                    │
      ┌─────────────┼─────────────┐
      │             │             │
  Students      Courses      Enrollments
      │             │             │
      │             │             │
 StudentID ◄────────┴────────► CourseID
 (Primary Key)               (Primary Key)
         ▲
         │
     Foreign Key
```

* **Students** stores student information.
* **Courses** stores course information.
* **Enrollments** links students and courses using foreign keys.
* SQL queries retrieve or update data across these related tables.

## Summary

| Part           | Purpose                            |
| -------------- | ---------------------------------- |
| Database       | Collection of organized data       |
| Table          | Stores related data                |
| Row (Record)   | One item of data                   |
| Column (Field) | One attribute of the data          |
| Primary Key    | Uniquely identifies a row          |
| Foreign Key    | Links tables together              |
| Relationship   | Defines connections between tables |
| Schema         | Overall database structure         |
| Constraints    | Rules that ensure data integrity   |
| Index          | Speeds up data retrieval           |
| Query          | Retrieves or modifies data         |
| View           | Virtual table based on a query     |
| DBMS           | Software that manages the database |

A helpful way to think about it is that a **database is like a digital filing cabinet**: the database is the cabinet, tables are the folders, rows are individual documents, columns are the fields on each document, and keys and relationships tell the system how documents in different folders are connected.
