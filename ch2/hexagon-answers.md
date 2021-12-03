# Chapter 2 Questions

## Explain NoSQL and Relational Databases in your own words. What are their strengths? What are their weaknesses?

Relational databases use relations (tables) that consist of tuples (rows) of data. Relational databases are popular and easy to use with a query language called SQL. They work well with many-to-one and many-to-many relationships and support joins between tables. They also enforce schemas, which can make the structure of the database very clear. On the other hand, schemas can hinder evolvability, and some complicated data can be difficult to express in a traditional relational schema.

NoSQL is a general term that encompases distirbuted, non relational databases. There are many different kinds of NoSQL databases including document and key value DBs. NoSQL dbs are often considered to scale well, especially for writes. They often make it easier to query data that isn’t naturally represented in a relational schema. However, most NoSQL dbs have some drawbacks, like how document dbs can struggle with many-to-many relationships. 


## There are three types of NoSQL databases (KV, Document, Graph). Explain each in your own words and some of the strengths and weaknesses of each.

KV - basically like a big hashmap/dictionary. Very simple, but this simplicity can be a drawback for more complex data that isn’t easily represented through key value pairs.

Document - stores JSON-like nested structures. Very natural for one-to-many relationships. Also a nice locality property. The main drawback is that they can struggle to deal with many-to-one and many-to-many relationships and they don’t always support joins. The locality of documents in document dbs can also cause performance issues if the documents are too big, as any query of the data will require the db to fetch the entire large document, even when making a small update. 

Graph - Models all of the data as a graph: vertices connected by edges. Good data model for use cases where you know you will have a ton of many-to-many relationships, like a social network. They are good for evolvability - their flexibility makes them easy to modify as requirements change, since vertices / edges lack restrictions for what kind of data they can represent. One drawback is that relational style queries can be very awkward, so you may have to learn a new query language like Cypher or SPARQL. 

## What was your favorite query language this chapter? Why? Explore a query language you haven't tried before and try to explain its pros and cons.

My favorite query language is SQL due to its simplicity. I also appreciate the declarative nature: the fact that you can optimize the query engine without having to modify queries is good design imo. 

## Assume your next project is to create a Social Network. How would you store the data, and why?

I would use a graph data model like Neo4j. I think this is a natural model since there are a ton of many-to-many relationships with people as vertices and relationships as edges. Graphs can also be also good for evolvability if we want to add new connections beyond relationships (peoples hometown, occupation, etc.)

## Mapreduce is the first example of a distributed batch processing paradigm. Give a short summary of it. How would you improve it? What does it do well? What are some things you dislike about it?

MapReduce is a programming model for processing large amounts of data in bulk across multiple machines. I like that MapReduce allows you to write carefully coordinated JavaScript functions which are helpful for more advanced queries, but at the same time I prefer SQLs simpler syntax when possible. 

## It is said that NoSQL databases do not require a schema up front. Is this true? Conversely, is it possible to change the schemas of SQL databases after creation? If so, what are the caveats?

It’s true that many NoSQL databases don’t require an explicit schema, but often a structure is implied so it can be interpretected when data is read.

It is possible to change SQL schemas, but it can be laborious and require some planned downtime.
