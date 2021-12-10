# Chapter 3 Questions

## Hash indexes are the first index we learn of in this chapter. What are its strengths? What are its limitations? If you've ever used a hash index, what were some of its limitations in practice?

Strengths: 
- Simple to reason about
- Very fast reads and writes
- Really good if you have a limited number of keys with frequent updates. 

Limitations: 
- Hashmap keys must be kept in memory, so space is limited. 
- Database restarts are painful since you need to recover the in memory hashmap.
- Range queries are not efficient


## Read [Re-introducing Hash Indexes in PostgreSQL](https://hakibenita.com/postgresql-hash-index) for a deeper dive into hash indexes in Postgres. What are some of the differences between this writeup and DDIA? What struck you as some strengths of hash indexes when you read about it?

The main advantages that stuck out to me were:
- hash indexes require less space than B-trees
- hash indexes perform better than B-trees.

For disadvantages:
- hash indexes can not enforce unique constraints.
- hash indexes are not effective for range queries.


## What are the strengths and weaknesses of SSTables? For more of a in-depth view, read this answer on [SO](https://stackoverflow.com/questions/8651346/sorted-string-table-sstable-or-b-tree-for-a-database-index/8654903#8654903).

Strengths:
- Don’t need to keep all keys in memory.
- Efficient reads, writes, and range queries. (although not as efficient as hash indexes)

Weaknesses:
- If db crashes, most recent writes in the memtables are lost. This forces you to maintain a separate log on disk for recovery.
- Overhead for compaction. 


## B-trees are the most common index in databases. What makes them so popular? What are some of their tradeoffs when compared to hash indexes and SSTables? What are some other applications of B-trees that you've come across?

B-trees have been around since the 1970s and have provided consistently good performance, so most people are happy to continue using them.
Unlike hash tables, B-trees support range queries and are persisted on disk. They also have comparable performance for reads and writes.
Compared to SSTables, B-trees often have faster reads but slower writes. Due to the page oriented model of B-trees, they cannot be compressed as well as LSM-trees, and may leave some disk space unused.


## Some other indexes are Partial (indexing only some values) and multi-column indexes. What are some of the strengths of those indexes? What are their weaknesses? Read [this](https://hakibenita.com/postgresql-unused-index-size) for a story where an index was compacted to save 20GB.

Partial indexes only index a subset of records. This allows you to have lower storage requirements and reduced work for index creation.

As the name implies, multi-column indexes allow you to index on multiple columns. This can improve query performance for queries on multiple columns. One weakness is that the index entries can be very large. 


## What are some of the pain points of working with indexes in your experience? Are they a silver bullet? Can you recall a time when using an index was unintuitive?
When I first learned about multi-column indexing, the fact that the order of the indexes matter was unintuitive to me. 

## Choose an index that wasn't covered by this chapter and briefly explain its characteristics. What is it used for, and what are similar indexes that can be used for the same use case?

BRIN - Block Range INdex. Used for large tables with columns that have naturally correlation with their physical location in the table. The index stores summary information about a group of pages that are adjacent to each other. 


## Read [Introduction to PostGIS](http://postgis.net/workshops/postgis-intro/indexing.html) for a deeper dive into spatial indexing. Kleppmann notes that Multi-column indexes don't work for spatial data. How does PostGIS index spatial data?

PostGIS indexes spatial data by using an R-tree. This essentially breaks up the data into rectangles that bound the features. These are much easier to work with compared to the original geometric features, as the query engine can reason about the intersection between different rectangles. 

## Column-Oriented Storage flips the mindset of relational databases by storing columns as vectors instead of rows as vectors. What are the pros and cons of such a database? What access patterns benefit the most from this change?

Pros:
- Can query columns from a row without having to load the entire row from disk (reduces volume of data that needs to be loaded from disk)
- Makes better use of the CPU, due to compression / vectorized processing.

Cons:
- Writes are more difficult, can't update in place since order must be maintained. 

Analytic workloads benefit the most from this change. They are typically read heavy and require aggregations across a ton of rows but only a few columns.


## What is a Materialized View? What are some queries that it would speed up? What are the costs of materialized views compared to normal views?

A materialized view is a table-like object on disk that holds precomputed results to common queries. It is most useful for speeding up expensive aggregation queries since the result has already been precomputed for you.
 
The main cost is that when data is changed, a materialized view needs to be updated. This makes writes more expensive.
