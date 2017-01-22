# Query processing and tuning
> Literature: 12.1,12.2,12.4,12.5,12.6, RG 20

### Query optimization
The process of finding a good evaluation plan is called *query optimization*.

The basic task is to consider several alternative evaluation plans for a query.

We recognize that a query is essentially treated as a *σ - π - ⋈* algebra expression, with the remaining operators.

Typically, an optimizer considers a subset of all possible plans because the number of possible plans is very large.

### Catalog
A relational RDBMS maintains information about every table and index that it contains in a collection of special tables called *catalog tables*.

These are simply referred to as the *catalog*.

#### Information in the Catalog

For each table:
- Its *table name*
- The *file name*
- The *file structure* of the file in which it is stored
- The *attribute name* and *type* of each of its attributes
- The *index name* of each index on the table
- The *integrity constraints* on the table (e.g. primary key and foreign key constraints)

For each index:
- The *index name* and the *structure* (e.g. B+ tree) of the index.
- The *search key* attributes.

For each view:
- The *view name* and definition.

Also, statistics are stored and updated periodically. The following information is stored:

- Cardinality: The number of tuples for each table.
- Size: The number of pages for each table.
- Index Cardinality: The number of distinct key values for each index.
- Index Size: The number of pages for each index.
- Index height: The number of nonleaf levels for each tree index.
- Index range: The minimum present key value and the maximum present key value for each index.

### Query Evaluation Plans
A query evaluation plan consists of an extended relational algebra tree, with additional annotations at each node indicating the access methods to use for each table and the implementation method to use for each relational operator.

## Selection

### Access Path
The access path has to do with the files that must be paged through while searching. For example, a sequential *file scan* to retrieve tuples, and/or an index scan.

### Selectivity
The selectivity of an access path refers to the percentage of pages retrieved.

### Covering indexes
A *covering index* is one where all relevant fields of the query appear in the index.

Given
```sql
SELECT F.arr
FROM Flights F
WHERE F.actype = "321"
```
A *covering index* would be:
```sql
CREATE INDEX MyIndex ON FLIGHTS (actype, arr)
```

### Simple selections
If we need to do 100 reads for a "full table scan" and we have a selectivity of 50%, then we need approximately 50 reads to determine the result.

The approximation can be generalized:
*(size of relation) x selectivity*

### Selection options
We could either:

1. **Have no index, and the data is unsorted**

To do:
```sql
SELECT *
FROM Flights
WHERE arr < "F%"
```

Would require a "Full table scan". And thus, we'd have a selectivity of 100% (all pages are returned).

2. **Have no index, and the data is sorted.**

To do:
```sql
SELECT *
FROM Flights
WHERE arr < "F%"
```

Would cost us a binary search operation and the number of pages that would end with resulting in.

3. **Use a B+ Tree index**

If the index was on *"arr"*, we would find the qualifying data entry and then retrieve the corresponding data record(s).

The efficiency of this comes down to whether more than 10% of the records satisfy this condition or not.

If we had 1000 records and only 10% of them satisfy the `WHERE` condition, then only 100 pages would be retrieved with a clustered index while *more* than 1000 pages would be retrieved with an unclustered index.

So, if we want our secondary indexes (which are unclustered by design) to be efficient in such scenarios, focus on sorting the data instead and do binary search (as seen in (2)). I think this is the take-away.

### Approaches to Selection

#### Approach 1

Find the cheapest access path, retrieve tuples using it, and then apply any remaining terms that don't match the index:

- Cheapest access path: an index or file scan with fewest I/Os.

- Terms that match this index reduce the number of tuples retrieved

- Other terms help discard some retrieved tuples, but do not affect number of tuples/pages fetched.

#### Approach 2
Using two or more indexes.

1. Get *rids* from each index.

2. Intersect all these *rids* sets.

3. Retrieve the records

# Algorithms for implementing Joins

## Nested Loop Joins
This one takes an expression such as
```sql
SELECT * FROM Flights F, Aircraft A WHERE F.actype = A.actype
```

and computes the join through a nested foreach loop:
```
foreach tuple f of F
	foreach tuple a of A
		output if they match
```

It would be nice, though, if the outer relation (the outer foreach loop) could be smaller. Also, we'd like to read blocks instead of tuples.

## Block Nested Loop Joins
One buffer for each relation as well as one for the output relation:

```
Read block from F
	Read block from A
		output if tuples match
```

We can clearly see that, as we know, we have at least *O(N<sup>2</sup>)* here.

But maybe if we had an index on the inner relation, we might do better!

## Sort-Merge Joins
Can be used if both tables are sorted on the joined key.
Then we can step through both in parallel - nice.

### Important take-away
SO, to summarize: Pick the smallest relation as outer relation, pack as much into memory as you can and do as much in parallel as possible.

## `EXPLAIN` keyword for `SELECT`s

This fellow displays information from the optimizer about the statement execution plan. It "explains" how it will process the query, including how tables are joined and in which order.

## The price of normalization (and how to denormalize)

Normalization can improve performance sometimes:

- Less redundancy leads to more rows per page which ultimately leads to less I/O.

- Decomposition leads to more tables which leads to more clustered indexes (remember, there can only be one per table) which leads to smaller indexes!

But, normalization can also be costly:

- We often need to do more Joins

- There are fewer indexing possibilities.
