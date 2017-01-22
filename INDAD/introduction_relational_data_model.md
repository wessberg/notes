# Introduction, Relational data model
> [RG] 1, 3.1, 3.2, 3.3, 3.4, 5.2

### Views
A view is conceptually a relation, but the records in a view are not stored in the DBMS. Rather, they are computed using a definition for the view.

## Conceptual schema/Logical schema

Describes the stored data in terms of the data model. **This means all relations that are stored in the database.**

Here is an example of a conceptual schema:
```
Students(sid: string, name: string, login: string, age: integer, gpa: real)
```

## Physical schema
Specifies how the relations described in the conceptual schema are actually stored on secondary devices such as disks and tapes. **This includes indexes (auxiliary data structures) to speed up data retrieval operations**.

Here is an example of a physical schema:
```
Store all relations as unsorted files of records.

Create indexes on the first column of the Students, Faculty and Courses relations, the sal column of Faculty and the capacity column of Rooms.
```

The process of arriving at a good physical schema is called **physical database design**.

## External schema
These describe which data can be accessed and/or customized at the level of individual users or groups of users.

These are generally views.

**A database may have many external schemas.**

**Each external schema consists of a collection of one or more views and relations from the conceptual schema.**

The external schema design is guided by end user requirements.

An example of a view could be:
```
CourseInfo(rid: string, fname: string, enrollment: integer)
```

But obviously we could have many more. They simply describe the "public interface" of the database.

And, even though the records in the view are not stored explicitly, they are computed as needed.

## Data independence

### Logical data independence
If the end-users can be shielded from changes in the logical structure of the data or changes in the choice of relations to be stored, we have **logical data independence**.

### Physical data independence
The conceptual schema insulates users from changes in physical storage details. This is referred to as physical data independence. The conceptual schema hides details such as how the data is actually laid out on disk, the file structure and the choice of indexes.

 # The Relational model
 A (relational) database is a collection of one or more *relations*, where each relations is a table with rows and columns.

 Rows are also known as records or tuples.

 Columns are also known as attributes or fields.

 ### Relation schema vs relation instance

 - A relation instance is a table.
 - A relation schema describes the column heads for the table.

### Relation(al) schema example
```
Students(sid: string, name: string, login: string, age: integer, gpa: real)
```

## Creating and modifying relations using SQL (with DDL)
The subset of SQL that supports the creation, deletion and modification of tables is called the **Data Definition Language (DDL)**.

### `CREATE TABLE`
This statement is used to define a new table.
To create a `Students` relation, we can use the following statement:
```sql
CREATE TABLE Students (sid CHAR(20), name CHAR(30), login CHAR(20), age INTEGER, gpa REAL)
```

### `INSERT`
Tuples are inserted using the `INSERT` command. We can insert a single tuple into the `Students` table as follows:
```sql
INSERT INTO Students (sid, name, login, age, gpa) VALUES (53688, 'Smith', 'smith@ee', 18, 3.2);
```

### `DELETE`
We can delete tuples using the `DELETE` command.
We can delete all `Student`s tuples with `name` equal to *"Smith"* using the command:
```sql
DELETE FROM Students S WHERE S.name = 'Smith'
```

### `UPDATE`
We can modify the column values in an existing row using the `UPDATE` command:
```sql
UPDATE Students S
SET S.age = S.age + 1, S.gpa = S.gpa - 1 WHERE S.sid = 53688
```

## Integrity constraints over relations
An *Integrity constraint* is a condition specified on a database schema and **restricts the data that can be stored in an instance of the database**.

These can be placed while defining a database schema to specify the integrity constraints that must hold on any instance of the database.

And then, when the database application is run, the DBMS will check for violations and disallow changes to the data that violate the integrity constraints.

### Key constraints
A *key constraint* is a statement that a certain *minimal* subset of the fields of a relation is a unique identifier for a tuple.

For instance, that an `id` of a table is unique for all records in it. (So it can be used as primary key).

**A set of fields that uniquely identifies a tuple according to a key constraint is called a *candidate key* for the relation.**

This also means that, for fields that has *key constraints*, the DBMS will reject records with values such fields that are non-unique.

**EVERY RELATION IS GUARANTEED TO HAVE A KEY!**. The set of *all* fields is always a *superkey*.

### Primary key
Out of all available *candidate keys*, a database designer can identify a primary key.

This allows for a tuple to be referred to from elsewhere in the database by storing the value of its primary key fields (typically just one, an "id" for instance).

### Specifying key constraints in SQL

By using the `UNIQUE` constraint, we can declare that a subset of the columns of a table constitute a key.

**At most one of these candidate keys can be declared to be a *primary key*, using the `PRIMARY KEY` constraint**.

For instance:
```sql
CREATE TABLE Students (sid CHAR(20), name CHAR(30), login CHAR(20), age INTEGER, gpa REAL, UNIQUE (name, age), CONSTRAINT StudentsKey PRIMARY KEY (sid))
```

Here, we make `sid` the primary key.
The combination of `name` and `age` is also a key.

### Foreign key constraints
Sometimes the information stored in a relation is linked to the information stored in another relation.

If one is modified, the other must be checked and perhaps modified, to keep the data consistent.

A *foreign key* constraint makes this possible.

**The foreign key in the referencing relation must match the primary key of the referenced relation.**

So, for instance:
```
Enrolled (student_id: integer, foo: string, bar: string)

Student (id: integer, name: string)
```

Here, we could make the `id` of a `Student` primary key and then define it as foreign key in `Enrolled` under the column `student_id`.

