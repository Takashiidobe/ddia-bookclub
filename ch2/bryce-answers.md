# Chapter 2 Questions

## Explain NoSQL and Relational Databases in your own words. What are their strengths? What are their weaknesses?

### Relational Databases.
Relational databases are really useful when you have a lot of relations/relationships between the data. :

## There are three types of NoSQL databases (KV, Document, Graph). Explain each in your own words and some of the strengths and weaknesses of each.

### Key Value Like Redis
a store of pairwise values. Useful for caching and building small graphs/tries very efficiently
#### Pros of redis
- Fast reads
- Fast Writes

#### COns of REDIS
- That shit be breaking not very persistent
- ALways has to be on. 

### Document database
#### Pros
- Dynamic Data Storage
- Quick Searching 
- Schema-less
- No FK
- Faster Creation

#### Cons
- Security
- Atomic Weaknessess
- SOme dont have native joins so that becomes the responsiblity of your application layer.
### Graph Databases
- This is my favorite type of database because it makes my leetcoding useful.
#### Pros
- Many to many relationships are represented really well in this model. 
## What was your favorite query language this chapter? Why? Explore a query language you haven't tried before and try to explain its pros and cons.
- SQL. I like SQL for the same reasons i like python. Its very easy to comprehend. Even if you dont know python, you can read python code and make sense of it. With SQL its the same thing.
### Exploration of A query language
#### GRAPHQL

##### Pros
- Dictate exactly what you want from the server. Reading in GRAPHQL is elegant. In a restful archetecture we would have to retrieve the entirety of objects whereas graphql we can just grab the parts we want! iTS REALLY AWESOME.
- Retrieve many resources in a single Request Similar to elasticsearch
- Strong Type( May be a disadvantage depending on who you ask)

##### COns
- Queries allows return 200 regardless if the query is successful. 
- Error handling is a lot more complicated since the structure of data can be whatever it dreams of being.
- COMPLEXITY. If your data is consistent its best to stick with a RESTFUL archetecture. SOmetimes complexity is required but in my short bit of expirence its a bit of a pain.


## Assume your next project is to create a Social Network. How would you store the data, and why?

- We need to ask some clarifying questions. First off is the network going to be undirected or directed? If its undirected i would use a graph database like neo4j.If its directed we could use a graph database or we could just use relational nodes with reference pointers to other nodes in the system. :wq!@
## Mapreduce is the first example of a distributed batch processing paradigm. Give a short summary of it. How would you improve it? What does it do well? What are some things you dislike about it?

MapReduce is a framework that allows the user to write code that is executed on multiple nodes without having to worry about fault tolerance, reliability, synchronization or availability. 

## It is said that NoSQL databases do not require a schema up front. Is this true? Conversely, is it possible to change the schemas of SQL databases after creation? If so, what are the caveats?

No SQL databases are a lot more dynamic in nature. Basically everything that isnt Defined as Relational is defined as NOSQL. There are dynamic mapping document models that do not require a schema as well as document dtabases with more strict schema requirements. You can alter the schemas of SQL databases with ALTER but if you have a large quantity of data in the database you will have to modify it to comply with a new schema so its a very expensive operation to change the migration schema 
