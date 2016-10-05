# Indexing (in SQL)
> Literature: RG 8.1,8.2,8.3,8.5,10.1-10.6

### Nice-to-know
Sequential memory access is faster than random accesses in memory! Another reason to keep your data sorted.

### Disk

- Relations of large databases are usually stored on hard drives (or SSDs).
- Hard drives can store large amounts of data, but work rather slowly compared to the memory of a modern computer:
	-	The time to access a specific piece of data is on the order of 10<sup>6</sup> (10<sup>4</sup>) times slower.
	- The **rate** at which data can be read is on the order of 100 (10) times slower.
- Time for accessing disk may be the main performance bottleneck.

### Parallel data accessing

- Large database systems (and modern SSDs) use several storage units to:
	-	Enable several pieces of data to be fetched in parallel.
	- Increase the total rate of data from disk.
- Systems of several disks are often arranged in so-called RAID systems with various levels of error resilience.
- Time used for accessing data is often the performance bottleneck.

### Full table scans

A scan that runs through all records in a table. e.g.
```sql
SELECT *
FROM R
WHERE <condition>
```

### Selective queries
The opposite of full table scans. Where we want to be very selective and it would be preferable not to run through all records.

### Point queries
A selection query with a single equality in the condition. e.g.
```sql
SELECT *
FROM Person
WHERE birthyear=175
```

### Range queries
Where we look for a *range* of values of an attribute.
E.g.:
```sql
SELECT *
FROM Person
WHERE year(birthdate) BETWEEN 1775 AND 1994
```

## Indexes
To speed up queries, the DBMS may build an **index** on a specific attribute.
- A database index is similar to an index on the back of a book:
	- For every piece of data you might be interested in, the index says where to find it.
	- The index itself is organized such that one can quickly do the lookup.
- Looking for info in a relation with the help of an index is called an **index scan**.
- It then becomes as easy as searching the three of indexes.

### Drawbacks
- Whenever something updates in the database, the indexes must be updated as well. Otherwise, updates will be slow. This adds additional overhead to update/insert operations.
	-	Small to medium for primary index.
	- High for secondary index (but improving)
- Indexing takes up additional space.
	- Small for primary index
	- Similar to data size for secondary index (that's a lot!)

### Create an index
Here's the syntax:
```sql
CREATE INDEX <indexname> ON <tablename>(<attribute>)
```

e.g.:
```sql
CREATE INDEX idx1 ON Movie(year)
```

It makes the DBMS generate indexes on the chosen attribute which significantly speeds up queries.

### Create index on multiple attributes
For instance,
```sql
CREATE INDEX idx1 ON Inv(movieId, personId, role)
```

This will create an index for all combinations of those three attributes. Its basically an easy fix for massive performance improvements.

Speeds up point queries such as
```sql
SELECT *
FROM Person
WHERE name="John Doe" AND birthdate < "1950-1-1"
```

Usually gives index for any **prefix** of these attributes, due to lexicographic sorting.

### Primary Indexes

- If the tuple of a relation are stored sorted according to some attribute, an index on this attribute is called **primary**.
	-	Primary indexes make point and range queries on the key **very** different.
- Many DBMSs automatically build an index on the primary key of each relation.
	-	In MySQL this depends on the storage engine. InnoDB builds an index on the primary key, MyISAM does not.
- A primary index is sometimes referred to as a **clustering** or **sparse** index.

### Secondary Indexes
- Multiple indexes can be created on a relation (with the same syntax as seen above).
- The non-primary indexes are called **secondary** indexes (sometimes **non-clustering** or **dense** indexes).
	- Secondary indexes make *most* point queries on the key more efficient.
	- Secondary indexes make *some* range queries on the key more efficient.

### Index scan vs Full table scan

Point- and range queries on the attribute(s) of the **primary index** are almost always best performed using an index scan.

**Secondary indexes** should be used with **high selectivity queries**: As a rule of thumb, a secondary index scan is faster than a full table scan for queries returning less than 10% of a relation.

### Choosing to use an index
- The choice of whether to use an index is made by the DBMS *for every instance of a query*.
	- May depend on query parameters.
	-	Doesn't have to take indexes into account when writing queries.
- Estimating selectivity is done using statistics.
	-	In MySQL, statistics is gathered by executing statements such as `ANALYZE TABLE <tablename>`
