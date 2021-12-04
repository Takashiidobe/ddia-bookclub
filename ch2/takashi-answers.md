# Chapter 2 Questions

## Explain NoSQL and Relational Databases in your own words. What are their strengths? What are their weaknesses?

NoSQL databases look at storing data in a Not Only Relational fashion.
This can involve storing tuples as Key-Value pairs, Documents, or
Graphs. By Shirking the Relational model, NoSQL Databases can get higher
performance, and be less complicated than Relational Databases for less
relational data.

SQL Databases store tuples in a Relational fashion (as tables). They can
be Row-Based (Postgres, MySQL, Sqlite) or Columnar (Redshift). They
support the ANSI SQL standard for querying their data (mostly) and
support features such as joins, triggers, transactions, and sharding. SQL
Databases are well understood for storing data.

## There are three types of NoSQL databases (KV, Document, Graph). Explain each in your own words and some of the strengths and weaknesses of each.

### Key-Value

A key-value database stores its data in a way where it can be queried by
a primary key (the key in Key-Value).

#### Strengths

- Key-Value databases are simple to understand (it's just a hashmap)
- Fast.

#### Weaknesses

- Loses simplicity when modeling relational data
- Indexing on more than one field (a possibility for more complicated
  schemas) incurs a heavy performance penalty.

### Document

A document database stores its data in some document format (JSON, XML).

#### Strengths

- No schema?

#### Weaknesses

- Adds complication to the application layer (since documents must be
  versioned for change).
- Indexing multiple fields is still expensive

### Graph

A graph database stores its data in a graph, of nodes and edges.

#### Strengths

- Allows for modeling graphs in a simple way.
- Declarative query/logic language for querying graphs.

#### Weaknesses

- Poor standard query language (Cypher as standard?)
- Data may be poorly laid out, leading in poor performance
- Transactions are hard (if not impossible) to support with reasonable
  performance.

## What was your favorite query language this chapter? Why? Explore a query language you haven't tried before and try to explain its pros and cons.

Honorable Mention: The DOM, because I got my first job making websites with jQuery.

Actual Answer: Datalog

I thought Datalog was an interesting logic based language (like prolog
and erlang) for modeling data. You define a set of rules for your
contract and the runtime can verify that your logic is sound before
making the query. It is also used in my favorite database, datomic, as
an easier alternative to SQL.

In Datalog, a set of facts is defined (much like defining variables in
functional programming, these cannot change).

Then, a set of rules are defined, from which new facts can be derived
from the first set of facts.

Then, queries are built, which return answers for questions based on the
facts provided.

### Pros

- More declarative and logic based than SQL.
- Facts always hold, assuming they are side effect free.
- Simple to understand.

### Cons

- Logic programming can be clunky to get started with.
- Unpopular, and not as standardized as ANSI SQL

## Assume your next project is to create a Social Network. How would you store the data, and why?

I would store the data in a graph database (Neo4j), due to the data
being modeled by graphs of nodes and edges. To query the data, cypher or
SPARQL would be easier than using WITH_RECURSIVE in ANSI SQL. While it's
true that relational databases can be used to store graphical data, it
can be hard to reason about otherwise.

## Mapreduce is the first example of a distributed batch processing paradigm. Give a short summary of it. How would you improve it? What does it do well? What are some things you dislike about it?

I think MapReduce is a reasonable paradigm: It is a batch processing
paradigm that involves three steps for processing large quantities of
data:

Map, which applies a function to all the sets of data.
Sort, which fans out and sends data to various machines to compute
Reduce, which takes a function from the user and the result from all the
machines and combines them into one result, which is then returned.

The killer feature of MapReduce is that it abstracts away the underlying
hardware by providing retry and abort support throughout a cluster of
machines connected by a network. Basically, the Sort function is
transparent to the user, which is why it is called `MapReduce`, since
those are the only two things the user has to worry about.

MapReduce does not fit all use cases: Since it is a batch processing
algorithm, it works well if all the data is provided to it, but poorer
for real-time processing, where the dataset may change incrementally.

If the data is heavily relational, joins are not
natively supported, so MapReduce is a poor fit.

If the data requires multiple stateful operations, then MapReduce will
be slower than other paradigms, since it will schedule each stateful
operation as a job, instead of incrementally adopting mutated data.

## It is said that NoSQL databases do not require a schema up front. Is this true? Conversely, is it possible to change the schemas of SQL databases after creation? If so, what are the caveats?

I believe that NoSQL databases don't need a schema up front, but this is
only true until the first change in the schema. Since in production,
changes need to be made to schemas, this falls apart. With relational
databases, the schema is validated at the data layer. With NoSQL
databases, the schema is validated at the application layer -- this
means taking complication from the database and putting it in the
application. For example, if there's code that assumes the existence of
a column, but that column was recently added, the application code must
do a null check for this column and move forward in a sane fashion if
the data does not exist. This may require schema versioning, which is
error-prone in application programming.

For SQL DBs, it is possible to change the schemas of SQL databases after
creation with a migration. It can be painful on DBs with multiple
writers, but relational databases allow for ALTER tables to be executed
in a transaction in isolation, which can reduce some of that pain.
