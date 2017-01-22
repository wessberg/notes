# Normalization
> [RG] 19.1, 19.2, 19.4, 19.5, 19.6, 19.7, 19.9

## Redundancy
Redundancy is evil.

Something that often causes redundancy are functional dependencies.

## Functional dependencies
If one attribute (value) is determined by another, there is a *functional dependency*.

So, lets say we have two attributes, A and B, and there is a functional dependency between them.

**For a given A, there is only one permissible value for B.**

**So, if the same value appears in two columns of A, then the same value must appear twice in B as well; This is redundant**.

## Anomalies with redundancy and functional requirements

Some issues with functional requirements and redundant information:

- **Update Anomalies**: We could update B without making a similar change in A.

- **Insertion Anomalies**: We cannot insert a tuple if we don't know A, even if we might know B.

- **Deletion Anomalies**: If we delete all tuples with a given value of A, we lose association between that and B.

**Generally, redundancy arises when a relational schema forces an association between attributes that is not natural**.

We can use functional dependencies to identify such situations and suggest refinements to the schema.

Examples of functional dependencies are:
- CPR -> name
- CPR -> address

But, the reverse is not (generally) true:
- name -> CPR is false,
- name -> address is also false

**We can address such problems by replacing a relation with a collection of smaller relations**.

This is called decompositions:

## Decompositions
As stated, a decomposition of a relation schema *R* consists of replacing the relation schema by two relation schemas that each contain a subset of the attributes of *R* and together include all attributes in *R*.

For example:
<img src="assets/decomposition_1.png" />

Here, there is a functional dependency between `rating` and `hourly_wages`. Notice how for each value of `rating`, the `hourly_wages` value is the same.

We can then do decomposition and arrive at this:

<img src="assets/decomposition_2.png" />

## Normal forms

We can use normal forms as a characterization of a schema decomposition in terms of the properties it satisfies.

If a relation has one of the *normal forms*, we know that certain kinds of problems **cannot** arise.

We can use the normal form to decide if we should decompose a relational schema further.

There are some variations:
- First normal form (1NF)
- Second normal form (2NF)
- Third normal form (3NF)
- Boyce-Codd normal form (BCNF).

**These have increasingly restrictive requirements**:
Each relation in BCNF is also in 3NF, every relation in 3NF is also in 2NF, and every relation in 2NF is also in 1NF.

### First normal form (1NF)
**A relation is in *first normal form* is every field contains only atomic values**.

Precisely:

1. No duplicate columns in a single table.

2. Each column has single value (i.e. there is no group of values or array).

3. Each row should be identified by unique key (A Primary Key).

So, if you had stuff like this:
<table>
	<tr>
		<td><strong>Name</strong></td>
		<td><strong>Email</strong></td>
	</tr>
	<tr>
		<td>John</td>
		<td>john@gmail.com, john@hotmail.com</td>
	</tr>
</table>

That would not be in 1NF, since the "email" field holds several values.

When we say "duplicate" columns, we mean columns that refer to the same data.
For example, if we had:

<table>
	<tr>
		<td><strong>Name</strong></td>
		<td><strong>Class A</strong></td>
		<td><strong>Class B</strong></td>
		<td><strong>Class C</strong></td>
	</tr>
	<tr>
		<td>John</td>
		<td>MODIS</td>
		<td>BDSA</td>
		<td>INDAD</td>
	</tr>
</table>

We would have duplicate columns. So instead, we can break it up:

<table>
	<tr>
		<td><strong>Name</strong></td>
		<td><strong>Class</strong></td>
	</tr>
	<tr>
		<td>John</td>
		<td>MODIS</td>
	</tr>
	<tr>
		<td>John</td>
		<td>BDSA</td>
	</tr>
	<tr>
		<td>John</td>
		<td>INDAD</td>
	</tr>
</table>

Which respects the two first properties of 1NF, but now we have rows that cannot be uniquely identified by a primary key!
So we need to also compute a primary key on the *name* and *class* combination (or introduce an "id" field that is a primary key).