**This is a constraint since the DBMS vill reject insertions if the foreign key of the referencing table doesn't match with any record with that primary key in the referenced table**.

### Specifying Foreign Key Constraints in SQL
```sql
CREATE TABLE Enrolled (studid CHAR(20), cid CHAR(20), grade CHAR(10), PRIMARY KEY (studid, cid), FOREIGN KEY (studid) REFERENCES Students)
```

Here we state that every `studid` value in `Enrolled` must also appear in `Students`. Notice that `studid` is also part of the tables primary key, combined with `cid`. This is perfectly fine.

These constraints allow us to ensure that one student only has exactly one grade per course he/she is part of.

### General constraints
We can also provide constraints on other things than keys.

For instance, that any student must be older than some age.

We can define *table constraints* and *assertions* to do just that.

## Handling foreign key violations
We need to consider three questions:

1. What should we do if a row is (attempted to be ) inserted with a foreign key to a record that doesn't exist?

	-	We should probably reject the insertion. This happens by default in most systems.

2. What should we do if a row is deleted that has a foreign key?

	- We need to consider composition aggregation. Can a Enrollment exist without a Student? If no, we should probably delete that one too.

	- We have three options:

		- Delete all records that refer to the deleted one as foreign key.

		- Simply disallow the deletion if another record refers to it.

		- Set the column value to some other existing record which will be referenced from now on.

3. What should we do if the primary key value of a referenced row is updated?

	-	Why would you do that? That's pure evil.

Anyway, to solve this in SQL, we can support these options on `DELETE` and `UPDATE`.

Let's specify that when a `Students` row is deleted, all `Enrolled` rows that refer to it are to be deleted as well, but that when the `sid` column of a `Students` row is *modified*, this update is to be **rejected** if an `Enrolled` row refers to the modified `Students` row:

```sql
CREATE TABLE Enrolled (studid CHAR(20),
											 cid CHAR(20),
											 grade CHAR(10),
											 PRIMARY KEY (studid, dd),
											 FOREIGN KEY (studid) REFERENCES Students
											 											ON DELETE CASCADE
																						ON UPDATE NO ACTION
)
```

`NO ACTION` means that the action (`DELETE` or `UPDATE`) is to be rejected.

**This is set by default, so it could be omitted**.

The `CASCADE` keyword says that, if a `Students` row is deleted, all `Enrolled` rows that refer to it are to be deleted as well.

**If the `UPDATE` clause specified `CASCADE`, and the `sid` column of a `Students` row is updated, this update is also carried out in each `Enrolled` row that refers to the updated `Students` row**.

We could also use `ON DELETE SET DEFAULT` to set the "default" student value if such a value exists.

To define such a value we could have used:
```sql
CREATE TABLE Enrolled (sid CHAR(20) DEFAULT '1234')
```

When we defined the schema for the table.

We could also simply set `null` with `ON DELETE SET NULL` if the field can be null.

### Deferring constraints
We can use the `DEFERRED` mode of a constraint to specify that we wait with checking constraints until we `commit` a transaction.

This can be useful if we make, for example, multiple insertions during a transaction, and one refers to some foreign key of another table which we haven't yet inserted a record for.

By waiting with checking for the constraints until we commit, we can solve this problem.

We specify it like this:
```sql
SET CONSTRAINT ConstraintFoo DEFERRED
```

### `DISTINCT` keyword
If you do a `SELECT DISTINCT something` from a table, and the table consists of two or more records that have the same combination of values, just one pair is returned.

For instance:
```sql
SELECT DISTINCT S.name, S.age
FROM Sailors S
```

Selects all sailors with the combination of the given name and age - but no duplicates (if two sailors had identical names and ages, they would otherwise have been returned).

### `AS` keyword
This one is an alias and can be omitted

For instance, if we did:
```sql
SELECT * FROM Sailors S
WHERE S.age > 12
```

...Then it would be identical to:
```sql
SELECT * FROM Sailors AS S
WHERE S.age > 12
```

### THE `*` selector
It is just short-hand for *"select all columns"*.

### `FROM`
The `FROM` clause is a list of table names. A table name can be followed by a *range variable* (such as `Students AS S`).

### `SELECT`
The *select-list* is a list of column names of tables named in the *from-list*, including any expressions (such as `COUNT()`).

### `WHERE`
The qualification in the `WHERE` clause is a boolean combination (`AND`, `OR` and `NOT`) of conditions of the form *expression op expressions* where *op* is one of the comparison operators (`<`, `<=`, `=`, `<>`, `>=`, `>`). An *expression* is a column name, a constant or an arithmetic or string expression.

### Cross-product
The *from-list* constitutes a cross-product.

So when you do:
```sql
SELECT * FROM Students, Enrollments, Whatever
```
You are taking the crossproduct of those three relations.

### Expressions and strings in the `SELECT` command

Again, you can use the `AS` keyword to alias columns:
```sql
SELECT S.name AS whatever
```

### The `LIKE` operator, the `%` and `_` wildcard symbols

This allows for pattern matching on strings.

`%` means *"zero or more arbitrary characters"*

`_` means **exactly one arbitrary character**.

So, for instance: `_AB%` means *some character, followed by AB, followed by zero or more characters*.

The `LIKE` operator is used to do such pattern matching:
```sql
SELECT name FROM Students
WHERE name LIKE '_o K%'
```

This would select all students that has a first-name with two letters, the second of which is *o*, and has a surname that starts with a (capitalized) *K*.
