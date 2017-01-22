# Indexing (in SQL)
> Literature: RG 8.1,8.2,8.3,8.5,10.1-10.6

### Clustered index vs Unclustered Index
A Clustered index is sorted based on the key value of whatever column the index is set on. So if it was "age", the index would be sorted and stored in sorted order with the age value.

On the contrary, unclustered indexes have a structure separate from the data it refers to. Instead it holds pointers to the actual data rows that holds the actual values.

**There can only be one clustered index per table, typically the primary key**.

# Book notes

### File Organization
A file organization is a method of arranging the records in a file when the file is stored on disk.
Such an organization makes certain operations efficient but other operations expensive.

### Indexing
Indexing can help when we have to access a collection of records in multiple ways, in addition to efficiently supporting various kinds of selection.

### Data on external storage
A database management system stores vast quantities of data, and data must persist across program executions. Thus, data is stored on external storage devices such as disks and fetched into main memory as needed for processing.

**The unit of information read from or written to disk is a page**.

The cost of page I/O (the input from disk to main memory and output from memory to disk) dominates the cost of typical database operations, and database systems are carefully optimized to minimize this cost.

So, if we read *several* pages in the order that they are stored in physically, we have to add that read-latency to *every* page we page through.

#### Buffer manager
The buffer manager is responsible for reading data into memory for processing as well as writing stuff to disk for persistent storage.

### Disk space manager
This one can grant more disk space to the DBMS if requested. This one keeps track of the pages in use by the file layer.

### File organizations and Indexing
#### Scans
A *scan* operation allows us to step through all the records in a file one at a time.

The *file layer* stores the records in a file in a collection of disk pages.

It keeps track of pages allocated to each file, and as records are inserted into and deleted from the file, it also tracks available space within pages allocated to the file.

#### Heap files
The simplest file structure is an **unordered file**, or *heap file*.

Records in such a file are stored in random order across the pages of the file.

A heap file organization supports retrieval of all records, or retrieval of a particular record specified by its record ID.

## Indexes
An *index* is a data structure that organizes data records on disk to optimize certain kinds of retrieval operations.

An index allows us to efficiently retrieve all records that satisfy search conditions on the search key fields of the index.

We could store all records of employees in a file organized as an index on employee age, for example. We could then create an auxiliary index file based on salary, to speed up queries involving salary.

### Clustered Indexes
When a file is organized so that the ordering of data records is the same as or close to the ordering of data entries in some index, we say that the index is *clustered*. Otherwise, it is an *unclustered* index.

### Primary and Secondary Indexes
An index on a set of fields that includes the *primary key* is called a primary index.

All other indexes are called *secondary indexes*.

A primary index is guaranteed not to contain duplicates (obviously, since primary keys are always unique), while secondary indexes *can* contain duplicates.

That doesn't mean that 2 indexes are the same, it means that the search key field *associated* with the index are the same.

### Index data structures
There are typically hash-based and tree-based indexing techniques.

### Hash-Based Indexing

With hash-based indexing, we can quickly find records that have a given search key value. So, if a file of, say, "Employee" records is hashed on a column named *name*, we can retrieve all records about, say, "Joe" just by searching for it - very fast.

In this approach, the records in a file are grouped in *buckets*.

#### Buckets
A bucket consists of a *primary page* and possibly additional pages linked in a chain.

The bucket to which a specific record belongs can be determined by applying a hash function to the search key. We then immediately know about that bucket and the primary page for it.

All in all, this happened in about one or two disk I/O's, so that's pretty ok!

#### Insertions
When we insert stuff with Hash-Based Indexing, the record is inserted into the appropriate bucket.

**Hash-Based Indexing are bad for range selections, but great for equality selections!**.

### Tree-Based Indexing
Here we arrange data entries in a tree-like data structure **in sorted order by search key value**.

We can then reap the same awesome benefits from BSTs as we know. If we index on *age*, for example, we could then check if the search value is less than or greater than the value of the current node, and go either left or right depending on the comparison - until we find the record(s) with that age value.

We obviously want to go for a BBST if possible. The book is very satisfied with B+ trees.

**Tree-Based Indexing are great for range selections AND equality selections!**.

### Indexes and performance tuning
In general, an index supports efficient retrieval of data entries that satisfy a given selection condition.

#### Index-Only evaluations
These are evaluations that doesn't need to look "into" the data, but are perfectly satisfied with just looking at the indexes.

For instance, if we have an index on *age* and we want to compute the average age of employees, the DBMS can do this by simply examining the data entries in the index. This is known as an *index-only evaluation*.

### Composite Search Keys
The search key for an index can contain several fields. Such keys are called *composite search keys* or *concatenated keys*.

## Tree indexing
B+ trees are the most widely used index structure because it adjusts well to changes and supports both equality and range queries.








# Lecture notes

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

### Other impacts of indexes
The DBMS may use indexes in other situations than a simple point or range query.
- Some joins can be executed using a modest number of index lookups.
	- May be faster than looking at all data.
- Some queries may be executed by only looking at the information in the index.
	-	**index only** query execution plan ("covering index").
	- May need to read much less data.

### Index types
<u>Common:</u>
- B-trees (point queries, range queries).
- Hash tables (only point queries, but somewhat faster).
- Bitmap indexes (good for "dense" sets.)

<u>More exotic:</u>
- Full text indexes (substring searches).
- Spatial indexes (proximity search, 2D range search, ...)
- ...and thousands more.

This of course varies between various relational database management systems.

## Conclusion
- Large databases **need** to be equipped with suitable indexes. To be an expert in database design, you:
	-	Need to understand how indexes might help with a given set of queries.
	- Need to understand distinction between primary and secondary indexes.
	- Need to have a detailed understanding of index types.
