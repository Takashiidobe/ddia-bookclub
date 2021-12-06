# Chapter 3 Questions

## Hash indexes are the first index we learn of in this chapter. What are its strengths? What are its limitations? If you've ever used a hash index, what were some of its limitations in practice?

A Hash index is efficient in indexing data that generally fits on disk,
as it allows for O(1) operations. However, implementations like bitcask
require that all keys fit in memory, making it difficult for large
datasets.

As well, range queries (<, >, <=, >=) won't use a hash index, which
makes reads over a segment of the data hard to scale with a hash index
only.

## Read [Re-introducing Hash Indexes in PostgreSQL](https://hakibenita.com/postgresql-hash-index) for a deeper dive into hash indexes in Postgres. What are some of the differences between this writeup and DDIA? What struck you as some strengths of hash indexes when you read about it?

1. Hash Indexes are smaller in memory than B-Tree Indexes
2. Hash Index side is not affected by the size of the indexed item.
3. Hash Indexes aren't affected by selectivity.

There are some cons though:

1. B-Tree Index writes can have larger variance, since a new bucket may
   need to be allocated (like how arrays can have O(n) write times when
   reallocating).
2. They can't be used to enforce unique constraints
3. They can't be used to create indexes on multiple columns
4. They can't be used to create sorted indexes.
5. They can't be used to cluster a table.

Postgres also seems to be able to persist the hash index to disk, so it
doesn't seem like there's a restriction to forcing the hash index in
memory, although given that a 2TB RAM server is about 2k USD a month, I
don't think its an issue for most servers (assuming a hash key is 64
bits, 2 TB of RAM can hold about 250B items, and virtualization can
probably allow for more RAM usage if needed).

## What are the strengths and weaknesses of SSTables? For more of a in-depth view, read this answer on [SO](https://stackoverflow.com/questions/8651346/sorted-string-table-sstable-or-b-tree-for-a-database-index/8654903#8654903).

SSTables allow for writing to an in-memory segment that is compacted on
disk, so writes should be very fast (O(log n) in memory). Reads are
slower, though, since they may have to go on disk, which is similar to
what a B-Tree index might need to do. Compaction can also affect the
performance of reads and writes to tables.

## B-trees are the most common index in databases. What makes them so popular? What are some of their tradeoffs when compared to hash indexes and SSTables? What are some other applications of B-trees that you've come across?

B-Trees have had extreme popularity in the literature, being introduced
in 1970 and being used in many places (Operating Systems, Databases,
Networking, etc) soon after. They are popular because they are
conceptually simple (a tree with arrays as Nodes) which provides good
performance for interacting with disk based media (this scales well with
the assumption that reaching to disk is magnitudes slower, so B-Trees
are optimized for reading as few pages as possible from the OS).

Vs. Hash Tables, B-Trees are larger on disk and more complicated, but
allow for range queries and perform similarly for read and write
throughput, but can also be persisted entirely on disk, which is a
luxury not afforded to Hash indexes (in 1995, the average workstation
had about 32MB of RAM but much more disk (4-8GBs)), making them much
more viable than hash indexes historically.

Vs. SSTables, they have less benefits, outside of being historically
more optimized, but that's changing with this crop of NoSQL databases.
Vs. MemTables and SSTables, Copy on write semantics are hard to code up
in a performant manner, so SSTables can achieve higher throughput, at
the cost of higher variance (with compaction in the background).

For applications of B-Trees, just about anything -- developers have
optimized C++'s std::map (an RB-Tree) by replacing it with a B-Tree for
faster Read and Write Throughput, on File Systems for querying data
without having to go through too many pages, and any kind of sorted
data, as it provides O(log n) operations on disk that is hard to beat
otherwise.

## Some other indexes are Partial (indexing only some values) and multi-column indexes. What are some of the strengths of those indexes? What are their weaknesses? Read [this](https://hakibenita.com/postgresql-unused-index-size) for a story where an index was compacted to save 20GB.

- For Geospatial data, R-Trees are commonly used for indexing (Quadtrees)
- SSTables for term dictionaries (a Posting list).
- Partial indexes are used to save space when values might be null or
  not queried on frequently.

## What are some of the pain points of working with indexes in your experience? Are they a silver bullet? Can you recall a time when using an index was unintuitive?

