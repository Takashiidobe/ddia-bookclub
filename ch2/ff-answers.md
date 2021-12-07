# Chapter 2 Questions

## Explain NoSQL and Relational Databases in your own words. What are their strengths? What are their weaknesses?

#### Relational
Relational databases are a data model implementation in which data relationships are organized into collections, typically known as tables and rows in SQL. This model was derived from needs in the business world, requiring relatively consistent structure across different use cases, and its underlying goal was to abstract away the underlying implementation of the database for developers. 

##### Strengths
-Access to data is straightforward due to its format
-Query optimization (ideal query execution) rarely required by developer
-New querying methods and features easy to implement
-Query optimizers, written once, can improve the experience of accessing the DB for all applications that come after

##### Weaknesses
-Query optimizers for RDBs require significant time and resources to develop
-Model rarely reflects format of the data on application side

#### NoSQL
NoSQL is one of the few competing models to relational that has seen successful adoption. Rather than organize data into tables and rows, NoSQL's document model simply organizes data in an unstructured manner, allowing for multiple data representations within the same database.

##### Strengths
-Easier scalability features
-Better alignment and reception by FOSS
-Unique querying operations not always found in RDBs
-Relatively greater flexibility and extensibility

##### Weaknesses
-Difficult to handle many-to-many relationships (i.e. joins), pushing such logic into application code
-Cannot refer to nested items within documents
-The larger the document in the database, the more expensive it is to load smaller portions of it

## There are three types of NoSQL databases (KV, Document, Graph). Explain each in your own words and some of the strengths and weaknesses of each.

#### Key-Value
Key-Value is a pattern for storing data in collections, in which a piece of data is associated to a key unique to that collection. This manifests as data structures commonly known as hash tables, maps, or dictionaries.

##### Strengths
-Fast read and write capabilities
-Structure more closely reflects application side use case
-Lower memory usage due to lack of pre-determined inputs and parameters

##### Weaknesses
-The more the data model changes, the more expensive look ups into the DB become
-Anything beyond simple key searches requires a scan of the entire database

#### Document
Document DBs share similar features of KVDBs but values are kept in a document format. This allows any type of document to be added to the database so long as the DB can read and interpret the file.

##### Strengths
-Not limited to key searches like KVDBs
-Transparent data removes the need for data translation while allowing for engine optimizations

##### Weaknesses
-Difficult to enforce consistency checks on data, resulting in unexpected query results
-Any modified data that involves more than one document requires greater than a single query

#### Graph
The graph model involves storing vertex and edge data in relational tables with no pre-existing definition, similar to KV and Doc DBs. The emphasis is on the relationships between data as much as the data themselves.  

##### Strengths
-Structure ideal for navigating deep interconnected diverse data sets
-Speed not constrained by amount of data, but rather the number of connections defined between them
-Data can be stored in manner that more closely reflects the use case application-side

##### Weaknesses
-Due to focus on relationships, scalability an issue the more complex relationships become
-Flexibility can impact performance of simpler queries

## What was your favorite query language this chapter? Why? Explore a query language you haven't tried before and try to explain its pros and cons.

SQL because I know it already :|

But seriously, I appreciated Cypher namely because it helps illustrate the logic of Graph DBs rather well, and the chapter showed how the Neo4j DB engine might navigate the data requested without putting the burden on the developer.

DMX (or Data Mining Extensions) is a query language similar to SQL supported by large orgs like Microsoft and Oracle, operating on data mining models instead of relational tables.

#### Pros
-Ideal for writing predictive queries
-Linguistic similarities to SQL make it easier to adopt and adapt

#### Cons
-Limited use cases due to the language's focus
-Much of language's querying functionality can be executed more efficiently through MSFT's GUI

## Assume your next project is to create a Social Network. How would you store the data, and why?

I would use a graph database like Neo4j, storing users as nodes and their connections and relationships to one another as edges. Social networks, by their nature, naturally align with the graph model, so it would be the most logical format to implement. 

## Mapreduce is the first example of a distributed batch processing paradigm. Give a short summary of it. How would you improve it? What does it do well? What are some things you dislike about it?

Mapreduce is a logical paradigm for executing parallel, distributed processes in queries, implemented in either declarative or imperative languages. The paradigm is good at integrating into queries easily so the query optimizer can continue to do its job. The downside is both mapping and reducing functions are strict about their inputs, so they only touch the data defined for them ahead of time.

Were I smart enough to improve this, I would attempt to anonymize the necessary inputs and create a more variable approach to each function. I would also combine the calls into a single parent function to reduce the necessity to call these separately. It is unclear if a query optimizer would be affected this change, so forgive the ignorance.

## It is said that NoSQL databases do not require a schema up front. Is this true? Conversely, is it possible to change the schemas of SQL databases after creation? If so, what are the caveats?

NoSQL databases using documents are schemaless in the sense that the database does not enforce one, but there is still a schema implicit to whatever structure that the stored data comes in. So the term may be a misnomer.

SQL databases can change their schemas through an ALTER TABLE statement, though MySQL ends up copying the entire table to a new one.