*Now* we're golden.

Another example of stuff that is **not** in 1NF:
<img src="assets/1nf.png" />

### Second normal form
To be in second form, the following requirements must be met:

1. Table should be in First Normal Form.

2. There should be no partial dependency between non-key attributes and composite key. A partial dependency occurs when a non-key attribute is dependent on only a part of the (composite) key.

3. There should be no subset of data that comes with multiple rows in a table. Place that data into different table.

4. Define relationships among the new tables and the base table using foreign key.

So, if we had this:
<table>
	<tr>
		<td><strong>Name</strong></td>
		<td><strong>Class</strong></td>
		<td><strong>Teacher</strong></td>
	</tr>
	<tr>
		<td>John</td>
		<td>MODIS</td>
		<td>Alice</td>
	</tr>
	<tr>
		<td>John</td>
		<td>BDSA</td>
		<td>Alice</td>
	</tr>
	<tr>
		<td>John</td>
		<td>INDAD</td>
		<td>Bob</td>
	</tr>
</table>

We would violate the third property of 2NF since Alice will occur in multiple rows (and so will the student, John)
Instead, we would be better off by having a "Student" and "Teacher" table, each with a primary key on a "studentId" and "teacherId" column and refer to the foreign keys instead.

But then also notice the second property. We have a primary key on *name* and *class*, but *Teacher* is functionally dependent on *class*, wouldn't you say? Wouldn't we get the same teacher whenever we have a class? Here, *Class -> Teacher*, but **not** *name -> Teacher*, right?

So, in other words, teacher is partially dependent on the composite primary key. Partially because it is on dependent on half of it, the class. So we also need to break *Teacher* out to have 2NF. So by doing that and referring to the primary key of a new Teacher table as the foreign key of the *teacherId* column in the table, we're golden.

### Third Normal Form
Third normal form must meet the following criteria:

1. Table should be in Second Normal Form.

2. No non-key column should depend on other non-key column or has no transitive functional dependency.

So, imagine this:
<table>
	<tr>
		<td><strong>Class</strong></td>
		<td><strong>Teacher</strong></td>
	</tr>
	<tr>
		<td>MODIS<</td>
		<td>Alice</td>
	</tr>
	<tr>
		<td>BDSA<</td>
		<td>Bob</td>
	</tr>
	<tr>
		<td>INDAD<</td>
		<td>Sara</td>
	</tr>
</table>

