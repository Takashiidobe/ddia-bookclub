# Chapter 3 Questions

## Hash indexes are the first index we learn of in this chapter. What are its strengths? What are its limitations? If you've ever used a hash index, what were some of its limitations in practice?

Hash indexes are easy to implement yourself. THe concept is pretty straight forward. We take a byte in a file, look up where to find stuff. Its pretty staightforward, its just limited by memory, makes writing to a db more expensive, and pain.

## Read [Re-introducing Hash Indexes in PostgreSQL](https://hakibenita.com/postgresql-hash-index) for a deeper dive into hash indexes in Postgres. What are some of the differences between this writeup and DDIA? What struck you as some strengths of hash indexes when you read about it?

After reading this I want to implement some of the index split functionality of hash indexing into our codebase. I think it would help with our distrubuted-dockerized system in scaling. If you split the index it also makes the problems that arise through hash collisions much less troublesome. More buckets means less collisions inside the same buckets. They are also much smaller in size than b-trees. Which makes sense. I learned that sometimes good tech is overlooked because existing tech doesnt exist for easy integration.

## What are the strengths and weaknesses of SSTables? For more of a in-depth view, read this answer on [SO](https://stackoverflow.com/questions/8651346/sorted-string-table-sstable-or-b-tree-for-a-database-index/8654903#8654903).

The SS table aka the sorted string table. After reading the article i am much more interested in fractal trees. An assTable provides an ordered immutable map from keys to values wherre bot hkeys and values are arbitrary byte stribngs . Operations are provided to look up the value associated with a specified key, and iterate over all key,value pairs in a specific key range. Internally each SSTable contains a block index. which can be used to look up the individual block.

## B-trees are the most common index in databases. What makes them so popular? What are some of their tradeoffs when compared to hash indexes and SSTables? What are some other applications of B-trees that you've come across?

B trees have more overhead and take longer to insert that something like a hash index, but as a result select speed is much higher. When implementing my own query language i found that as you distribute b-trees are very easy to work with.MOre memory is used as well. The leaf nodes for large datasets are often stored on disk as a result which increases load times sometimes. 

## Some other indexes are Partial (indexing only some values) and multi-column indexes. What are some of the strengths of those indexes? What are their weaknesses? Read [this](https://hakibenita.com/postgresql-unused-index-size) for a story where an index was compacted to save 20GB.

Sometimes indexes are a pre optimization. you anticipate some data may be used, but it gets used less frequenet than you expect, or oyu just wanna look up one very speific thing. In the base of Partial indexes we sometimes only want one field in our DB. 

## What are some of the pain points of working with indexes in your experience? Are they a silver bullet? Can you recall a time when using an index was unintuitive?

Indexes can rapidly increase in size depending on how they are indexing. I didnt realize how much space one of ours was taking at work. I do think you need to carefully think if you truely need an index to speed up reads before adding it if space or writes are something that require perfection. 

## Choose an index that wasn't covered by this chapter and briefly explain its characteristics. What is it used for, and what are similar indexes that can be used for the same use case?

## Lets talk about elasticsearch, a search index. More specificly how indexing works in elastisearch works.
To better understand how this index works lets cover some basic concepts of how it organizes data in its backend components.

### There are 3 logical components to elasticsearch
- Documents
- Indices
- The inverted Index

 The interesting one we want to talk about here is the inverted index. An index in elasticsearch is called an inverted index, which is the mechanism by which all search engies work. 

You might be familiar with the Terms query in elasticsearch. It turns out this works by splitting up the document up into individual search terms then maps each search term to the documents that those search terms occur with. 

By using distributed inverted indices, ES can quickly find the best matches for full text searches even on large scale datasets.

There is an inverted index for each individual clusters in elasticsearch. Thats because there will be an inverted index for each full-text field per index. So if you hve an index containing documents that contain five full text fields you will have 5 inverted indices, which allows for really fast reads, but really slow writes and updates.


For example lets say we want to store the text for the following titles: “The Best Pasta Recipe with Pesto” and “Delicious Pasta Carbonara Recipe.

We can index each indivdual term

Term | Document #1 | Document #2
--------------------------------
best | X | Not in this document|
carbonara | NF | X |
delicious | NF | X |
pasta | X | NF |
-------------------------------

Imagine we have a hash that says we can find term in the following documents.
This way we have instant access to which documents contain a given term. So we just ask elasticsearch for the given term, it just returns the terms it has already indexed. 

The inverted index can also be used to apply stemming of words and synomyms. This would also be applied to the inverted index, but i wanted to keep the examples simple here.

## Read [Introduction to PostGIS](http://postgis.net/workshops/postgis-intro/indexing.html) for a deeper dive into spatial indexing. Kleppmann notes that Multi-column indexes don't work for spatial data. How does PostGIS index spatial data?

Spatial indexing uses this idea of an R-Tree. These R trees allow us to break up data into rectangles recursively(Smaller and smaller rectangles inside that rect as well). It is a self tuning index structure that automatically handles variable data density, different amounts of overlap and object size.

## Column-Oriented Storage flips the mindset of relational databases by storing columns as vectors instead of rows as vectors. What are the pros and cons of such a database? What access patterns benefit the most from this change?

In some cases we dont deal with individual posts or groups of data that can be mdeled. Column oritented database is really great for represetning timeseries data. Why you may ask? Well it has to deal with regular aggregate operations over a small number of columns.  If we are only concerned with specific columns like finding the min and finding the max in one column of data we can perform aggregations really quickly. 

## What is a Materialized View? What are some queries that it would speed up? What are the costs of materialized views compared to normal views?

### Views
- A logical and virtual copy of a table that is created by executing a select query
- this view isnty sotred anywere on disk
- every-time a query needs to be executed when certain data is needed.
- If we dont save state and always pull directly from the database we never have an old version of the data.

### Materialized View
- Its basically a cached result of a sleect query. That specific query can be stored in the table or disk. 
