# Chapter 2 Questions

## Explain NoSQL and Relational Databases in your own words. What are their strengths? What are their weaknesses?

Relational Databases are DBs that split your data into relations or Tables and joined with each other.  These DBs are very common and popular, and queried with a ANSI defined language called SQL.  They were superior at the time for both OLTP and OLAP workloads compared to alternatives. Some of the weaknesses include, difficulty in modifying the schema once it is in place, scalability, and complexity in querying data that isn't represented well in relations. 

NoSQL is name for a collection of modern dbs that implement a non relational approach to their data store and querying.
NoSQL Databases were created to address some concerns with scalability for high write and very large datasets, simplified queries for data that is not well represented by relational DBs, and a desire for less restrictive DB schemas.  


## There are three types of NoSQL databases (KV, Document, Graph). Explain each in your own words and some of the strengths and weaknesses of each.

>KV is for Key Value pairs, similar to a dictionary like redis and memcached.

**Pros**
- Fast
- Simple

**Cons**
- Not good for complex data

>Document is like MongoDB to share documents, which are like KV but the V is structured data.

**Pros**

- Flexible Schema
- Better Locality ( don't need to do joins for data in the document )

**Cons**

- Difficult to represent Many to Many relationships, as everything is nested in the document.
- Can represent joins with multiple queries which can be expensive.

>Graph is like neo4j for graph relations.  Graph relationships make it easy to represent data that has many to many graph relationships.

**Pros**

- Easy to represent Many to Many relationships in queries with Cypher as opposed to long SQL with many joins.

**Cons**

- Not good for Transactional workloads.
- Issues scaling write throughput


## What was your favorite query language this chapter? Why? Explore a query language you haven't tried before and try to explain its pros and cons.

Cypher.  Reading the examples of SQL vs Cypher was very interesting, as I have definitely been very annoyed in the past when having to attempt to write complex SQL queries joining many tables.  Cypher greatly simplifies this usecase with an easy to use declarative query language.  The only CON I can see is that it is only used for Neo4j and maybe a few other niche databases.

## Assume your next project is to create a Social Network. How would you store the data, and why?

A GraphDB like Neo4j.  I would store using graphs as relationships within a Social Network can be represented as graphs. 

Queries on relationships between people, would be very simplified using cypher rather than sql.

## Mapreduce is the first example of a distributed batch processing paradigm. Give a short summary of it. How would you improve it? What does it do well? What are some things you dislike about it?

MapReduce is a programming model for collecting and filtering data in large batches.  
I don't think I have anything I would improve mapreduce by.  I like how powerful it is, and how it is easy to write functions to call code and libraries for complex operations.
I dislike that it is through batches, this makes it weak for usecases where you want closer to real time processing.

## It is said that NoSQL databases do not require a schema up front. Is this true? Conversely, is it possible to change the schemas of SQL databases after creation? If so, what are the caveats?

Yes, it is true. NoSQL Databases generally do not require you to define all of the columns and their associated datatypes upfront.

Yes, it is possible to change the schema of a SQL database after creation via schema migration, but it is an annoying process.

Some issues you may run into include

* Corrupt data that was written by old versions of the software and not cleaned properly
* Implied dependencies in the data which no one knows about anymore
* Mistakes in assumptions how data should be migrated
* Operational overhead of setting your application up to work with the old and new schemas, to do the migration without downtime.