Let's assume that the same class is always associated with the same teacher.
Then we have a functional dependency: *Class -> Teacher*. Iff *class* is a non-key attribute (e.g. it isn't the Primary Key),
we are **not** in 3NF since *Class* implies *Teacher*, even though *Class* is not a key.

So, if we make sure that *Class* is the primary key, we're golden!

### Boyce-Codd normal form (BCNF)
Here's the criteria:

1. Table should be in Third Normal Form.
2. Every determinant must be a candidate key.

A determinant is a column used to identify other column values in the same row.
So, if we had this:
<table>
	<tr>
		<td><strong>Class</strong></td>
		<td><strong>TeacherId</strong></td>
		<td><strong>StudentId</strong></td>
	</tr>
	<tr>
		<td>MODIS<</td>
		<td>0</td>
		<td>3</td>
	</tr>
	<tr>
		<td>BDSA<</td>
		<td>1</td>
		<td>4</td>
	</tr>
	<tr>
		<td>INDAD<</td>
		<td>2</td>
		<td>6</td>
	</tr>
</table>

And the Primary Key was on *TeacherId*, then we would have a problem with *StudentId*. It is a determinant as well.
But - it isn't a candidate key, - it can occur in multiple rows and hence doesn't uniquely identify any single record.
So, we are not in BCNF.

Other than that, it guarantees no redundancies and no lossless-joins - but not dependency preservation.

### Fourth Normal Form
Here's the criteria:

1. Table should be in Third Normal Form.

2. It should not have any Multi-Valued dependencies.

**When the presence of one or more rows in a table implies the presence of one or more other rows in a table, we have multi-valued dependencies**.


## Properties of decompositions
As stated, before we do decomposition, we should ensure that it doesn't introduce new problems.

TL;DR; We should check whether a decomposition allows us to recover the original relation, and whether it allows us to check integrity constraints efficiently.

## Lossless-joins

The *lossless-join* enables us to recover any instance of the decomposed relation from corresponding instances of smaller relations.

We should be able to reconstruct the original relation by joining the parts together.

## Dependency-preservation
The *dependency-preservation* property enables us to enforce any constraint on the original relation by simply enforcing the same constraints on each of the smaller relations.

So, we do not need to perform joins on the smaller relations to check whether a constraint on the original relation is violated.

The slides propose another, simpler explanation:
**Ensure that the original functional dependencies cannot span multiple tables**.

## Performance penalties of decomposition

Yes, they to be felt. If we want to do queries over the original relation, we have to join the decomposed relations.

So don't do that if such queries are common.

## Armstrong's Axioms
These are:
- Reflexivity: *if X ⊇ Y* then *X → Y*:

	-	If *X* is a superset of *Y*, then there is a functional dependency between X and Y.

	- For example, if *X* is the combination of columns *cpr* and *courseId* and *Y* is *cpr*, then there is a functional dependency between X and Y.

- Augmentation: *if X → Y* then *XZ → YZ* for all *Z*.

	-	If *X* implies *Y*, then *XZ* also implies *YZ* for all values of *Z*.

	- For example, if *cpr → name* then *cpr grade → name grade*.

- Transitivity: *if X → Y* and *Y → Z* then *X → Z*.

	- If *X* implies *Y* and *Y* implies *Z*, then *X* implies *Z*.

	- For example, if *cpr → address* and *address → country* then *cpr → country*.

## Other rules:

- Union: If *X → Y* and *X → Z* then *X → YZ*.

- Decomposition (the other way around): If *X → YZ* then *X → Y* and *X → Z*.

- Pseudo-transitivity: If *X → Y* and *YW → Z* then *XW → Z*.

	- Hard to wrap your head around at first, but if *X* implies *Y*, and *Y* in combination with some other attribute *W* implies *Z*, then *X* combined with the same attribute *W* will also imply *Z*.

## Closures
Given a set of functional dependencies *F*, the closure *F<sup>+</sup>* is defined as the set of all **implied functional dependencies**.

For example,
<img src="assets/functional_dependencies_table.png" />

Here we can see, that for each *cpr*, there is a provided functional dependency between *cpr* and any combination of *name* and *address*. So, this is a **provided** functional dependency.

Also, we can see that a functional dependency between the combination of *cpr* and *courseId* and each *grade*. Again, this is a **provided** functional dependency.

But to determine the closure, we need to focus on the **implied** functional dependencies, which we can use the rules listed above to determine.

So, by Reflexivity we can tell that all combinations of *cpr* and *name* implies *cpr*, since *cpr* and *name* is a superset of *cpr*.

And, with decomposition, since we can already tell that *cpr* implies the combination of *name* and *address*, we can also use decomposition to define the implied functional dependencies that *cpr* implies *name* and *cpr* implies *address*.

And, by augmentation, since we already determined that *cpr* implies *name*, we can also say that *cpr* and *grade* implies *name* and *grade*.

And so it goes.

### Attribute closure
For a given attribute *X*, the attribute closure *X<sup>+</sup>* is the set of all attributes such that *X -> A* can be inferred using Armstrong's axioms.

### Canonical cover
The Canonical cover is the "lightweight" version.

It states that the canonical cover is defined as **the minimal set of functional dependencies**.

Do this:

1. Input a set of functional dependencies.

2. For each functional dependency in the set, drop extraneous attributes and redundant functional dependencies **until the remaining functional dependencies have a single attribute on the left.**

## Main insight
It appeared on at least 3 slides:
**Split relations along functional dependencies.**
