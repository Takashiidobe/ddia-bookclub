# Chapter 2 Questions

## Explain NoSQL and Relational Databases in your own words. What are their strengths? What are their weaknesses?

Relational databases are a family of databases that follow the relational model, which involves storing the data in tables, association tables for relationships, foreign keys, constraints, and schemas. Person, woman, man, camera, TV. To my understanding, a relational database follows a set of normalization rules and a defined schema so it's a more specific meme like ABGs. By contrast, NoSQL is a less specific meme like hipsters. It refers to any non-mainstream (not-relational) databases. Generally, I think people are most familiar with document databases (MongoDB, CouchDB, etc.), but there's also key-value stores and graphs, mentioned in the subsequent question. Relational DBs have better support for joins, many-to-one, and many-to-many relationships than K-V or Document DBs.

It's hard to compare the two, since one of them is roughly defined by not being the first, but the advantage of a relational database was data normalization, ACIDity, relation modeling, and, in my opinion but not mentioned in the book, better hygiene. The tradeoff is less flexibility (have to update schema and migrate data if the model changes) and less speed (due to low pH guarantees). Moreover, there is impedance mismatch (cost of translating memes) when converting from an application object to a relational database object, which NoSQL reduces by not having as many restrictions around how an object is saved.

Relational DBs tend to be focused on availability and consistency while NoSQL databases place less emphasis on availability or consistency in exchange for partition tolerance. No CAP. This can alow NoSQL to scale for very high throughput or massive datasets.

## There are three types of NoSQL databases (KV, Document, Graph). Explain each in your own words and some of the strengths and weaknesses of each.

Key-value stores are when you just want to chuck some data somewhere and you just have the key. Good for caches and also for storing large amounts of data where you only plan to look up by key so you can just yeet data in. Bad if you want anything like indexing on parts of the value, if there are any relations between the values that need to be preserved, or if you want to perform joins.

Document database allows us to store data in a document using xml, json, or some binary format. The data representation is closer to how the object is represented in business logic and it can evolve quickly. It's best for objects that are loaded all at once and/or have one-to-many relationships since one of the document model's main advantages is locality. However, if an object isn't loaded all at once, then the improved locality is wasted. Moreover, if the data involves many-to-many or many-to-one relationships, then the relational model would fit better. Joins are not well supported and denormalizing the data puts more burden on the application to keep the data consistent. One can mitigate the need for joins by running multiple requests, but that also places more burden on the application when that logic would be better handled by a database engine. Also, updates can be more costly if they modify the size of the document so use cases where the object size can change drastically are not optimal.

Graph database is optimal for workloads where there many-to-many relationships and the data is highly connected. While relational dbs are alright to use in these cases, graph databases will tend to be more performant. However, graph databases should be used for primarily storing the connections between data. If I recall correctly, storing too much data in the node negatively impacts performance.

## What was your favorite query language this chapter? Why? Explore a query language you haven't tried before and try to explain its pros and cons.

SQL. I'm familiar with it. It's imperative and, as a result, convenient to work with. Cypher is interesting. It looks kind of straightforward, but I'm not familiar with it so perhaps it would be confusing to work with. However, in my opinion, graphs are difficult in general to work with conceptually.

## Assume your next project is to create a Social Network. How would you store the data, and why?

A social network is pretty complicated so I think polyglot persistence would apply here. We'd want multiple database types for different use cases.
Caching for the site would use a key-value store. User profiles, which are loaded all at once and remain around the same size, can be stored in a document db. The things a user might interact with such as groups, posts, and relationships with other users would be stored in a relational database or a graph database depending on the level of interconnectivity we'd expect or want to optimize for. Assets like images and videos would be kept in blob storage.

## Mapreduce is the first example of a distributed batch processing paradigm. Give a short summary of it. How would you improve it? What does it do well? What are some things you dislike about it?

Map reduce takes some kind of query function and applies it to multiple machines or batches of data. An improvement might be some way to coordinate between multiple machines while the query is executing or have it execute changes in the query (since it currently cannot modify the underlying data). It parallelizes well, but I suppose the issue with it is that it's sorta reinventing existing functionality in SQL.


## It is said that NoSQL databases do not require a schema up front. Is this true? Conversely, is it possible to change the schemas of SQL databases after creation? If so, what are the caveats?

Half true. NoSQL databases require no schema up front to write, but they 'enforce' or, at least, assume some schema when reading from the database. It is possible to alter schemas for SQL databases after creation and theoretically alter table is quick to execute, but you would need to run some sort of migration and there could be many potential issues from updating the database schema without properly updating the underlying data. Updating the schema should not be taken lightly (in NoSQL as well, but especially with respect to SQL since it can break queries).
