# Chapter 2 Questions

## Explain NoSQL and Relational Databases in your own words. What are their strengths? What are their weaknesses?

NoSQL stands for not only SQL that use a non relational model for storage.
NoSQL databases were created to solve some of the weaknesses of Relational 
databases and are known for their high scalability allowing for usage of large 
datasets and very high write throughput, support of specialized queries 
operations that are not supported by relational databases and support for more 
dynamic and expressive data models than Relational Databases

A relational database is a database that organizes and stores data into 
relations that can be queried using the SQL language.
The data in a relational database is organized into relations that are known as 
tables, where a relation stored inside of a table is a collection of tuples 
called rows, all of these descriptions of data put together are known as the 
database schema. Some downsides of relational databases is their poor ability 
to scale when using very large data sets or when high write throughput is 
needed, as well as their poor ability to change the schema of data. Despite 
that though relational databases are among the most popular types of databases 
especially when people need to store and query data that has a rigid structure. 


## There are three types of NoSQL databases (KV, Document, Graph). Explain each in your own words and some of the strengths and weaknesses of each.


### Document Database
A document database is a database that stores information in structured data 
formats and is a subset of KV databases. A key can be used to retrieve/link to 
the document of information, and the information returned has no guarantee that 
it will follow the structured data format of any other document.
__Pros__
* schema flexibility 
* better performance due to locality

__Cons__
* poor support for joins
* struggles with many to one and many to many relationships

### KV Database
A KV database is a key value store similar to a dictionary or 
hashtable(ex. redis)

__Pros__
* simple
* fast

__Cons__
* scales poorly
* unable to model complex data

### Graph Database

A graph database is a way to store vertices and edges that model many to many 
or many to one relationships

__Pros__
* can easily extend data model
* can represent many to many relationships very well

__Cons__
* query language is lacking compared to others(ex cypher)

## What was your favorite query language this chapter? Why? Explore a query language you haven't tried before and try to explain its pros and cons.

My favorite query language this chapter was cypher. I liked this language 
because it shows how a query language can be used on a graph database to query
many to many connections in a readable manner despite the slightly more verbose
syntax that it uses. Some of the pros of cypher are that it is a declarative 
language letting a query optimizer speed up its operations, it also has very 
explicit syntax for creating and querying vertices and edges. One downside of
cypher is also the syntax. In other declarative languages querying had been
made exceedingly concise and easy with the correct syntax, this is
unfortunately not the case in cypher which leads to the extra verbosity being
both a pro and con in my opinion.


## Assume your next project is to create a Social Network. How would you store the data, and why?

If I were tasked with a project to create a social network, then my immediate
answer would be to implement the data store in a graph database. The reasoning
behind this is because of the unique ability of graph databases to model many
to many relationships exceedingly well. Additionally because of how adaptive
social media has become in the modern world the ability to store data in key
value pairs on each vertex allows for easy data model changes whenever data
has to be added or removed


## Mapreduce is the first example of a distributed batch processing paradigm. Give a short summary of it. How would you improve it? What does it do well? What are some things you dislike about it?

Mapreduce allows for an operation or operations to be applied across large
quantities of data in parallel. This allows for data processing in large 
batches that will scale a cross multiple pieces of hardware for increased
speed. One of the downsides of mapreduce is that because it runs in batches it
is does not provide any results back in real time. If I could improve one thing
about mapreduce it would be to extend the current functionality so it applies
the map operation in realtime to all new data allowing for real time
processing(though at that point it might be time to question if mapreduce is
the right tool for the job).


## It is said that NoSQL databases do not require a schema up front. Is this true? Conversely, is it possible to change the schemas of SQL databases after creation? If so, what are the caveats?

Most noSQL databases do not require a schema up front but it is entirely up to
the implementation of the database on whether or not it enforces the schema on 
write in the database(explicit) or handles it on read in the 
application(implicitly). It is possible to change the schemas of databases
after they have been created but some of the caveats are that existing data has
to be modified to support the new schema so the database knows how to handles
interactions with data that was added to the database before the schema changes
were implemented