For pain points on using indexes, at _really_ large scale, O (log n)
operations (B-Tree indexes w/ range queries) can have large variance
based on the data queried and how well the index has been constructed.
In general, since indexes are maintained with writes to the system,
B-Trees may be unoptimized for sequential reads (since they could have
pointers out to random places on disk), leading toward similar range
queries having a wide window of performance issues. To fix this issue,
it might be required to reindex a table in the background (Postgres
allows this, but MySQL doesn't, thanks Oracle), or vacuum dead tuples,
which can affect read performance due to memory pressure.

In MySQL, one problem I had to chase was that queries can only use one
index per table (thanks Sun and Oracle) so when a service became unhealthy,
I migrated an index to that table, and was ready to call it a day except
that the query didn't end up using the new index, since it defaulted to
the old index.

Another issue I had was that it was tricky to realize when MySQL would
use a certain index, since it does not provide `EXPLAIN ANALYZE` like
Postgres, but just generates a query plan (that can be fickle) to query
the underlying data. You can force index in rails (USE INDEX in mysql)
but InnoDB can also ignore that too. Did you get that I'm not the
biggest fan of MySQL?

## Choose an index that wasn't covered by this chapter and briefly explain its characteristics. What is it used for, and what are similar indexes that can be used for the same use case?

I chose [BRINs](https://www.postgresql.org/docs/current/brin-intro.html)
, aka Block Range indexes. Block Range Indexes are used for read-heavy
large tables by sorting them. They work on data that have high
correlation and are sortable. They're very small indexes, so they have
low overhead for scanning. They require incremental index maintenance to
maintain performance, which has a bit overhead compared to B-Trees.

## Read [Introduction to PostGIS](http://postgis.net/workshops/postgis-intro/indexing.html) for a deeper dive into spatial indexing. Kleppmann notes that Multi-column indexes don't work for spatial data. How does PostGIS index spatial data?

PostGIS handles spatial data by creating a tree that indexes the
bounding boxes of the data. Then, a query is issued for the exact
intersection of the data. R-Trees create boxes over this spatial data
and then calculate intersections by checking for intersections inside
these subdivided boxes over the dataset.

## Column-Oriented Storage flips the mindset of relational databases by storing columns as vectors instead of rows as vectors. What are the pros and cons of such a database? What access patterns benefit the most from this change?

Column Oriented Storage is much like Data Oriented Programming. [Mike
Acton's CppCon 2014 talk](https://www.youtube.com/watch?v=rX0ItVEVjHc).

Let's say we have this data, organized into rows (as would be normal in
a Row based database):

```json
[
  { "key": 1, "value": 10, "address": "something" },
  { "key": 2, "value": 20, "address": "something else" },
  { "key": 3, "value": 30, "address": "what" },
  { "key": 4, "value": 40, "address": "else" }
]
```

Querying for all the `values` require all the rows to be read into
memory, which is a waste, because we only really want 8 bytes of memory
per row, but we have to read 24 per row.

That being said, reading a row (`SELECT *`) has no waste.

What would happen if we rearranged the data like this?

```json
const keys = [1, 2, 3, 4];
const values = [10, 20, 30, 40];
const addresses = ["something", "something else", "what", "else"];
```

If we say we want all addresses, we can query for "all addresses" by
looking only inside the addresses array. As well, if we want to do
something like:

```sql
SELECT key, addresses from table;
```

This still works, since it queries both the values and the addresses. If
your data pattern mainly involves querying along one key for lots of
data, Column Oriented storage is much much faster.

In exchange, though, row-based querying is slightly slower, since arrays
are close to each other in memory, while a certain entry will not be, so
that's the tradeoff.

As for what access patterns benefit the most from this tradeoff, I'd
think any application where querying by one key is more common.

## What is a Materialized View? What are some queries that it would speed up? What are the costs of materialized views compared to normal views?

A Materialized view is a view that is cached (persisted to disk). This
is useful for read heavy workloads over many tables, since this can go
around the costs of joining through many tables that you might have to
do with a regular view. That being said, a materialized view takes up
space on disk, and is denormalized, so it must be updated when any of
its relations are updated. A view is a query that is computed every time
it is queried, much like saving a query in a variable. A materialized
view query hits the cache, but can be stale, if care isn't taken to
update the view often.
