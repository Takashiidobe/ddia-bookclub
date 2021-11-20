# Chapter 3 Questions

## Hash indexes are the first index we learn of in this chapter. What are its strengths? What are its limitations? If you've ever used a hash index, what were some of its limitations in practice?

-- YOUR ANSWER HERE --

## Read [Re-introducing Hash Indexes in PostgreSQL](https://hakibenita.com/postgresql-hash-index) for a deeper dive into hash indexes in Postgres. What are some of the differences between this writeup and DDIA? What struck you as some strengths of hash indexes when you read about it?

-- YOUR ANSWER HERE --

## What are the strengths and weaknesses of SSTables? For more of a in-depth view, read this answer on [SO](https://stackoverflow.com/questions/8651346/sorted-string-table-sstable-or-b-tree-for-a-database-index/8654903#8654903).

-- YOUR ANSWER HERE --

## B-trees are the most common index in databases. What makes them so popular? What are some of their tradeoffs when compared to hash indexes and SSTables? What are some other applications of B-trees that you've come across?

-- YOUR ANSWER HERE --

## Some other indexes are Partial (indexing only some values) and multi-column indexes. What are some of the strengths of those indexes? What are their weaknesses? Read [this](https://hakibenita.com/postgresql-unused-index-size) for a story where an index was compacted to save 20GB.

-- YOUR ANSWER HERE --

## B-trees are the most common index in databases. What makes them so popular? What are some of their tradeoffs when compared to hash indexes and SSTables? Likewise, what are some pain points of B-trees?

-- YOUR ANSWER HERE --

## What are some of the pain points of working with indexes in your experience? Are they a silver bullet? Can you recall a time when using an index was unintuitive?

-- YOUR ANSWER HERE --

## Choose an index that wasn't covered by this chapter and briefly explain its characteristics. What is it used for, and what are similar indexes that can be used for the same use case?

-- YOUR ANSWER HERE --


## Read [Introduction to PostGIS](http://postgis.net/workshops/postgis-intro/indexing.html) for a deeper dive into spatial indexing. Kleppmann notes that Multi-column indexes don't work for spatial data. How does PostGIS index spatial data?

-- YOUR ANSWER HERE --

## Column-Oriented Storage flips the mindset of relational databases by storing columns as vectors instead of rows as vectors. What are the pros and cons of such a database? What access patterns benefit the most from this change?

-- YOUR ANSWER HERE --

## What is a Materialized View? What are some queries that it would speed up? What are the costs of materialized views compared to normal views?

-- YOUR ANSWER HERE --
